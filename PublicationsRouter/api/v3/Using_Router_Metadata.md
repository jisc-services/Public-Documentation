# Using Router Metadata #

Jisc Publications Router provides Institutional Repositories and CRISs with article metadata (as well as associated articles).  See the [Outgoing Notification](./OutgoingNotification.md) page for a detailed description of the metadata that is provided.
 
Below we provide insight and guidance into the interpretation and use of certain of the metadata values.  It should be borne in mind that Router obtains information from a wide variety of sources and as a result there tends to be significant variation in the content and completeness of the metadata. 

## Licence details ##
Where Router has obtained details of licences applying to an article it will send these as an array (list) in the `licence_ref` element - the full possible structure is shown below. This section describes the use of each of the elements, any of which may be missing or empty.  
```json
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
```
### url ###
Where present the URL should be regarded as the definitive determinant of the licence applying to the article. It will be either:
* a Creative Commons licence URL
* a publisher's proprietary licence URL.

### start ###
The licence start date, format: YYYY-MM-DD.

If no value is present then it should be assumed that the licence is already in effect.

### title ###
A paragraph or phrase that describes the licence. For example, this could be several sentences provided by a publisher or a simple creative commons abbreviation such as `"cc by"`.

Where no `url` is provided then the value of this element may be relied upon to describe the licence. 
### type ###
A phrase that describes the licence type. For example it could say `"open access"` or it could be the same text supplied in the `title` element (above). 

### version ###
Alphanumeric string indicating version of the licence. For example `"v1.0"`. 

(This element is usually missing or empty).

### best ###

This is a boolean indicator (set using a PubRouter algorithm) that is intended to guide selection of an appropriate licence where a receiving system can only handle a single license. 

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

* Where any of the licences has a URL that corresponds to an open licence (either Creative Commons, or a publisher's proprietary open licence that PubRouter recognises as such) then the *best* licence is assessed as being the most recent active licence (i.e. no start date, or start date not in the future), or if none yet active then the open licence with the earliest future start date.

* Where none of the licence URLs is recognised as "open" by PubRouter, then the *best* licence is chosen from these using the same start date considerations as for open licences (see preceding paragraph).  In this circumstance, where there is more than one licence, then systems should place a relatively low level of confidence in PubRouter's assessment of which is *best*.

In all cases, **provision should be made for manual checks by the user**, along the lines indicated above.

This indicator is intended to help systems interpret the licence data; it is not meant for display to users.
