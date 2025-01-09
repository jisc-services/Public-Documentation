# Duplicate Filtering

You decide whether or not you wish to receive duplicate notifications (i.e. article deposits) from Router's sources:
* Publisher's direct submissions to Router
* Secondary sources (i.e. Crossref, Europe PMC, PubMed) from which Router harvests notifications.

:zap:A notification is considered a duplicate if it has the same DOI as one previously processed.

On your Router Organisation Account page, in the ***Manage duplicate filtering*** panel, you can set which duplicates (if any) you receive from Router.

## Manage duplicate filtering panel

Using the 2 drop-down selection boxes you can select which duplicates you receive from:

1. Publishers
2. Secondary sources (Crossref, Europe PMC, PubMed).

Each drop down has 5 options, described in the table below.

### Filter options
| Option | Description | Impact of option selected |
|--------|----|--------------|
| No duplicates | No duplicates | You will not receive any duplicates from the particular source (Publisher or Secondary) |
| Duplicates with Enhanced/Corrected VoR or first with PDF | Duplicate notifications with article-version 'Enhanced/Corrected VoR' or the FIRST with a full-text PDF (no PDF received previously) | You will receive a duplicate notification if:<br><br>* its **Article Version** has one of these values:<br> &nbsp; - **CVoR** (corrected version of record)<br> &nbsp; - **EVoR** (enhanced version of record)<br> &nbsp; - **C/EVoR** (corrected or enhanced version of record);<br><br>* or it is the first notification that includes a PDF (article) file. |
| Duplicates with additional metadata or first with PDF | Duplicate notifications with version 'Enhanced/Corrected VoR' or containing additional metadata or the FIRST with a full-text PDF (no PDF received previously) | You will receive a duplicate notification if:<br><br>* its **Article Version** has one of these values: **CVoR**, **EVoR**, **C/EVoR**. (See above for acronym definition);<br><br>* or it has **additional metadata** (further info below);<br><br>* or it is the first notification that includes a PDF (article) file. |
| Duplicates with additional metadata or with a full-text PDF | Duplicate notifications with version 'Enhanced/Corrected VoR' or containing additional metadata or with a full-text PDF (even if a PDF was previously received) | You will receive a duplicate notification if:<br><br>* its **Article Version** has one of these values: **CVoR**, **EVoR**, **C/EVoR**. (See above for acronym definition);<br><br>* or it has **additional metadata** (further info below);<br><br>* or it contains a PDF (article) file (even if the previous notification also contained a PDF). |
| All duplicates | All duplicate notifications whether or not they have additional metadata fields or files | You will receive all duplicate notifications from the particular source (i.e. no filtering). |
<br>

## Additional metadata - explanation

Router evaluates the metadata of each notification it processes to determine whether it contains particular elements and, for some of these, the element count. The table below lists the metadata that Router assesses - which therefore may impact duplicate filtering.

:zap:A duplicate notification is considered to have **additional metadata** if any of the following are true:
* the notification contains metadata elements not previously provided
* the value of the article-version has changed
* any of the counts have increased (for those elements that are counted). 

Note that Router does NOT compare actual metadata values between duplicate versions - it only assesses whether new metadata elements are present (not seen in any previous version of the notification), or whether the count of some elements has increased.&nbsp; So, if a duplicate notification contains ONLY changed text, for example the *abstract* was amended, this would not be noticed by Router; and so it would NOT be assessed as containing additional metadata. 


### Metadata evaluated by Router
The *measure* column in the following table indicates how Router assesses particular metadata elements in each notification. These measures have the following meanings:
* Presence - Router records if the partricular metadata element is present
* Value - Router records if the metadata element is present with a particular value or set of values
* Count - Router records the number (count) of the particular metadata element.

| Metadata element | Measure | Notes |
|-------|---------|--------|
| Journal title | Presence | |
| Journal abbreviated title | Presence | |
| Journal volume | Presence | |
| Journal issue | Presence | |
| Publisher name | Presence | |
| Publisher identifier | Presence | |
| Article title | Presence | |
| Article subtitle | Presence | |
| Article type | Presence | |
| Article version - AM | Value | Article version is **AM** (Accepted Manuscript). | 
| Article version - VoR | Value | Article version is **VoR** (Version of Record). | 
| Article version - EVoR, CVoR or C/EVoR | Value | Article version is **EVoR** (Enhanced VoR), **CVoR** (Changed Vor), **C/EVoR** (Changed or Enhanced VoR). |
| Article page start | Presence | |
| Article page end | Presence | |
| Article page range | Presence | |
| Article number of pages | Presence | |
| Electronic article number | Presence | |
| Article language | Presence | |
| Article abstract | Presence | |
| Article DOI | Presence | |
| Non-DOI article identifier | Presence | |
| Subject keywords | Presence | |
| Authors | Count | Number of authors. |
| Corresponding author(s) | Presence | |
| Author ORCIDs | Count | Number of authors with an ORCID. |
| Author with structured affiliations | Count | Number of structured affiliations across all authors (can exceed the number of authors). |
| Author with affiliation Org Ids | Count | Number of structured affiliations across all authors that contain at least 1 organisation ID. |
| Contributors | Presence | At least one contributor (non-author) is present. |
| Accepted date | Presence| |
| Publication date - full (YYYY-MM-DD) | Presence | |
| Publication date - partial (YYYY or YYYY-MM) | Presence | |
| Publication status | Presence | |
| History dates | Presence | |
| Funding information | Count | Number of funders. |
| Funder identifiers | Count | Number of funder identifiers across all funders (can exceed the number of funders). |
| Grant numbers | Count | Number of grant/project numbers across all funders (can exceed the number of funders). |
| Embargo date | Presence | |
| Open licence | Count | Number of open licences. |
| Other licence | Count | Number of other licences. |
| Peer reviewed | Presence | |
| Acknowledgements | Presence | |
