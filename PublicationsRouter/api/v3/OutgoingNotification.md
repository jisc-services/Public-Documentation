# Outgoing Notification

The Publications Router OutgoingNotification is the meta-data structure output by Publications Router for notifications that have been matched (designated for transmission) to at least one Repository.

## JSON Data Structure

The full JSON structure of the model is shown here.

IMPORTANT: the structure returned by an API request will only have elements for which data exists; where no data is available the element will be omitted from the structure. 

Please see the [Using Router Metadata](./Using_Router_Metadata.md#using-router-metadata) page for important guidelines on using particular data elements.

```
{
	"id": "<String>",
	"created_date": "<Date/time in ISO 8601 format - YYYY-MM-DDThh:mm:ttZ  e.g. 2015-12-01T17:26:40Z>",
	"analysis_date": "<Date/time in ISO 8601 format - YYYY-MM-DDThh:mm:ttZ  2015-12-01T17:26:40Z>",
	"event": "<Keyword indicating publishing event that gave rise to this notification: 'undefined', 'submitted', 'accepted', 'published', 'corrected', 'revised'.>",
	"provider": {
		"agent": "<String defining the software/process which put the content here, provided by provider>"
	},
	"content": {
		"packaging_format": "<Identifier for packaging format used>"
	},
	"links": [
	    	{
		"type": "<Link type: package|unpackaged|splash|fulltext>",
		"format": "<text/html|application/pdf|application/xml|application/zip|...>",
		"url": "<Provider's splash, fulltext or machine readable page>",
		"packaging": "<Package format identifier string (may be absent)>",
		"access":"<Permitted access level, one of: public|router|special >"
			}
	],
	// dup_diffs will be present ONLY FOR DUPLICATE notifications - hence it's presence/absence will indicate if notification is a duplicate or not  
	"dup_diffs": [
	    { // First array element is a comparison of the current notification with the original notification (the first with this DOI)
		"old_date": "<The created_date of the original notification  e.g. 2021-06-02T15:58:58Z>",
		"curr_bits": <Long integer: Bit mask which summarises analysis of current notification's metadata e.g 205084675068>,
		"old_bits": <Long integer: Bit mask which summarises analysis of original notification's metadata e.g 144513149944>,
		// The following elements (n_…) are signed integer variations between counts of particular sets of metadata
		// They indicate whether the current notification has more/fewer/identical numbers compared to the original notification
		"n_auth": <Integer: Difference (+/-) in number of Authors between current and original notification e.g. 1>
		"n_orcid": <Integer: Difference (+/-) in number of authors with ORCIDs between current and original notification e.g. 2>,
		"n_fund": <Integer: Difference (+/-) in number of Funding elements between current and original notification e.g. 3>,
		"n_fund_id": <Integer: Difference (+/-) in number of Funder IDs between current and original notification e.g. 0>,
		"n_grant": <Integer: Difference (+/-) in number of Grant numbers between current and original notification e.g. 4>,
		"n_lic": <Integer: Difference (+/-) in number of Licences numbers between current and original notification e.g. 0>,
	    },
	    { // Second array element will be present ONLY for 2nd duplicate onwards.
	  // It compares the current notification with an accumulated view of all previous notifications
		"old_date": "<The created_date of the last duplicate notification e.g. 2021-06-04T06:22:51Z>",
		"curr_bits": <Long integer: Bit mask which summarises analysis of current notification's metadata e.g 205084675068>,
		"old_bits": <Long integer: Bit mask which summarises cumulative analysis of all previous notifications' metadata e.g 204513149944>,
		// The following elements (n_…) are signed integer variations between counts of particular sets of metadata
		// They indicate whether the current notification has identical/more/fewer count of an entity compared to the maximum count seen any previous notification
		// A POSITIVE number indicates that the current notification has MORE of the particular entity than any notification previously seen
		// A NEGATIVE number indicates that the current notification has FEWER of the particular entity than seen in at least one previous notification
		"n_auth": <Integer: Difference (+/-) in number of Authors between current and maximum number of Authors found in previous notifications e.g. 0>
		"n_orcid": <Integer: Difference (+/-) in number of author ORCIDs between current and maximum number of ORCIDs found in previous notifications e.g. 1>,
		"n_fund": <Integer: Difference (+/-) in number of Funding elements between current and maximum number found in previous notifications e.g. 0>,
		"n_fund_id": <Integer: Difference (+/-) in number of Funder IDs between current and maximum number found in previous notifications e.g. 0>,
		"n_grant": <Integer: Difference (+/-) in number of Grant numbers between current and maximum number found in previous notifications e.g. 0>,
		"n_lic": <Integer: Difference (+/-) in number of Licences numbers between current and maximum number found in previous notifications e.g. 0>,
	    }
	],
	"metadata": {
		"journal": {
			"title": "<Journal / publication title>",
			"abbrev_title": "<Abbreviated version of journal title>",
			"volume": "<Number of a journal (or other document) within a series>",
			"issue": "<Issue number of a journal, or in rare instances, a book.>",
			"publisher": ["<Name of the publisher(s) of the content>"],
			"identifier": [
			    {
				"type": "issn",
				"id": "<issn of the journal (could be print or electronic)>"
			    }, {
				"type": "eissn",
				"id": "<electronic issn of the journal>"
			    }, {
				"type": "pissn",
				"id": "<print issn of the journal>"
			    }, {
				"type": "doi",
				"id": "<doi for the journal or series>"
				}
			]
		},
		"article": {
			"title": "<Article title or book chapter title>",
			"subtitle": [ "<Article title or book chapter Subtitle>" ],
			"type": "<Type or kind of article (e.g. 'research', 'commentary', 'review', 'case', or 'calendar')>",
			"version": "<Article version e.g. VoR>",
			"start_page": "<Page number on which a document starts>",
			"end_page": "<Page number on which a document ends>",
			"page_range": "<Text describing discontinuous pagination.>",
			"num_pages": "<Total number of pages >",
			"language": [ "<languages >" ],
			"abstract": "<Abstract of the work >",
			"identifier": [
				{
				"type": "<Type of identifier, such as 'doi'>",
				"id": "<Article identifier value>"
				}
			],
			"subject": [ "<Subject keywords/classifications>" ]
		},
		"author": [
			{
			"type": "<Type of author e.g. 'author' or 'corresp'>",
			"name": {
				"firstname": "<Author first name>",
				"surname": "<Author surname>",
				"fullname": "<Author name>",
				"suffix": "<Qualifiers that follow a persons name Sr. Jr. III, 3rd>"
				},
			"organisation_name": "<Name of organisation if author is an organisation >",
			"identifier": [
			    {
				"type": "orcid",
				"id": "<Author's orcid>"
			    }, {
				"type": "email",
				"id": "<Author's email address>"
			    }
			],
			"affiliation": "<Author affiliation>"
			}
		],
		"contributor": [
			{
			"type": "<Type of contributor e.g. 'editor'>",
			"name": {
				"firstname": "<Contributor first name>",
				"surname": "<Contributor surname>",
				"fullname": "<Contributor name>",
				"suffix": "<Qualifiers that follow a persons name Sr. Jr. III, 3rd>"
				},
			"organisation_name": "<Name of organisation if author is an organisation >",
			"identifier": [
			    {
				"type": "orcid",
				"id": "<Contributor's orcid>"
			    }, {
				"type": "email",
				"id": "<Contributor's email address>"
			    }
			],
			"affiliation": "<Author affiliation>"
			}
		],
		"accepted_date": "<Date YYYY-MM-DD format>",
		"publication_date": {
			"publication_format": "<Format of publication (print, electronic)>",
			"date": "<Date YYYY-MM-DD format>",
			"year": "<Year YYYY format>",
			"month": "<Month MM format>",
			"day": "<Day DD format>",
			"season": "<Season of publication (for example, Spring, Third Quarter).>"
		},
		"history_date": [
			{
			"date_type": "<Type of date: received, accepted...>",
			"date": "<Date>"
			}
		],
		"publication_status": "<Published, accepted or blank>",
		"funding": [
		 	{
			"name": "<Name of funder>",
			"identifier": [
		    	 	{
				"type": "<Identifier type>",
				"id": "<Funder identifier>"
				}
			],
			"grant_numbers": ["<List of grant numbers associated with this funder>"]
			}
		],
		"embargo": {
			"start": "<Embargo start date, format: YYYY-MM-DD>",
			"end": "<Embargo end date, format: YYYY-MM-DD>",
			"duration": "<Embargo duration in months>"
		},
		"license_ref": [
			{
			"title": "<Name of licence>",
			"type": "<Type>",
			"url": "<URI of licence>",
			"version": "<Licence version; for example: 4.0>",
			"start": "<Date licence starts (YYYY-MM-DD format)>",
			"best": "<Boolean indicates the optimum open licence - will be true for maximum of ONE licence in the array>"
			}
		]
	}
}
```

## Field Definitions

Each of the fields in the JSON structure above is defined in the table below in the order that they appear (dot notation is used to qualify them).

NOTE that fields which will always be populated are indicated with an asterisk (*) in the Field column, all other fields are optional.

| Field | Description | Datatype | Format |
| ----- | ----------- | -------- | ------ |
| id * | Opaque, persistent system identifier for this record | unicode | |
| created_date * | Date this record was created | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| analysis_date * | Date the routing analysis took place | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| event | Keyword indicating publishing event that gave rise to this notification (one of: 'undefined', 'submitted', 'accepted', 'published', 'corrected', 'revised')| unicode | text | 
| provider * | Object describing the source of this notification | object | | 
| provider.agent * | Free-text field for identifying the API client used to create the notification | unicode | free text |
| content.packaging_format | Package format identifier for the associated binary content (example: "https://pubrouter.jisc.ac.uk/FilesAndJATS") | unicode | URL |
| links.type | Keyword for type of resource (e.g. "package", "unpackaged", "splash", "fulltext") - no restrictions on use in this version of the system | unicode | |
| links.format | The mimetype of the resource available at the URL (e.g. "application/pdf", application/zip) | unicode | |
| links.url | URL to the associated resource.  All URLs provided by publishers should be publicly accessible for a minimum of 3 months | unicode | URL |
| links.packaging | Package format identifier for the resource available at the URL, one of "https://pubrouter.jisc.ac.uk/FilesAndJATS" or "http://purl.org/net/sword/package/SimpleZip".  This element will be present only for Router packaged content. | unicode | |
| links.access | URL access type, one of: **"public"** - indicates the content is available from public URL; **"router"** - content is in temporary Publications Router store (kept for 3 months); **"special"** - unpackaged content in temporary Publications Router store (this will duplicate content contained in a package with access-type "router")  | unicode | |
| dup_diffs | Array of duplicate-differences objects (either 1 or 2 elements). `dup_diffs` will be present ONLY if the notification is a duplicate.<br>The first duplicate notification will have a single difference object, subsequen duplicate notifications will all have 2 difference objects. |  |  |
| dup_diffs.old_date | created_date timestamp of the notification these differences relate to   | Unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| dup_diffs.curr_bits | Bit mask summarises an analysis of the current notification's metadata | Integer |  |
| dup_diffs.old_bits |  Bit mask summarises an analysis of the comparison notification's (original notification or accumulated notifications ) metadata | Integer |  |
| dup_diffs.n_auth | Difference (+/-) in the Author count between current and original or accumulated notifications | Integer |  |
| dup_diffs.n_orcid | Difference (+/-) in the Author ORCID count between current and original or accumulated notifications  | Integer |  |
| dup_diffs.n_fund |  Difference (+/-) in the Funder count between current and original or accumulated notifications | Integer |  |
| dup_diffs.n_fund_id | Difference (+/-) in the Funder ID count between current and original or accumulated notifications  |Integer  |  |
| dup_diffs.n_grant | Difference (+/-) in the Grant count between current and original or accumulated notifications  | Integer |  |
| dup_diffs.n_lic | Difference (+/-) in the Licence count between current and original or accumulated notifications  | Integer |  |
| metadata.journal * | Object describing the journal this article was published in | object | | 
| metadata.journal.title * | Title of the journal or publication | unicode | |
| metadata.journal.abbrev_title | Abbreviated form of journal/publication title | unicode | |
| metadata.journal.volume | Number of a journal (or other document) within a series | unicode | |
| metadata.journal.issue | Issue number of a journal, or in rare instances, a book | unicode | |
| metadata.journal.publisher * | Publisher(s) of the article (Array field) | unicode | |
| metadata.journal.identifier.type * |  Identifier type (e.g. "issn", "eissn", "pissn", "doi") - no vocabulary for this field in this version of the system | unicode | |
| metadata.journal.identifier.id * | Identifier of the journal / publication (e.g. the ISSN number) | unicode | |
| metadata.article * | Object describing various details of the article | object | | 
| metadata.article.title * | Title of the Article| unicode | |
| metadata.article.subtitle | Subtitle (if any) of the Article | unicode | |
| metadata.article.type | Type or kind of article (e.g. 'research', 'commentary', 'review', 'case', or 'calendar') | unicode | |
| metadata.article.version * | Specifies article version that meta-data relates to, preferably expressed using NISO scheme (http://www.niso.org/publications/rp/RP-8-2008.pdf) (e.g. AO, SMUR, AM, P, VoR, CVoR, EVoR, C/EVoR)   | unicode | |
| metadata.article.start_page | Article start page  | unicode | |
| metadata.article.end_page | Article end page| unicode | |
| metadata.article.page_range | Text describing discontinuous pagination | unicode | |
| metadata.article.num_pages | Number of pages | unicode | |
| metadata.article.language | Language(s) that article is published in (Array field) | unicode | |
| metadata.article.abstract | Article abstract | unicode | |
| metadata.article.identifier.type * | Type of identifier (e.g. DOI) | unicode | |
| metadata.article.identifier.id * | Article identfier value (e.g. DOI number) | unicode | |
| metadata.article.subject | Subject classification(s) / keyword(s) (Array field) | unicode | |
| metadata.author * | Array of author objects describing authors of this article | array | | 
| metadata.author.type | Type of author, typically "author" or "corresp" (for corresponding author) | unicode | |
| metadata.author.name.firstname * | Author's firstname(s) - space separated if more than one | unicode | |
| metadata.author.name.surname * | Author's surname (lastname) | unicode | |
| metadata.author.name.fullname | Full name - preferably expressed as "Surname, Firstname(s)" | unicode | |
| metadata.author.name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) | unicode | |
| metadata.author.organisation_name |Name of organisation if author is an organisation  | unicode | |
| metadata.author.identifier.type * | Type of identifier (e.g. ORCID, email) | unicode | |
| metadata.author.identifier.id * | Author identfier value (e.g. ORCID number, email address) | unicode | |
| metadata.author.affiliation | Author organisational affiliation | unicode | free text  |
| metadata.contributor | Array of contributor objects describing contributors to this article | array | |
| metadata.contributor.type | Type of contributor (e.g. editor) | unicode | |
| metadata.contributor.name.firstname | Contributor's firstname(s) - space separated if more than one | unicode | |
| metadata.contributor.name.surname | Contributor's surname (lastname) | unicode | |
| metadata.contributor.name.fullname | Full name - preferably expressed as "Surname, Firstname(s)" | unicode | |
| metadata.contributor.name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) | unicode | |
| metadata.contributor.organisation_name |Name of organisation if contributor is an organisation  | unicode | |
| metadata.contributor.identifier.type | Type of identifier (e.g. ORCID, email) | unicode | |
| metadata.contributor.identifier.id | Contributor identfier value (e.g. ORCID number, email address) | unicode | |
| metadata.contributor.affiliation | Contributor organisational affiliation | unicode | free text  |
| metadata.accepted_date | Date publication accepted for publication | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| metadata.publication_date | Object describing date that article was published | object | | 
| metadata.publication_date.publication_format | Format of publication (print, electronic) | unicode | print or electronic |
| metadata.publication_date.date | Date of publication | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| metadata.publication_date.year | Year of publication | unicode | YYYY |
| metadata.publication_date.month | Month of publication (where known) | unicode | MM |
| metadata.publication_date.day | Day of publication (where known) | unicode | DD |
| metadata.publication_date.season | Season of publication (e.g. Spring, Third quarter) | unicode | |
| metadata.history_date | Array of objects describing dates of this article at different stages of it's history | array | | 
| metadata.history_date.date_type | Type of date, e.g. received, accepted, published, published_online, epub, ppub, pub | unicode | |  |
| metadata.history_date.date | Date of particular publishing event | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| metadata.publication_status * | Status of publication that this metadata refers to: published, accepted | unicode | |
| metadata.funding | List of funding information associated with this article | array | | 
| metadata.funding.name | Funder name | unicode | |
| metadata.funding.identifier.type | Funder identifier type (e.g "ringold") - no vocabulary for this field in this version of the system | unicode | |
| metadata.funding.identifier.id | Funder identifier (e.g. Ringold ID) | unicode | |
| metadata.funding.grant_numbers | List of grant numbers associated with funding source behind this article | unicode | |
| metadata.embargo | Embargo information for this article | object | | 
| metadata.embargo.start | Date that embargo starts | unicode | YYYY-MM-DD |
| metadata.embargo.end | Date that embargo ends | unicode | YYYY-MM-DD |
| metadata.embargo.duration | Embargo duration in MONTHS | unicode | |
| metadata.license_ref | Array of license_ref objects, describing licences associated with this article | | |
| metadata.license_ref.title | Title or name of the licence applied to the article; free-text | unicode | |
| metadata.license_ref.type | Type of licence (most likely the same as the title); free-text | unicode | |
| metadata.license_ref.url | URL for information on the licence | unicode | URL |
| metadata.license_ref.version | Version of the licence | unicode | |
| metadata.license_ref.start | License start date | unicode | |
| metadata.license_ref.best | Best licence indicator, 1 licence (at most) in the array will have *best* set to *true*. (IMPORTANT: see note below) | boolean | |

## Notes
### Licences
See [Using Router Metadata](./Using_Router_Metadata.md#licence-details) for **important** guidance on using and displaying these fields.

