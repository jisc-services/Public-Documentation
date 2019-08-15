# Using PubRouter Metadata
## Introduction
This page is intended to:
* explain how PubRouter uses the metadata it collects from publishers and other sources when it constructs notifications that it sends direct to Repositories
* provide guidance to CRIS vendors (and others) who retrieve raw data via PubRouter's API for the purpose of representing it in some form to users, for example as an Article record in a CRIS system.

Below are guidelines, based on what PubRouter sends to repositories, for displaying particular data sets.

The table below that describes how each metadata element is used and also recommends how it should be displayed to users.

## Guidelines for particular data sets 




## Field usage table

This table lists all the fields that are provided in PubRouter's [OutgoingNotification](/PublicationsRouter/api/v3/OutgoingNotification.md) JSON data structure which may be retrieved via an API GET request.

NOTES:
* Fields which will always be populated are indicated with an asterisk (*) in the Field column, all other fields are optional.

| Field | Description | PubRouter's Usage | Recommendation for user display |
| ----- | ----- | -------- | ------- | 
| id * | Persistent internal identifier for this record | Internal use only | Do not expose to users |
| created_date * | Date this metadata record was created | Internal use only | Do not expose to users |
| analysis_date * | Date the routing analysis took place | Internal use only | Do not expose to users |
| event | Keyword indicating publishing event that gave rise to this notification (one of: 'undefined', 'submitted', 'accepted', 'published', 'corrected', 'revised')| Not used - may be deprecated in future | Do not rely upon its value or expose to users| 
| provider.agent * | The notification source - either a Publisher or a consolidator (like PubMed) that Router harvests from | Sent to repository in an information string | Expose to users |
| content.packaging_format | Package format of the original associated article binary content (example: "https://pubrouter.jisc.ac.uk/FilesAndJATS") | Internal use only | Do not expose to users |
| links | Object that describes a link to a file | | |
| links.type | Keyword for type of resource (e.g. "package" - indicates a zip file, "unpackaged" - indicates an individual file, "fulltext" - article file, "splash" - splash screen)  | Selecting particular link - for example to select a "package" for sending to repository via SWORD | May assist with identifying links to expose to users |
| links.format | The mimetype of the resource available at the URL (e.g. text/html) | In conjunction with other links information, deciding whether/how to process a particular link | Assist with deciding how to process a link |
| links.url | URL to the associated resource | Depending on other links elements, a URL may be sent to a repository so that article text can be retrieved | Use to retrieve article text (depending on other links elements) |
| links.packaging | Package format identifier for the resource available at the URL, one of "https://pubrouter.jisc.ac.uk/FilesAndJATS" or "http://purl.org/net/sword/package/SimpleZip" | In conjunction with other links information, deciding whether/how to process a particular link | Assist with deciding how to process a link |
| links.access | URL access type, one of: "public" - indicates the content is available from public URL; "router" - content is in temporary PubRouter store (kept for 3 months); "special" - unpackaged content in temporary PubRouter store (this will duplicate content contained in a package with access-type "router") | In conjunction with other links information, deciding whether/how to process a particular link | Assist with deciding how to process a link  |
| metadata.journal * | Object describing the journal this article was published in |  | | 
| metadata.journal.title * | Title of the journal or publication | Provided to Repositories (in bibliographic citation information) | Expose to users |
| metadata.journal.abbrev_title | Abbreviated form of journal/publication title | May be used in the absence of journal.title |  |
| metadata.journal.volume | Number of a journal (or other document) within a series | Sent to repository (in bibliographic citation information) | Expose to users |
| metadata.journal.issue | Issue number of a journal, or in rare instances, a book | Sent to repository (in bibliographic citation information) | Expose to users |
| metadata.journal.publisher * | Publisher(s) of the article | Sent to repository | Expose to users |
| metadata.journal.identifier.type * |  Journal identifier type (e.g. "issn", "eissn", "pissn", "doi") | May be used to select particular identifier(s) for sending to repository | Expose to users |
| metadata.journal.identifier.id * | Identifier of the journal / publication (e.g. the ISSN number) | Usually sent to repository, along with the corresponding *type* value| Expose to users |
| metadata.article * | Object describing various details of the article | | | 
| metadata.article.title * | Title of the Article| Sent to repository | Expose to users |
| metadata.article.subtitle | Sub-title (if any) of the Article | For some repositories, may be appended to the *article.title* value | Possibly expose to users |
| metadata.article.type | Type or kind of article (e.g. 'research', 'commentary', 'review', 'case', or 'calendar') | Sent to repository | Expose to users |
| metadata.article.version * | Specifies the article version that the meta-data relates to | Sent to repository after normalising (where possible) to one of these values: AO, SMUR, AM, P, VoR, CVoR, EVoR  | Expose to users |
| metadata.article.start_page | Article start page | Sent to repository | Expose to users |
| metadata.article.end_page | Article end page| Sent to repository | Expose to users |
| metadata.article.page_range | Text describing discontinuous pagination | Sent to repository | Expose to users |
| metadata.article.num_pages | Number of pages | Sent to repository | Expose to users |
| metadata.article.language | Language(s) that article is published in (Array field) | Sent to repository | Expose to users |
| metadata.article.abstract | Article abstract | Sent to repository | Expose to users |
| metadata.article.identifier.type * | Type of identifier (e.g. DOI) | Sent to repository | Expose to users |
| metadata.article.identifier.id * | Article identfier value (e.g. DOI number) | Sent to repository | Expose to users |
| metadata.article.subject | Subject classification(s) / keyword(s) (Array field) | Sent to repository | Expose to users   |
| metadata.author * | Array of author objects describing authors of this article | Author name, type and identifiers are sent to repository | Expose to users | 
| ....type | Type of author (e.g. corresponding) | |  |
| ....name.firstname * | Author's firstname(s) | |  |
| ....name.surname * | Author's surname (lastname) |  |  |
| ....name.fullname | Full name - usually "Surname, Firstname(s)" |  |  |
| ....name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) |  |  |
| ....organisation_name | Name of organisation (seldom populated) | |  |
| ....identifier.type * | Type of identifier (e.g. ORCID, email) |  |  |
| ....identifier.id * | Author identfier value (e.g. ORCID number, email address) |  |  |
| ....affiliation | Author organisational affiliation | Internal processing only | |
| metadata.contributor | Array of contributor objects describing contributors to this article | Contributor name, type and identifiers are sent to repository | Expose to users |
| ....type | Type of contributor (e.g. editor) |  |  |
| ....name.firstname | Contributor's firstname(s) | |  |
| ....name.surname | Contributor's surname (lastname) | |  |
| ....name.fullname | Full name  | |  |
| ....name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) |  |  |
| ....organisation_name |Name of organisation (seldom populated) | |  |
| ....identifier.type | Type of identifier (e.g. ORCID, email) |  |  |
| ....identifier.id | Contributor identfier value (e.g. ORCID number, email address) | |  |
| ....affiliation | Contributor organisational affiliation | Internal processing only | |
| metadata.accepted_date | Date publication accepted for publication | Sent to repository | Expose to users |
| metadata.publication_date | Object describing date that article was published | Sent to repository | Expose to users | 
| ....publication_format | Format of publication (print, electronic) | Sent to some repositories | |
| ....date | Date of publication | | |
| ....year | Year of publication | | |
| ....month | Month of publication (where known) | | |
| ....day | Day of publication (where known) | | |
| ....season | Season of publication (e.g. Spring, Third quarter) | | |
| metadata.history_date | Array of objects describing dates of this article at different stages of it's history | Sent to repository | Expose to users | 
| ....date_type | Type of date, e.g. received, accepted, published, published_online, epub, ppub, pub |  |  |
| ....date | Date of particular publishing event | | |
| metadata.publication_status * | Status of publication that this metadata refers to: published, accepted | Sent to repository | Expose to users | 
| metadata.funding | List of funding information associated with this article | Sent to repository | Expose to users |  
| ....name | Funder name |  |  |
| ....identifier.type | Funder identifier type (e.g "ringold")  |  |  |
| ....identifier.id | Funder identifier (e.g. Ringold ID) |  |  |
| ....grant_numbers | List of funder's grant numbers  | | |
| metadata.embargo | Embargo information for this article | Sent to repository | Expose to users |  
| ....start | Date that embargo starts | | |
| ....end | Date that embargo ends | | |
| ....duration | Embargo duration in MONTHS | | |
| metadata.license_ref | Array of license_ref objects, describing licenses associated with this article | | |
| ....title | Title or name of the licence applied to the article; free-text | | |
| ....type | Type of licence (most likely the same as the title or 'ali_free' if ali:free-to-read); free-text | |  |
| ....url | URL for information on the licence | | |
| ....version | Version of the licence | |  |
| ....start | License start date | | |
| ....best | Best license indicator, 1 license (at most) in the array will have *best* set to *true*. | |  |
| metadata.free2read | Ali:free-to-read information | Not used | |
| ....start | Ali:free-to-read Start date (may be an empty string) | Not used | |
| ....end | Ali:free-to-read End date (may be an empty string) | Not used | |
