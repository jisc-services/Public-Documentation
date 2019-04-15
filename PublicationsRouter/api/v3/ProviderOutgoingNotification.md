# Provider Outgoing Notification

The PubRouter ProviderOutgoingNotification is the structure available to publishers of meta-data output by PubRouter for notifications that have been matched (designated for transmission) to at least one Repository.

It differs from the [OutgoingNotification](./OutgoingNotification.md) only by the addition of the *id*, *route* and *ref* elements in the *provider* object (i.e. provider.id, provider.ref and provider.route).

# Outgoing Notification

The PubRouter OutgoingNotification is the meta-data structure output by PubRouter for notifications that have been matched (designated for transmission) to at least one Repository.

## JSON Data Structure

The full JSON structure of the model is shown here.

IMPORTANT: the structure returned by an API request will only have elements for which data exists; where no data is available the element will be omitted from the structure. 

```json
{
	"id": "<string>",
	"created_date": "<date/time in ISO 8601 format - YYYY-MM-DDThh:mm:ttZ  e.g. 2015-12-01T17:26:40Z>",
	"analysis_date": "<date/time in ISO 8601 format - YYYY-MM-DDThh:mm:ttZ  2015-12-01T17:26:40Z>",
	"event": "<keyword indicating publishing event that gave rise to this notification: 'undefined', 'submitted', 'accepted', 'published', 'corrected', 'revised'.>",
	"provider": {
		"id" : "<user account id of the provider>",
		"agent": "<string defining the software/process which put the content here, provided by provider>",
		"ref": "<provider's globally unique reference for this research object>",
		"route" : "<method by which notification was received: native api, sword, ftp>"
	},
	"content": {
   		"packaging_format": "<identifier for packaging format used>"
	}, 
	"links": [
	    	{
		"type": "<link type: splash|fulltext>",
		"format": "<text/html|application/pdf|application/xml|application/zip|...>",
		"url": "<provider's splash, fulltext or machine readable page>",
	        "packaging": "<package format identifier string>",
		"access":"<permitted access level, one of: public|router >"
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
			"sub_title": [ "<Article title or book chapter Subtitle>" ],
			"type": "<Type or kind of article (e.g. 'research', 'commentary', 'review', 'case', or 'calendar')>",
			"version": "<version of the record, e.g. AAM>",
			"start_page": "<Page number on which a document starts>",
			"end_page": "<Page number on which a document ends>",
			"page_range": "<Text describing discontinuous pagination.>",
			"num_pages": "<Total number of pages >",
			"language": [ "<languages >" ],
			"abstract": "<Abstract of the work >",
			"identifier": [
				{
				"type": "doi",
				"id": "<doi for the record>"
				}
			],
			"subject": [ "<subject keywords/classifications>" ]
		},
		"author": [
			{
			"type": "<Type of contribution author>",
			"name": {
				"firstname": "<author first name>",
				"surname": "<author surname>",
				"fullname": "<author name>",
				"suffix": "<Qualifiers that follow a persons name Sr. Jr. III, 3rd>"
				},
			"organisation_name": "<Name of organisation if author is an organisation >",
			"identifier": [
			    {
				"type": "orcid",
				"id": "<author's orcid>"
			    }, {
				"type": "email",
				"id": "<author's email address>"
			    }
			],
			"affiliation": "<author affiliation>"
			}
		],
		"contributor": [
			{
			"type": "<Type of contribution author>",
			"name": {
				"firstname": "<author first name>",
				"surname": "<author surname>",
				"fullname": "<author name>",
				"suffix": "<Qualifiers that follow a persons name Sr. Jr. III, 3rd>"
				},
			"organisation_name": "<Name of organisation if author is an organisation >",
			"identifier": [
			    {
				"type": "orcid",
				"id": "<author's orcid>"
			    }, {
				"type": "email",
				"id": "<author's email address>"
			    }
			],
			"affiliation": "<author affiliation>"
			}
		],
		"accepted_date": "<date YYYY-MM-DD format>",
		"publication_date": {
			"publication_format": "<Format of publication (print, electronic)>",
			"date": "<date YYYY-MM-DD format>",
			"year": "<year YYYY format>",
			"month": "<month MM format>",
			"day": "<day DD format>",
			"season": "<Season of publication (for example, Spring, Third Quarter).>"
		},
		"history_date": [
			{
			"date_type": "<Type of date: received, accepted...>",
			"date": "<date>"
			}
		],
		"publication_status": "<Published, accepted or blank>",
		"funding": [
		 	{
			"name": "<name of funder>",
			"identifier": [
		    	 	{
				"type": "<identifier type>",
				"id": "<funder identifier>"
				}
			],
			"grant_numbers": ["<list of grant numbers associated with this funder>"]
			}
		],
		"embargo": {
			"start": "<embargo start date>",
			"end": "<embargo end date>",
			"duration": "<embargo duration in months>"
		},
		"license_ref": [
			{
			"title": "<name of licence>",
			"type": "<type>", 
			"url": "<url>",
			"version": "<license version; for example: 4.0>",
			"start": "<Date licence starts (YYYY-MM-DD format)>"
			}
		],
		"free2read": {
			"start": "<start date of ali:free-to-read, YYYY-MM-DD format (or empty string)>",
			"end": "<end date of ali:free-to-read, YYYY-MM-DD format (or empty string)>"
		}
	}
}
```

## Field Definitions

Each of the fields in the JSON structure above is defined in the table below in the order that they appear (dot notation is used to qualify them).

NOTE that fields which will always be populated are indicated with an asterisk (*) in the Field column, all other fields are optional.

| Field | Description | Datatype | Format |
| ----- | ----------- | -------- | ------ |
| id * | opaque, persistent system identifier for this record | unicode |  |
| created_date * | Date this record was created | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| analysis_date * | Date the routing analysis took place | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |
| event | Keyword indicating publishing event that gave rise to this notification (one of: 'undefined', 'submitted', 'accepted', 'published', 'corrected', 'revised')| unicode | text | 
| provider * | Object describing the source of this notification | object | | 
| provider.agent * | Free-text field for identifying the API client used to create the notification | unicode | free text |
| provider.id * | User account ID of provider | unicode | free text |  |
| provider.ref | Publisher's own identifier for the notification | unicode | free text |  |
| provider.route * | Method by which notification was received: native api, sword, ftp | unicode | free text |  |
| content.packaging_format | Package format identifier for the associated binary content (example: "https://pubrouter.jisc.ac.uk/FilesAndJATS") | unicode | URL |
| links.type | Keyword for type of resource (e.g. splash, fulltext) - no restrictions on use in this version of the system | unicode |  |
| links.format | The mimetype of the resource available at the URL (e.g. text/html) | unicode |  |
| links.url | URL to the associated resource.  All URLs provided by publishers should be publicly accessible for a minimum of 3 months | unicode | URL |
| links.packaging | Package format identifier for the resource available at the URL | unicode |  |
| links.access | URL access type, one of: "public" - indicates the content is available from public URL; "router" - content is available from temporary PubRouter store (kept for 3 months) | unicode |  |
| metadata.journal * | Object describing the journal this article was published in | object | | 
| metadata.journal.title * | Title of the journal or publication | unicode |  |
| metadata.journal.abbrev_title | Abbreviated form of journal/publication title | unicode |  |
| metadata.journal.volume | Number of a journal (or other document) within a series | unicode |  |
| metadata.journal.issue | Issue number of a journal, or in rare instances, a book | unicode |  |
| metadata.journal.publisher * | Publisher(s) of the article (Array field) | unicode |  |
| metadata.journal.identifier.type * |  Identifier type (e.g. "issn", "eissn", "pissn", "doi") - no vocabulary for this field in this version of the system | unicode |  |
| metadata.journal.identifier.id * | Identifier of the journal / publication (e.g. the ISSN number) | unicode |  |
| metadata.article * | Object describing various details of the article | object | | 
| metadata.article.title * | Title of the Article| unicode |  |
| metadata.article.sub_title | Sub-title (if any) of the Article | unicode |  |
| metadata.article.type | Type or kind of article (e.g. 'research', 'commentary', 'review', 'case', or 'calendar') | unicode |  |
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
| metadata.free2read | Ali:free-to-read information | | |
| metadata.free2read.start | Ali:free-to-read Start date (may be an empty string) | unicode | YYYY-MM-DD |
| metadata.free2read.end | Ali:free-to-read End date (may be an empty string) | unicode | YYYY-MM-DD |
