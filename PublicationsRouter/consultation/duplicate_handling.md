# Duplicate handling consultation

Following Router's introduction of basic duplicate filtering options (in June 2021) we are considering additional features:

1. Sending duplicate notifications by email
2. Additional duplicate filter options or a checkbox menu approach, as described below.

We would welcome your feedback on these using the mechanism described at the bottom of this page. 
## Duplicates by email
You would have the option of providing one or more email addresses to which duplicate notifications would be sent *instead* of depositing them directly into your repository.  

There would be a new *Duplicates by Email* field on your Router account page (in the *Manage duplicates* panel) for this purpose.

You would receive an email for each duplicate notification that is matched to your account. This would contain:
* A brief summary of the differences between the duplicate notification and
  * The original notification
  * A cumulative  view of all previous notifications (where this is not the first duplicate)
* All the metadata (with additional metadata highlighted)
* A download link (valid for 90 days) if an article PDF is available
* For SWORD users, it would also include the URL of the original deposit in your repository.

You would need to use the information supplied in the email to update your repository record manually.  

This option avoids you having to de-duplicate deposits which Router would otherwise make into your repository.

## Filter options

Filter options allow you to select the kinds of duplicates you will receive.

### Current duplicate filter options:

The current options are presented as a drop-down list:
```
A. No duplicates from secondary sources (Crossref, PubMed, EMPC)
B. Secondary source duplicates only if they contain additional metadata 
C. All duplicates.
```
(Duplicates direct from publishers are always sent).

### Proposed duplicate filter options
These would be presented as a drop-down list - you would choose one to apply to your account.
```
0. No duplicates from any source (including publishers)
1. All duplicates direct from Publisher (none from secondary sources)
2. As (1) or where notification has an increased number of any of these: Authors, Author-ORCIDS, Funders, Funder-IDs, Grant-numbers, Licences
3. As (2) or with additional HIGH rank metadata fields or a PDF article (where none previously supplied)
4. As (3) or with additional MEDIUM rank metadata fields
5. As (4) or with additional LOW rank metadata fields
6. All duplicates.
```
**Notes:**

* Options from 1 onwards are all *additive*: they include the criteria of all preceding options in addition to those described (so, for example, option 3 includes options 1 & 2)
* Options 2 to 6 effectively apply only to secondary sources, because they inherit option 1, which is passing ALL duplicates direct from a publisher. 
* For options 3 to 5, we only consider the presence of new metadata fields, we do NOT analyse for changes in existing metadata fields. E.g. if an Abstract had previously been supplied, and in a later notification it is revised, this would not be noticed.
* Comparison of current and proposed options:
  * A <=> 1
  * B <=> 5
  * C <=> 6

### Consultation questions
* Do the proposed options provide additional benefits over the current options?
* Do they offer enough granularity?
* Should option 1 be excluded so that Publisher and Secondary sources are treated equally? (This raises the possibility of you not receiving a publisher notification that arrives after an EPMC notification and has no additional metadata). 
* Or should we offer 2 drop-downs (each with options 0, 2-6) one applying to notifications from publishers the other to notifications from secondary sources.


## Ranking of metadata fields
The proposed options introduce the concept of **metadata rank**, which is intended to allow differentiation between metadata based on its "value" to your repository entries.

**This is a key area for consultation with you**, as we need to achieve consensus for the rankings.  

Below is our *straw-man* draft which we would like you to comment on - either agreeing or proposing changes.

| Metadata Field | Proposed Rank | Notes |
| ----- | ----------- | -------- |
| Journal title | High |  |
| Journal abbreviated title | Low |  |
| Journal volume | Medium |  |
| Journal issue | Medium |  |
| Publisher name | High |  |
| Journal identifier (e.g. ISSN) | Medium |  |
| Article title | High |  |
| Article subtitle | Low |  |
| Article type (e.g research, review) | Low |  |
| Article version (e.g. AM, VoR) | High |  |
| Article start-page | Medium |  |
| Article end-page | Medium |  |
| Article page-range | Medium |  |
| Article num-pages | Medium |  |
| Article language | Low | This is invariably 'En' - English |
| Article abstract | High |  |
| Article identifier (e.g. PubMed Id) | Low | Note that articles ALWAYS have a DOI |
| Article subject | Low |  |
| Author | High |  |
| Contributor (e.g. editor) | Low |  |
| Accepted date | High |  |
| Publication date | High |  |
| History date(s) | Low |  |
| Funding (including funder-IDs and grant-numbers | High |  |
| Embargo | High |  |
| Licence | High |  |


## Checkbox alternative to drop-down filter options and rankings
If the proposed duplicate filter options do not meet your needs or consensus cannot be reached on rankings, then another approach would be to offer a "menu" approach. 

This would be presented as a set of ~35 checkboxes:
* one checkbox for each of the 26 metadata fields listed in the table above
* 6 checkboxes corresponding to increases in the number of Authors, Author-ORCIDs, Funders, Funder-IDs, Grant-numbers, Licences 
* additional checkboxes for other attributes such as: Article PDF available; All duplicates from publishers; None from secondary source.


Your selections would then determine which duplicates you receive - for example, if you ticked checkboxes for just "Publication date" and "Funder count" and "Grant-number count", then you would only receive duplicates which have a publication date (where one had not been provided before) or where the number of funders or grant-numbers has increased (compared to the maximum values seen in all prior notifications).        


## Consultation Feedback
We would welcome your comments on the following topics:
* Receiving duplicates by email (instead of into your repository)
* Proposed additional duplicate filter options (7 options numbered 0 to 6) - are they useful? What alternatives would you prefer (see the Consultation questions in the filter options section above)?
* Proposed rank - Do you agree with the suggesed ranking? How would you rank the metadata fields differently?
* Does the checkbox alternative better meet your needs or would the filter option drop-down list be sufficient?

We would prefer you to provide feedback by adding a new Issue or by commenting on or reacting to an existing issue on this page: **https://github.com/jisc-services/Public-Documentation/issues** as this will then be visible to all interested parties, who can also comment or "vote" by clicking a reaction icon.   You will need to create a GitHub account to do this.
