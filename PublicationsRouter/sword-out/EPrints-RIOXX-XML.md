# Publications Router EPrints-RIOXX XML Description

This document describes the RIOXXplus XML (Version 2) output by Publications Router for ingestion via the SWORDv2 interface into Eprints repositories configured with RIOXX and RIOXXplus 2 plugins.

Background information:
* [eprints-rioxx.xsd](./pubrouter-xsd/eprints-rioxx.xsd) - Eprints-RIOXX schema definition
* [Dubin Core dcterms](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX XML profile](http://rioxx.net/v2-0-final/)

The following table lists:
* Column 1 - the XML elements output by Publications Router
* Column 2 - Publications Router internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note
* Column 4 - cardinality (number of elements permitted) and notes.

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Any other text is output as it appears in the format.  Text derived through processing is indicated in `( parentheses )`.


| XML Element Terms | Publications Router Metadata | XML Format | Cardinality & Notes |
|:-----------------------------|:-----------------------|:------------------------------------------------------------------------------|:----------------------------------|
| [pr:note](./pubrouter-xsd/eprints-rioxx.xsd#L96) | Information to be displayed in Eprints Additional Information public field, including the following where available: <br> - Article version: article.version <br> - Embargo end date: embargo.end<br> - Provider agent: provider.agent <br> - History dates: history_date.* <br> - Licence information: license_ref.* <br> - Peer review status: peer_reviewed <br> - Acknowledgement text: ack <br> | `<pr:note>[text to display]</pr:note>` | {0..n} |
| [pr:comment](./pubrouter-xsd/eprints-rioxx.xsd#L103) | Information to be displayed in Eprints Comments & Suggestions private field | `<pr:comment>[text to display]</pr:comment>` | {0..1} |
| [pr:relation](./pubrouter-xsd/eprints-rioxx.xsd#L110) | links.format<br>links.url<br>links.packaging| `<pr:relation url=[links.url] format=[links.format] packaging=[links.packaging]>[text to display]</pr:relation>` | {0..n}<br>If the element has no text to display, then Eprints will display the URL.  |
| [pr:source](./pubrouter-xsd/eprints-rioxx.xsd#L130) | journal.title<br>journal.volume<br>journal.issue| `<pr:source volume=[journal.volume] issue=[journal.issue]>[journal.title]</pr:source>` | {0..1} |
| [pr:source_id](./pubrouter-xsd/eprints-rioxx.xsd#L148) | journal.identifier.id<br> journal.identifier.type | `<pr:source_id type=[journal.identifier.type]>[journal.identifier.id]</pr:source_id>` | {1..n}<br>e.g. ISSN |
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher)  | journal.publisher | `<dcterms:publisher>[journal.publisher] </dcterms:publisher>` | {0..1}<br>Publisher name |
| [dcterms:format](http://dublincore.org/documents/dcmi-terms/#terms-format)  | links.format | `<dcterms:format>[links.format]</dcterms:format>` | {0..n}<br>Mime type(s) of article full-text files associated with metadata notification |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title |  `<dcterms:title>[article.title]</dcterms:title>` | {1..1} |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dcterms:language>[article.language]</dcterms:language>` | {0..n}<br>Controlled value, conforming to ISO 639–3 (2 or 3 characters) E.g. “en” or “eng” for English |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract) | article.abstract | `<dcterms:abstract>[article.abstract]</dcterms:abstract>` | {0..1} |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) | article.subject | `<dcterms:subject>[article.subject] </dcterms:subject>` | {0..n}<br>Subject keywords |
| [dcterms:medium](http://dublincore.org/documents/dcmi-terms/#terms-medium) | publication_date. publication_format | `<dcterms:medium> [publication_date.publication_format] </dcterms:medium>` |  {0..1}<br>print or electronic |
| [dcterms:type](http://dublincore.org/documents/dcmi-terms/#terms-type) | article.type | `<dcterms:type>[article.type]</dcterms:type>` | {0..1}<br>Resource type (uncontrolled list) |
| [rioxxterms:type](http://www.rioxx.net/profiles/v2-0-final/) | article.type |  `<rioxxterms:type>[article.type]</rioxxterms:type> `| {0..1}<br>Resource type (from controlled list - rioxxterms:typeList) |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) | accepted_date | `<dcterms:dateAccepted>[accepted_date]</dcterms:dateAccepted>` | {0..1}<br>Accepted date: YYYY-MM-DD format |
| [rioxxterms:publication_date](http://www.rioxx.net/profiles/v2-0-final/) | publication_date | `<rioxxterms:publication_date>[publication_date]</rioxxterms:publication_date>` | {0..1}<br>Possible formats: YYYY-MM-DD \| YYYY-MM \| YYYY \| YYYY, season |
| [rioxxterms:version](http://www.rioxx.net/profiles/v2-0-final/) | article.version |  `<rioxxterms:version>[article.version]</rioxxterms:version>` | {0..1}<br>Version of article (controlled value from rioxxterms:versionList) |
| [rioxxterms:version_of_record](http://www.rioxx.net/profiles/v2-0-final/) | article.identifier.type<br>article.identifier.id |  `<rioxxterms:version>[article.version]</rioxxterms:version>` | {0..1}<br>DOI where version is one of VoR \| CVoR \| EVoR \| C/EVoR |
| [rioxxterms:project](http://www.rioxx.net/profiles/v2-0-final/) | funding.name<br> project.identifier<br>funding.grant_numbers | `<rioxxterms:project funder_id=[funding.identifier.type]:[funding.identifier.id] funder_name=[funding.name]>[funding.grant_numbers] </rioxxterms:project>` | {0..n}<br>Note the funder_id attribute holds a compound string of general format "type:id" e.g. "FundRef:10.13039/100000002" |
| [ali:license_ref](./pubrouter-xsd/eprints-rioxx.xsd#L252) | license_ref.url<br>license_ref.start | `<ali:license_ref start_date=[license_ref.start]>[license_ref.url]</ali:license_ref>` | {0..1}<br>Date format: YYYY-MM-DD |
| [pr:embargo](./pubrouter-xsd/eprints-rioxx.xsd#L272) | embargo.start<br> embargo.end<br>embargo.duration | `<pr:embargo start_date=[embargo.start] end_date=[embargo.end]></pr:embargo>` | {0..1}<br>At least one of attributes start \| end must be present, format: YYYY-MM-DD |
| [pr:start_page](./pubrouter-xsd/eprints-rioxx.xsd#L170) | article.start_page <br> article.article_e_num <br>| `<pr:start_page>[article.start_page]</pr:start_page>` <br><br>Or, if article.page_range is absent: <br> `<pr:start_page>[article.article_e_num]</pr:start_page>` | {0..1} |
| [pr:end_page](./pubrouter-xsd/eprints-rioxx.xsd#L177) | article.end_page | `<pr:end_page>[article.end_page]</pr:end_page>` | {0..1} |
| [pr:page_range](./pubrouter-xsd/eprints-rioxx.xsd#L184) | article.page_range | `<pr:page_range>[article.page_range]</pr:page_range>` | {0..1} |
| [pr:num_pages](./pubrouter-xsd/eprints-rioxx.xsd#L191) | article.num_pages |  `<pr:num_pages>[article.num_pages]</pr:num_pages>` | {0..1} |
| [pr:identifier](./pubrouter-xsd/eprints-rioxx.xsd#L200) | article.identifier.id<br> article.identifier.type | `<pr:identifier type=[article.identifier.type]>[article.identifier.id]</pr:identifier>` | {0..n}<br>Identifier such as DOI URI or Pubmed Id |
| [pr:history_date](./pubrouter-xsd/eprints-rioxx.xsd#L221) | history_date.type<br> history_date.date | `<pr:history_date type=[history_date.type]>[history_date.date]</pr:history_date>` | {0..n}<br>Any publishing event dates, YYYY-MM-DD |
| [pr:author](./pubrouter-xsd/eprints-rioxx.xsd#L288) | author.type<br>author.name<br>author.organisation_name<br>author.identifier<br>author.email | `<pr:author>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:type>[author.type]</pr:type> `<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:id type=[author.identifier.type]>[author.identifier.id]</pr:id>`<br>  &nbsp;&nbsp;&nbsp;&nbsp; `<pr:email>[author.email]</pr:email>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:surname>[author.name.surname]</pr:surname>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:firstnames>[author.name.firstname]</pr:firstnames>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:suffix>[author.name.suffix]</pr:suffix>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:org_name>[author.organisation_name]</pr:org_name>`<br> `</pr:author>` | {0..n}<br>Multiple authors; any author may have multiple Ids and/or Emails|
| [pr:contributor](./pubrouter-xsd/eprints-rioxx.xsd#L30) |  contributor.type<br>contributor.name<br>contributor.organisation_name<br>contributor.identifier<br>contributor.email | `<pr:contributor>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:type>[contributor.type]</pr:type>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:id type=[author.identifier.type]>[author.identifier.id]</pr:id>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:email>[contributor.email]</pr:email>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:surname>[contributor.name.surname]</pr:surname>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:firstnames>[contributor.name.firstname]</pr:firstnames>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:org_name>[contributor.organisation_name]</pr:org_name>` <br> `</pr:contributor>` | {0..n} |
| [pr:download_link](./pubrouter-xsd/eprints-rioxx.xsd#L302) | links.format<br>links.url<br>links.packaging  | `<pr:download_link url=[links.url] filename=(derived from links.url) format=[links.format] public=(true\|false) packaging=[links.packaging] primary=(true\|false) set_details=(true\|false) ></pr:download_link>` | {0..n}<br>Links to full-text files; *public* attribute determines if displayable or not; *primary* attribute determines listing precedence (true items are displayed first); 'set_details' attrib determines if version, license & embargo information is to be associated with file |


## Example XML Output

An example Entry document containing the metadata listed above is shown here.

```xml
<?xml version="1.0"?>
<entry xmlns="http://www.w3.org/2005/Atom" xmlns:ali="http://www.niso.org/schemas/ali/1.0/" xmlns:dcterms="http://purl.org/dc/terms/" xmlns:pr="http://pubrouter.jisc.ac.uk/rioxxplus/v2.0/" xmlns:rioxxterms="http://www.rioxx.net/schema/v2.0/rioxx/">
	<pr:download_link url="http://dummy.jisc.ac.uk/api/v1/notification/1234567890/content/1" format="text/html" filename="1" primary="false"/>
	<pr:note>** Article version: VoR ** Embargo end date: 22-08-2018 ** From Publisher via Jisc Publications Router ** Licence for VoR version of this article starting on 22-08-2018: https://testing.org/licenses/by/4.0/ ** Peer reviewed: TRUE ** Acknowledgements: ...acknowledgement text...</pr:note>
	<pr:comment>Some text to display in Eprints privately visible Comments & Suggestions field</pr:comment>
	<pr:relation url="https:/dummy.jisc.ac.uk/api/v3/notification/1e28c474a04ea8260/content/eprints-rioxx/pone.12345.pdf" format="application/pdf"/>
	<pr:download_link url="https:/dummy.jisc.ac.uk/api/v3/notification/1e28c474a04ea8260/content/eprints-rioxx/pone.12345.pdf" filename="pone.12345.pdf" format="application/pdf" primary="true"  set_details="true"/>
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
	<ali:license_ref start_date="2018-08-22">https://creativecommons.org/licenses/by/4.0/</ali:license_ref>
	<pr:embargo start_date="2017-08-22" end_date="2018-08-22"/>
	<pr:author>
		<pr:type>http://www.loc.gov/loc.terms/relators/AUT</pr:type>
		<pr:id type="orcid">6666-0000-1111-2222</pr:id>
		<pr:email>richard@example.com</pr:email>
		<pr:email>richard2@example.com</pr:email>
		<pr:surname>Williams</pr:surname>
		<pr:firstnames>Richard</pr:firstnames>
	</pr:author>
	<pr:author>
		<pr:type>http://www.loc.gov/loc.terms/relators/AUT</pr:type>
		<pr:id type="orcid">8888-0000-1111-9999</pr:id>
		<pr:email>mark@example.com</pr:email>
		<pr:surname>Smith</pr:surname>
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
