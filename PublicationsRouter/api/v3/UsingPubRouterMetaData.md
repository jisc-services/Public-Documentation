# Using PubRouter Metadata
## Introduction
This page is intended to:
* explain how PubRouter uses the metadata it collects from publishers and other sources when it constructs notifications that it sends direct to Repositories
* provide guidance to CRIS vendors (and others) who retrieve raw data via PubRouter's API for the purpose of representing it in some form to users, for example as an Article record in a CRIS system.

The table below describes how each metadata element is used and also recommends how it should be displayed to users.

## Field usage

This table lists all the fields that are provided in PubRouter's [OutgoingNotification](/PublicationsRouter/api/v3/OutgoingNotification.md) JSON data structure which may be retrieved via an API GET request.

NOTES:
* Fields which will always be populated are indicated with an asterisk (*) in the Field column, all other fields are optional.

| Field | Description | PubRouter's Usage | Recommendation for user display |
| ----- | ----------- | -------- | ---- | 
| id * | Persistent internal system identifier for this record | Internal use only | Do not expose to users |
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

### --- Work in progress - got this far ---

| metadata.article.version * | Specifies article version that meta-data relates to, preferably expressed using NISO scheme (http://www.niso.org/publications/rp/RP-8-2008.pdf) (e.g. AO, SMUR, AM, P, VoR, CVoR, EVoR)   | unicode |  |
| metadata.article.start_page | Article start page  | unicode |  |
| metadata.article.end_page | Article end page| unicode |  |
| metadata.article.page_range | Text describing discontinuous pagination | unicode |  |
| metadata.article.num_pages | Number of pages | unicode |  |
| metadata.article.language | Language(s) that article is published in (Array field) | unicode |  |
| metadata.article.abstract | Article abstract | unicode |  |
| metadata.article.identifier.type * | Type of identifier (e.g. DOI) | unicode |  |
| metadata.article.identifier.id * | Article identfier value (e.g. DOI number) | unicode |  |
| metadata.article.subject | Subject classification(s) / keyword(s) (Array field) | unicode |  |
| metadata.author * | Array of author objects describing authors of this article | array | | 
| metadata.author.type | Type of author (e.g. corresponding) | unicode |  |
| metadata.author.name.firstname * | Author's firstname(s) - space separated if more than one | unicode |  |
| metadata.author.name.surname * | Author's surname (lastname) | unicode |  |
| metadata.author.name.fullname | Full name - preferably expressed as "Surname, Firstname(s)" | unicode |  |
| metadata.author.name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) | unicode |  |
| metadata.author.organisation_name |Name of organisation if author is an organisation  | unicode |  |
| metadata.author.identifier.type * | Type of identifier (e.g. ORCID, email) | unicode |  |
| metadata.author.identifier.id * | Author identfier value (e.g. ORCID number, email address) | unicode |  |
| metadata.author.affiliation | Author organisational affiliation | unicode | free text  |
| metadata.contributor | Array of contributor objects describing contributors to this article | array | |
| metadata.contributor.type | Type of contributor (e.g. editor) | unicode |  |
| metadata.contributor.name.firstname | Contributor's firstname(s) - space separated if more than one | unicode |  |
| metadata.contributor.name.surname | Contributor's surname (lastname) | unicode |  |
| metadata.contributor.name.fullname | Full name - preferably expressed as "Surname, Firstname(s)" | unicode |  |
| metadata.contributor.name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) | unicode |  |
| metadata.contributor.organisation_name |Name of organisation if contributor is an organisation  | unicode |  |
| metadata.contributor.identifier.type | Type of identifier (e.g. ORCID, email) | unicode |  |
| metadata.contributor.identifier.id | Contributor identfier value (e.g. ORCID number, email address) | unicode |  |
| metadata.contributor.affiliation | Contributor organisational affiliation | unicode | free text  |
| metadata.accepted_date | Date publication accepted for publication | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| metadata.publication_date | Object describing date that article was published | object | | 
| metadata.publication_date.publication_format | Format of publication (print, electronic) | unicode | print or electronic |
| metadata.publication_date.date | Date of publication | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| metadata.publication_date.year | Year of publication | unicode | YYYY |
| metadata.publication_date.month | Month of publication (where known) | unicode | MM |
| metadata.publication_date.day | Day of publication (where known) | unicode | DD |
| metadata.publication_date.season | Season of publication (e.g. Spring, Third quarter) | unicode |  |
| metadata.history_date | Array of objects describing dates of this article at different stages of it's history | array | | 
| metadata.history_date.date_type | Type of date, e.g. received, accepted, published, published_online, epub, ppub, pub | unicode |  |  |
| metadata.history_date.date | Date of particular publishing event | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| metadata.publication_status * | Status of publication that this metadata refers to: published, accepted | unicode |  |
| metadata.funding | List of funding information associated with this article | array | | 
| metadata.funding.name | Funder name | unicode |  |
| metadata.funding.identifier.type | Funder identifier type (e.g "ringold") - no vocabulary for this field in this version of the system | unicode |  |
| metadata.funding.identifier.id | Funder identifier (e.g. Ringold ID) | unicode |  |
| metadata.funding.grant_numbers | List of grant numbers associated with funding source behind this article | unicode |  |
| metadata.embargo | Embargo information for this article | object | | 
| metadata.embargo.start | Date that embargo starts | unicode | YYYY-MM-DD |
| metadata.embargo.end | Date that embargo ends | unicode | YYYY-MM-DD |
| metadata.embargo.duration | Embargo duration in MONTHS | unicode |  |
| metadata.license_ref | Array of license_ref objects, describing licenses associated with this article | | |
| metadata.license_ref.title | Title or name of the licence applied to the article; free-text | unicode |  |
| metadata.license_ref.type | Type of licence (most likely the same as the title or 'ali_free' if ali:free-to-read); free-text | unicode |  |
| metadata.license_ref.url | URL for information on the licence | unicode | URL |
| metadata.license_ref.version | Version of the licence | unicode |  |
| metadata.license_ref.start | License start date | unicode |  |
| metadata.license_ref.best | Best license indiator, 1 license (at most) in the array will have *best* set to *true*. (See note below). | boolean |  |
| metadata.free2read | Ali:free-to-read information | | |
| metadata.free2read.start | Ali:free-to-read Start date (may be an empty string) | unicode | YYYY-MM-DD |
| metadata.free2read.end | Ali:free-to-read End date (may be an empty string) | unicode | YYYY-MM-DD |
