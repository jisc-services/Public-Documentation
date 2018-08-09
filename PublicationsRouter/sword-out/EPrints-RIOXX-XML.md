# PubRouter EPrints-RIOXX XML Description

This document describes the RIOXXplus XML output by PubRouter for ingestion via the SWORDv2 interface into Eprints repositories configured with RIOXX and RIOXXplus plugins.

Background information:
* [eprints-rioxx.xsd](./pubrouter-xsd/eprints-rioxx.xsd) - Eprints-RIOXX schema definition
* [Dubin Core dcterms](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX XML profile](http://rioxx.net/v2-0-final/)

The following table lists:
* Column 1 - the XML elements output by PubRouter
* Column 2 - PubRouter internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note
* Column 4 - cardinality (number of elements permitted) and notes.

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Any other text is output as it appears in the format.  Text derived through processing is indicated in `( parentheses )`.


| XML Element Terms | PubRouter Metadata | XML Format | Cardinality & Notes |
|:-----------------------------|:-----------------------|:------------------------------------------------------------------------------|:----------------------------------|
| [pr:source](./pubrouter-xsd/eprints-rioxx.xsd#L130) | journal.title<br>journal.volume<br>journal.issue| `<pr:source volume=[journal.volume] issue=[journal.issue]>[journal.title]</pr:source>` | {0..1} |
| [pr:source_id](./pubrouter-xsd/eprints-rioxx.xsd#L148) | journal.identifier.id<br> journal.identifier.type | `<pr:source_id type=[journal.identifier.type]>[journal.identifier.id]</pr:source_id>` | {1..n} e.g. ISSN |
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher)  | journal.publisher | `<dcterms:publisher>[journal.publisher] </dcterms:publisher>` | {0..1} Publisher name |
| [dcterms:format](http://dublincore.org/documents/dcmi-terms/#terms-format)  | links.format | `<dcterms:format>[links.format]</dcterms:format>` | {0..n} Mime type(s) of article full-text files associated with metadata notification |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title |  `<dcterms:title>[article.title]</dcterms:title>` | {1..1} |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dcterms:language>[article.language]</dcterms:language>` | {0..n} Controlled value, conforming to ISO 639–3 (2 or 3 characters) E.g. “en” or “eng” for English |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract) | article.abstract | `<dcterms:abstract>[article.abstract]</dcterms:abstract>` | {0..1} |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) | article.subject | `<dcterms:subject>[article.subject] </dcterms:subject>` | {0..n} Subject keywords |
| [dcterms:medium](http://dublincore.org/documents/dcmi-terms/#terms-medium) | publication_date. publication_format | `<dcterms:medium> [publication_date.publication_format] </dcterms:medium>` |  {0..1} print or electronic |
| [dcterms:type](http://dublincore.org/documents/dcmi-terms/#terms-type) | article.type | `<dcterms:type>[article.type]</dcterms:type>` | {0..1} Resource type (uncontrolled list) |
| [rioxxterms:type](http://www.rioxx.net/profiles/v2-0-final/) | article.type |  `<rioxxterms:type>[article.type]</rioxxterms:type> `| {0..1} Resource type (from controlled list - rioxxterms:typeList) |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) | accepted_date | `<dcterms:dateAccepted>[accepted_date]</dcterms:dateAccepted>` | {0..1} Accepted date: YYYY-MM-DD format |
| [rioxxterms:publication_date](http://www.rioxx.net/profiles/v2-0-final/) | publication_date | `<rioxxterms:publication_date>[publication_date]</rioxxterms:publication_date>` | {0..1} Possible formats: YYYY-MM-DD \| YYYY-MM \| YYYY \| YYYY, season |
| [rioxxterms:version](http://www.rioxx.net/profiles/v2-0-final/) | article.version |  `<rioxxterms:version>[article.version]</rioxxterms:version>` | {0..1} Version of article (controlled value from rioxxterms:versionList) |
| [rioxxterms:version_of_record](http://www.rioxx.net/profiles/v2-0-final/) | article.identifier.type<br>article.identifier.id |  `<rioxxterms:version>[article.version]</rioxxterms:version>` | {0..1} DOI where version is one of VoR \| CVoR \| EVoR |
| [rioxxterms:project](http://www.rioxx.net/profiles/v2-0-final/) | funding.name<br> project.identifier<br>funding.grant_number | `<rioxxterms:project funder_id=[funding.identifier.type]:[funding.identifier.id] funder_name=[funding.name]>[funding.grant_number] </rioxxterms:project>` | {0..n} Note the funder_id attribute holds a compound string of general format "type:id" e.g. "FundRef:10.13039/100000002" |
| [pr:license](./pubrouter-xsd/eprints-rioxx.xsd#L252) | license_ref.title<br>license_ref.type<br>license_ref.url<br>license_ref.version<br>license_ref.start | `<pr:license start_date=[license_ref.start] url=[license_ref.url] version=[license_ref.version]>[license_ref.title or license_ref.type]</pr:license>` | {0..n} May have many licenses; start format: YYYY-MM-DD |
| [pr:embargo](./pubrouter-xsd/eprints-rioxx.xsd#L272) | embargo.start<br> embargo.end<br>embargo.duration | `<pr:embargo start_date=[embargo.start] end_date=[embargo.end]></pr:embargo>` | {0..1} At least one of attributes start \| end must be present, format: YYYY-MM-DD |
| [pr:start_page](./pubrouter-xsd/eprints-rioxx.xsd#L170) | article.start_page | `<pr:start_page>[article.start_page]</pr:start_page>` | {0..1} |
| [pr:end_page](./pubrouter-xsd/eprints-rioxx.xsd#L177) | article.end_page | `<pr:end_page>[article.end_page]</pr:end_page>` | {0..1} |
| [pr:page_range](./pubrouter-xsd/eprints-rioxx.xsd#L184) | article.page_range | `<pr:page_range>[article.page_range]</pr:page_range>` | {0..1} |
| [pr:num_pages](./pubrouter-xsd/eprints-rioxx.xsd#L191) | article.num_pages |  `<pr:num_pages>[article.num_pages]</pr:num_pages>` | {0..1} |
| [pr:identifier](./pubrouter-xsd/eprints-rioxx.xsd#L200) | article.identifier.id<br> article.identifier.type | `<pr:identifier type=[article.identifier.type]>[article.identifier.id]</pr:identifier>` | {0..n} Identifier such as DOI URI or Pubmed Id |
| [pr:history_date](./pubrouter-xsd/eprints-rioxx.xsd#L221) | history_date.type<br> history_date.date | `<pr:history_date type=[history_date.type]>[history_date.date]</pr:history_date>` | {0..n} Any publishing event dates, YYYY-MM-DD |
| [pr:author](./pubrouter-xsd/eprints-rioxx.xsd#L288) | author.type<br>author.name<br>author.organisation_name<br>author.identifier<br>author.email | `<pr:author>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:type>[author.type]</pr:type> `<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:id type=[author.identifier.type]>[author.identifier.id]</pr:id>`<br>  &nbsp;&nbsp;&nbsp;&nbsp; `<pr:email>[author.email]</pr:email>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:surname>[author.name.surname]</pr:surname>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:firstnames>[author.name.firstname]</pr:firstnames>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:suffix>[author.name.suffix]</pr:suffix>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:org_name>[author.organisation_name]</pr:org_name>`<br> `</pr:author>` | {0..n} Multiple authors; any author may have multiple Ids and/or Emails|
| [pr:contributor](./pubrouter-xsd/eprints-rioxx.xsd#L30) |  contributor.type<br>contributor.name<br>contributor.organisation_name<br>contributor.identifier<br>contributor.email | `<pr:contributor>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:type>[contributor.type]</pr:type>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:id type=[author.identifier.type]>[author.identifier.id]</pr:id>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:email>[contributor.email]</pr:email>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:surname>[contributor.name.surname]</pr:surname>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:firstnames>[contributor.name.firstname]</pr:firstnames>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:org_name>[contributor.organisation_name]</pr:org_name>` <br> `</pr:contributor>` | {0..n} |
| [pr:download_link](./pubrouter-xsd/eprints-rioxx.xsd#L302) | links.format<br>links.url<br>links.packaging  | `<pr:download_link url=[links.url] filename=(derived from links.url) format=[links.format] public=(true\|false) packaging=[links.packaging] primary=(true\|false)></pr:download_link>` | {0..n} Links to full-text files; *public* attribute determines if displayable or not; *primary* attrib determines if version, license & embargo information is to be associated with article |


## Example XML Output

An example Entry document containing the metadata listed above is shown here.

```xml
<?xml version="1.0"?>
<entry xmlns:ali="http://www.niso.org/schemas/ali/1.0/" xmlns:dcterms="http://purl.org/dc/terms/" xmlns:pr="http://pubrouter.jisc.ac.uk/rioxxplus/" xmlns:rioxxterms="http://www.rioxx.net/schema/v2.0/rioxx/">
	<pr:download_link url="http://dummy.jisc.ac.uk/api/v1/notification/1234567890/content/1" format="text/html" filename="1" primary="false"/>
	<pr:relation url="http://dummy.jisc.ac.uk/api/v1/notification/1234567890/content/2" format="application/pdf"/>
	<pr:download_link url="http://dummy.jisc.ac.uk/api/v1/notification/1234567890/content/2" format="application/pdf" public="true" filename="2.pdf" primary="true"/>
	<dcterms:format>application/pdf</dcterms:format>
	<pr:source issue="Issue number of a journal, or in rare instances, a book" volume="Number of a journal (or other document) within a series">Journal of Important Things</pr:source>
	<pr:source_id type="issn">1234-5678</pr:source_id>
	<pr:source_id type="eissn">1234-5678</pr:source_id>
	<pr:source_id type="pissn">9876-5432</pr:source_id>
	<pr:source_id type="doi">10.pp/jit</pr:source_id>
	<dcterms:publisher>Premier Publisher</dcterms:publisher>
	<dcterms:title>Test Article</dcterms:title>
	<rioxxterms:type>Journal Article/Review</rioxxterms:type>
	<dcterms:type>article</dcterms:type>
	<rioxxterms:version>VoR</rioxxterms:version>
	<pr:start_page>7</pr:start_page>
	<pr:end_page>11</pr:end_page>
	<pr:page_range>7-11</pr:page_range>
	<pr:num_pages>4</pr:num_pages>
	<dcterms:language>en</dcterms:language>
	<dcterms:abstract>Abstract of the work </dcterms:abstract>
	<pr:identifier type="doi">55.aa/base.1</pr:identifier>
	<rioxxterms:version_of_record>55.aa/base.1</rioxxterms:version_of_record>
	<dcterms:subject>science</dcterms:subject>
	<dcterms:subject>technology</dcterms:subject>
	<dcterms:subject>arts</dcterms:subject>
	<dcterms:subject>medicine</dcterms:subject>
	<dcterms:dateAccepted>2017-05-11</dcterms:dateAccepted>
	<rioxxterms:publication_date>2017-08-22</rioxxterms:publication_date>
	<pr:history_date type="submitted">2017-02-21</pr:history_date>
	<pr:history_date type="accepted">2017-05-11</pr:history_date>
	<pr:history_date type="published">2017-08-22</pr:history_date>
	<rioxxterms:project funder_id="ringold:bbsrcid" funder_name="BBSRC">BB/34/juwef</rioxxterms:project>
	<pr:license url="https://creativecommons.org/licenses/by/4.0/" start_date="2018-08-22" version="4.0">licence title</pr:license>
	<pr:embargo start_date="2017-08-22" end_date="2018-08-22"/>
	<pr:author>
		<pr:type>http://www.loc.gov/loc.terms/relators/AUT</pr:type>
		<pr:id type="orcid">aaaa-0000-1111-bbbb</pr:id>
		<pr:email>richard@example.com</pr:email>
		<pr:email>richard2@example.com</pr:email>
		<pr:surname>Jones</pr:surname>
		<pr:firstnames>Richard</pr:firstnames>
	</pr:author>
	<pr:author>
		<pr:type>http://www.loc.gov/loc.terms/relators/AUT</pr:type>
		<pr:id type="orcid">dddd-2222-3333-cccc</pr:id>
		<pr:email>mark@example.com</pr:email>
		<pr:surname>MacGillivray</pr:surname>
		<pr:firstnames>Mark</pr:firstnames>
	</pr:author>
	<pr:contributor>
		<pr:type>http://www.loc.gov/loc.terms/relators/EDT</pr:type>
		<pr:email>manolo@example.com</pr:email>
		<pr:surname>Williams</pr:surname>
		<pr:firstnames>Manolo</pr:firstnames>
	</pr:contributor>
</entry>
```
