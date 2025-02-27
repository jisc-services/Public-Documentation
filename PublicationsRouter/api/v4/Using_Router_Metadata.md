# Using Router Metadata #

Jisc Publications Router provides Institutional Repositories and CRISs with article metadata (as well as associated article files).  The [Outgoing Notification](./OutgoingNotification.md) page provides a detailed description of the metadata.
 
It should be borne in mind that Router obtains information from a wide variety of sources and as a result there tends to be significant variation in the metadata.

Below we provide insight and guidance into the interpretation and use of the following key metadata elements:
* Article version
* Embargo  
* Licences.

### Note on displaying Embargo and Licence details
CRIS and repositories frequently associate and display embargo and licence details at the  file (e.g. article)  level.  

However, a substantial number of notification records that Publications Router provides will be metadata only (for example those from PubMed, Crossref, and Elsevier).  In these cases, where there is no associated file, it is important that any licence or embargo information is still captured and displayed, for example in a note or additional information field.   

## Article version ##
JSON element: `metadata.article.version` 
```json
"metadata": {
    "article": {
        "version": "<Article version e.g. VoR>",
        },
}
```
Where this value is provided it indicates the version of the article to which the rest of the metadata applies.  It will have one of these values:
* AM - Accepted manuscript
* P - Proof
* VoR - Version of record 
* CVoR - Corrected version of record
* EVoR - Enhanced version of record
* C/EVoR - Corrected or enhanced version of record.

(For further information see the NISO working group recommendations https://groups.niso.org/publications/rp/RP-8-2008.pdf).  

Note that this value is frequently missing.

## Embargo ##
Where router has captured embargo information this will be provided in the structure shown below.  Note that fields may be missing; for example, it may contain just an `end` date, or it may contain just `start` and `duration` fields.

Where `end` is absent, but `start` and `duration` are present, we recommend that you calculate the end date for display to users.

Where the Article version element is provided (see above) then the embargo details will relate only to that version.

JSON element: `metadata.embargo` 
```json
"embargo": {
    "start": "<embargo start date, format: YYYY-MM-DD>",
    "end": "<embargo end date, format: YYYY-MM-DD>",
    "duration": "<embargo duration in months>"
},
```

### start ###
The embargo start date, provided in YYYY-MM-DD format.
### end ###
The embargo end date, provided in YYYY-MM-DD format.
### duration ###
The embargo duration in months.

## Licence details ##
Where Router has obtained details of licences applying to an article it will send these as an array (list) in the `licence_ref` element - the full possible structure is shown below. 

This section describes the use of each of the elements, any of which may be missing or empty.

Where the *Article version* element is provided (see above) then the supplied licence details will relate only to that version.   

JSON element: `metadata.license_ref` 
```json
"metadata": {
    "license_ref": [
        {
        "url": "<url>",
        "start": "<Date licence starts (YYYY-MM-DD format)>",
        "title": "<name of licence>",
        "type": "<type>", 
        "version": "<licence version; for example: 4.0>",
        "best": "<Boolean indicates the optimum open licence - will be true for maximum of ONE licence in the array>"
        }
    ],
}
```

### url ###
Where present, the URL should be regarded as the definitive determinant of the licence applying to the article. It will be either:
* a Creative Commons licence URL
* a publisher's proprietary licence URL.

### start ###
The licence start date, format: YYYY-MM-DD.  

Where present this date should always be displayed to users.

If no value is present then it should be assumed that the licence is already in effect.

### title ###
A paragraph or phrase that describes the licence. For example, this could be several sentences provided by a publisher or a simple creative commons abbreviation such as `cc by`.

Where ***url*** is missing then the value of this element may be relied upon to describe the licence and should therefore be displayed to the end user. 

Where a ***url*** is present, then the value of this element may be used to provide a description of the licence.

### type ###
A phrase that describes the licence type. For example it could say `open access` or it could be the same text supplied in the ***title*** element (above). 

### version ###
Alphanumeric string indicating version of the licence. For example `v1.0`. NOTE: This element has nothing to do with the *Article Version*.

(This element is usually missing or empty).

### best ###

This is a boolean indicator (set using a Publications Router algorithm) that is intended to guide selection of an appropriate licence where a receiving system can only handle a single licence. 

This indicator will be set to `true` for 1 licence in the array (at most).

See the following section for more details. 

## Receiving system limitations - implications for processing licences ## 

#### Systems that can handle multiple licences
If your system system caters for multiple licences along the lines of the [NISO ALI standard](https://www.niso.org/schemas/ali/1.0) then you should capture all the licences and their start dates and display them accordingly (the *best* licence indicator should be ignored).

#### Systems that can handle only one licence
If your system can capture only one licence for a given version of an article, then the *best* licence indicator (metadata.license_ref.best) may be of use, but 
**it should be used with care**. 

It indicates Router's assessment of which licence is most likely to represent the post-embargo licence (if an embargo applies), or one that has subsequently come into effect. In many cases this will be reliable but in others it could be wrong, so you should guide your users accordingly. 

If you make use of it to capture just one licence into your system's licence field(s), we **strongly advise that you also capture as a text string the complete list of licences and their start dates** into a field that your users can see for manual checking purposes. This string should be constructed along the following lines:

>"Licence for [metadata.article.version] version of this article starting on [metadata.license_ref.start]: [metadata.license_ref.url OR metadata.license_ref.title]; ...[[repeat for each licence]]"

Example: 
>*Licence for VoR version of this article starting on 20-06-2019: http://creativecommons.org/licenses/by-nc/3.0/*

#### "Best" licence indicator detail and algorithm

Each object in the **license_ref** array has a Boolean element named ***best***.  At most, only one licence object in the array will have *best* set to *true*.  NB It is possible for all licence objects to have *best* set to *false* - this occurs where there is more than one licence and none has a URL.  

Router's assessment **applies on the date that the notification is retrieved** and is determined by the following algorithm:

* Where any of the licences has a URL that corresponds to an open licence (either Creative Commons, or a publisher's proprietary open licence that Publications Router recognises as such) then the *best* licence is assessed as being the most recent active licence (i.e. no start date, or start date not in the future), or if none yet active then the open licence with the earliest future start date.

* Where none of the licence URLs is recognised as "open" by Publications Router, then the *best* licence is chosen from these using the same start date considerations as for open licences (see preceding paragraph).  In this circumstance, where there is more than one licence, then systems should place a relatively low level of confidence in Publications Router's assessment of which is *best*.

In all cases, **provision should be made for manual checks by the user**, along the lines indicated above.

This indicator is intended to help systems interpret the licence data; it is not meant for display to users.
