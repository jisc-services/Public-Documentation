# Incoming Notification

The Publications Router IncomingNotification is the structure used by a Publisher to send publication meta-data to Publications Router for routing to Repositories.

## JSON Data Structure

The JSON structure of the model is as follows:

```json
{
	"event": "<keyword indicating publishing event that gave rise to this notification: 'undefined', 'submitted', 'accepted', 'published', 'corrected', 'revised'.>",
	"provider": {
		"agent": "<string defining the software/process which put the content here, provided by provider>",
		"ref": "<provider's globally unique reference for this research object>"
	},
	"content": {
		"packaging_format": "<identifier for packaging format used>"
	},
	"links": [
		{
		"type": "<link type: splash|fulltext>",
		"format": "<text/html|application/pdf|application/xml|application/zip|...>",
		"url": "<provider's splash, fulltext or machine readable page>"
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
			"e_num": "<Electronic article number - an alternative to page_range / start_page / end_page>",
			"num_pages": "<Total number of pages >",
			"language": [ "<languages >" ],
			"abstract": "<Abstract of the work >",
			"identifier": [
				{
				"type": "doi",
				"id": "<doi for the record>"
				}, {
					}
			],
			"subject": [ "<subject keywords/classifications>" ]
		},
		"author": [
			{
			"type": "<Type of author e.g. 'author' or 'corresp'>",
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
				}, {
				}
			],
			"affiliations": [
				{
				"identifier": [
					{
					"type": "<Identifier type e.g. 'GRID' or 'ROR'>",
					"id": "<Identifier value>"
					}
				],
				"org": "<Organisation name (may include Department name)>",
				"dept": "<Department name>",
				"street": "<Street name>",
				"city": "<City>",
				"state": "<State or County>",
				"postcode": "<Postal code>",
				"country": "<Country name>",
				"country_code": "<ISO 3166 country code - 2 character>",
				"raw": "<Unstructured affilation string - Optional unless ALL other affilation fields are empty>"
				}
			]
			}
		],
		"contributor": [
			{
			"type": "<Type of contributor e.g. 'editor'>",
			"name": {
				"firstname": "<contributor first name>",
				"surname": "<contributor surname>",
				"fullname": "<contributor name>",
				"suffix": "<Qualifiers that follow a persons name Sr. Jr. III, 3rd>"
			},
			"organisation_name": "<Name of organisation if contributor is an organisation >",
			"identifier": [
				{
				"type": "orcid",
				"id": "<contributor's orcid>"
				}, {
				"type": "email",
				"id": "<contributor's email address>"
				}, {
				}
			],
			"affiliations": [
				{
				"identifier": [
					{
					"type": "<Identifier type e.g. 'GRID' or 'ROR'>",
					"id": "<Identifier value>"
					}
				],
				"org": "<Organisation name (may include Department name)>",
				"dept": "<Department name>",
				"street": "<Street name>",
				"city": "<City>",
				"state": "<State or County>",
				"postcode": "<Postal code>",
				"country": "<Country name>",
				"country_code": "<ISO 3166 country code - 2 character>",
				"raw": "<Unstructured affilation string - Optional unless ALL other affilation fields are empty>"
				}
			]
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
			"grant_numbers":  ["<funder's grant numbers>"]
			}
		],
		"embargo": {
			"start": "<embargo start date>",
			"end": "<embargo end date>",
			"duration": "<embargo duration in days>"
		},
		"license_ref": [
			{
			"title": "<name of licence>",
			"type": "<type>",
			"url": "<url>",
			"version": "<version of license; for example: 4.0>" ,
			"start": "<Date licence starts (YYYY-MM-DD format)>"
			}
		],
		"peer_reviewed": "<Boolean value indicating if article has been peer reviewed>",
		"ack": "<Acknowledgement text>"
	}
}
```

## Field Definitions

Each of the fields in the JSON structure above is defined in the table below in the order that they appear (dot notation is used to qualify them).  NOTE that REQUIRED fields are indicated with an asterisk (*) in the Field column, all other fields are optional.

| Field (* = Required field) | Description | Datatype | Format | Allowed Values |
| ----- | ----------- | -------- | ------ | -------------- |
| event | Keyword indicating publishing event that gave rise to this notification. (Note that 'submitted' should be used in place of 'received') | unicode | text | undefined, submitted, accepted, published, corrected, revised  |
| provider.agent | Free-text field for identifying the API client used to create the notification | unicode | free text |  |
| provider.ref | Publisher's own identifier for the notification | unicode | free text |  |
| content.packaging_format  | Package format identifier for the associated binary content (example: "https://pubrouter.jisc.ac.uk/FilesAndJATS") | unicode | URL |  |
| links.type | Keyword for type of resource (e.g. splash, fulltext) - no restrictions on use in this version of the system | unicode |  |  |
| links.format | mimetype of the resource available at the URL (e.g. text/html) | unicode |  |  |
| links.url | URL to the associated resource.  All URLs provided by publishers should be publicly accessible for a minimum of 3 months | unicode | URL | | 
| metadata.journal.title * | Title of the journal or publication | unicode |  |  |
| metadata.journal.abbrev_title | Abbreviated form of journal/publication title | unicode |  |  |
| metadata.journal.volume | Number of a journal (or other document) within a series | unicode |  |  |
| metadata.journal.issue | Issue number of a journal, or in rare instances, a book | unicode |  |  |
| metadata.journal.publisher * | Publisher(s) of the article (Array field) | unicode |  |  |
| metadata.journal.identifier.type * |  Identifier type (e.g. "issn", "eissn", "pissn", "doi") - no vocabulary for this field in this version of the system | unicode |  |  |
| metadata.journal.identifier.id * | Identifier of the journal / publication (e.g. the ISSN number) | unicode |  |  |
| metadata.article.title * | Title of the Article| unicode |  |  |
| metadata.article.subtitle | Subtitle (if any) of the Article | unicode |  |  |
| metadata.article.type | Type or kind of article (e.g. 'research', 'commentary', 'review', 'case', or 'calendar') | unicode |  |  |
| metadata.article.version * | Specifies article version that meta-data relates to, preferably expressed using NISO scheme (http://www.niso.org/publications/rp/RP-8-2008.pdf) (e.g. AO, SMUR, AM, P, VoR, CVoR, EVoR, C/EVoR)   | unicode |  |  |
| metadata.article.start_page | Article start page  | unicode |  |  |
| metadata.article.end_page | Article end page| unicode |  |  |
| metadata.article.page_range | Text describing discontinuous pagination | unicode |  |  |
| metadata.article.e_num | Electronic article number (an alternative to page_range / start_page / end_page) | unicode |  |  |
| metadata.article.num_pages | Number of pages | unicode |  |  |
| metadata.article.language | Language(s) that article is published in (Array field) | unicode |  |  |
| metadata.article.abstract | Article abstract | unicode |  |  |
| metadata.article.identifier.type * | Type of identifier (e.g. 'DOI') | unicode |  |  |
| metadata.article.identifier.id * | Article identfier value (e.g. DOI number) | unicode |  |  |
| metadata.article.subject | Subject classification(s) / keyword(s) (Array field) | unicode |  |  |
| metadata.author.type | Type of author, typically "author" or "corresp" (for corresponding author) | unicode |  |  |
| metadata.author.name.firstname * | Author's firstname(s) - space separated if more than one | unicode |  |  |
| metadata.author.name.surname * | Author's surname (lastname) | unicode |  |  |
| metadata.author.name.fullname | Full name - preferably expressed as "Surname, Firstname(s)" | unicode |  |  |
| metadata.author.name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) | unicode |  |  |
| metadata.author.organisation_name | Name of organisation if author is an organisation  | unicode |  |  |
| metadata.author.identifier.type * | Type of identifier (e.g. ORCID, email) | unicode |  |  |
| metadata.author.identifier.id * | Author identfier value (e.g. ORCID number, email address) | unicode |  |  |
| metadata.author.affiliations | Array of author organisational affiliation objects |  |  |  |
| metadata.author.affiliations.identifier | Array of organisation Ids such as ROR or GRID |  |  |  |
| metadata.author.affiliations.identifier.type | Type of identifier (e.g. 'ROR') | unicode |  |  |
| metadata.author.affiliations.identifier.id | Organisation identfier value (e.g. ROR value) | unicode |  |  |
| metadata.author.affiliations.org | Name of institution (may include department) | unicode  |  |  |
| metadata.author.affiliations.dept | Name of department (if not included in preceding *org* field) | unicode |  |  |
| metadata.author.affiliations.street | Street address | unicode |  |  |
| metadata.author.affiliations.city | City | unicode |  |  |
| metadata.author.affiliations.state | State or Country | unicode |  |  |
| metadata.author.affiliations.postcode | Postal code | unicode |  |  |
| metadata.author.affiliations.country | Country | unicode |  |  |
| metadata.author.affiliations.country_code | Country code (ISO 3166 2 character codes) | unicode |  |  |
| metadata.author.affiliations.raw | Unstructured affiliation string - use where structured information not available | unicode |  |  |
| metadata.contributor.type | Type of contributor (e.g. editor) | unicode |  |  |
| metadata.contributor.name.firstname | Contributor's firstname(s) - space separated if more than one | unicode |  |  |
| metadata.contributor.name.surname | Contributor's surname (lastname) | unicode |  |  |
| metadata.contributor.name.fullname | Full name - preferably expressed as "Surname, Firstname(s)" | unicode |  |  |
| metadata.contributor.name.suffix | Qualifiers that follow name (such as Senior/Sr, Junior/Jr, 3rd etc.) | unicode |  |  |
| metadata.contributor.organisation_name |Name of organisation if contributor is an organisation  | unicode |  |  |
| metadata.contributor.identifier.type | Type of identifier (e.g. ORCID, email) | unicode |  |  |
| metadata.contributor.identifier.id | Contributor identfier value (e.g. ORCID number, email address) | unicode |  |  |
| metadata.contributor.affiliations | Array of contributor organisational affiliation objects |  |  |  |
| metadata.contributor.affiliations.identifier | Array of organisation Ids such as ROR or GRID |  |  |  |
| metadata.contributor.affiliations.identifier.type | Type of identifier (e.g. 'ROR') | unicode |  |  |
| metadata.contributor.affiliations.identifier.id | Organisation identfier value (e.g. ROR value) | unicode |  |  |
| metadata.contributor.affiliations.org | Name of institution (may include department) | unicode  |  |  |
| metadata.contributor.affiliations.dept | Name of department (if not included in preceding *org* field) | unicode |  |  |
| metadata.contributor.affiliations.street | Street address | unicode |  |  |
| metadata.contributor.affiliations.city | City | unicode |  |  |
| metadata.contributor.affiliations.state | State or Country | unicode |  |  |
| metadata.contributor.affiliations.postcode | Postal code | unicode |  |  |
| metadata.contributor.affiliations.country | Country | unicode |  |  |
| metadata.contributor.affiliations.country_code | Country code (ISO 3166 2 character codes) | unicode |  |  |
| metadata.contributor.affiliations.raw | Unstructured affiliation string - use where structured information not available | unicode |  |  |
| metadata.accepted_date | Date publication accepted for publication | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| metadata.publication_date.publication_format | Format of publication (print, electronic) | unicode | print or electronic |  |
| metadata.publication_date.date | Date of publication | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| metadata.publication_date.year | Year of publication | unicode | YYYY |  |
| metadata.publication_date.month | Month of publication (where known) | unicode | MM |  |
| metadata.publication_date.day | Day of publication (where known) | unicode | DD |  |
| metadata.publication_date.season | Season of publication (e.g. Spring, Third quarter) | unicode |  |  |
| metadata.history_date.date_type | Type of date, e.g. received, accepted, published, published_online, epub, ppub, pub | unicode |  |  |
| metadata.history_date.date | Date (YYYY-MM-DD format) | unicode | YYYY-MM-DD or UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| metadata.publication_status * | Status of publication that this metadata refers to: published, accepted | unicode |  |  |
| metadata.funding.name | Funder name | unicode |  |  |
| metadata.funding.identifier.type | Funder identifier type (e.g "ringold") - no vocabulary for this field in this version of the system | unicode |  |  |
| metadata.funding.identifier.id | Funder identifier (e.g. Ringold ID) | unicode |  |  |
| metadata.funding.grant_numbers | Grant numbers (award identifiers) funding the work of this article | unicode |  |  |
| metadata.embargo.start | Date that embargo starts (YYYY-MM-DD) | unicode |  |  |
| metadata.embargo.end | Date that embargo ends (YYYY-MM-DD) | unicode |  |  |
| metadata.embargo.duration | Embargo duration in DAYS | unicode |  |  |
| metadata.license_ref.title | Title or name of the licence applied to the article; free-text | unicode |  |  |
| metadata.license_ref.type | Type of licence (most likely the same as the title); free-text | unicode |  |  |
| metadata.license_ref.url | URL for information on the licence | unicode | URL |  |
| metadata.license_ref.version | Version of the licence | unicode |  |  |
| metadata.license_ref.start | License start date | unicode |  |  |
| metadata.peer_reviewed | Indicates if article has been peer reviewed | boolean |  | true / false |
| metadata.ack | Acknowledgement text | unicode |  |  |
