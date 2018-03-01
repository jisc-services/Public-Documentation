# PubRouter EPrints-RIOXX XML Schema Description



This document describes the crosswalk from the notification metadata fields received via the JPER API to the
 XML format supplied to repositories via SWORDv2.
 
This crosswalk maps values in the metadata to terms in the PubRouter.xsd document. The PubRouter.xsd document is made up of a mixture of Rioxx terms, DC terms and PubRouter defined terms. Priority is granted to DC terms, then additionally Rioxx terms will be populated. Finally, PubRouter defined terms. 
For information about the schemas see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX](http://rioxx.net/v2-0-final/)
* [PubRouter.xsd](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd)

The following table lists the various terms which the xwalk will populate, then the relevant metadata items which will populate these terms along with the template of the stated xml term. This template will be setup such that elements inside \<tags> will be XML terms and elements in [square brackets] will be from our metadata fields. 

| Terms | PubRouter Metadata fields | XML template |
|:-----------------------------|:-----------------------|:--------------------------------------------------------------------------------------------------------------|
| [pr:source](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L133) | journal.title<br> journal.abbrevTitle<br>  journal.volume<br>  journal.issue| `<pr:source volume=[journal.volume] issue=[journal.issue]> [journal.title] + [journal.abbrevTitle] </pr:source>` |
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher)  | journal.publisher | `<dcterms:publisher> [journal.publisher] </dcterms:publisher>` |
| [pr:source_id](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L151) | journal.identifier.id<br> journal.identifier.type | `<pr:source_id type=[journal.identifier.type]> [journal.identifier.id] </pr:source_id>` |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title |  `<dcterms:title> [article.title] </dcterms:title>` |
| [dcterms:type](http://dublincore.org/documents/dcmi-terms/#terms-type) | article.type | `<dcterms:type> [article.type] </dcterms:type>` |
| [rioxxterms:type](http://www.rioxx.net/profiles/v2-0-final/) | article.type |  `<rioxxterms:type> [article.type] </rioxxterms:type> `|
| [rioxxterms:version](http://www.rioxx.net/profiles/v2-0-final/) | article.version |  `<rioxxterms:version> [article.version] </rioxxterms:version>`|
| [pr:start_page](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L173) | article.start_page |  `<pr:start_page> [article.start_page] </pr:start_page>` |
| [pr:end_page](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L180) | article.end_page |  `<pr:end_page> [article.end_page] </pr:end_page>` |
| [pr:page_range](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L187) | article.page_range |   `<pr:page_range> [article.page_range] </pr:page_range>` |
| [pr:num_pages](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L194) | article.num_pages |  `<pr:num_pages> [article.num_pages] </pr:num_pages>` |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dcterms:language> [article.language] </dcterms:language>` |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract) | article.abstract | `<dcterms:abstract> [article.abstract] </dcterms:abstract>` |
| [pr:identifier](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L203) | article.identifier.id<br> article.identifier.type | `<pr:identifier type=[article.identifier.type]> [article.identifier.id] </pr:identifier>` |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) | article.subject | `<dcterms:subject> [article.subject] </dcterms:subject>` |
| [pr:author](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L30) | author.name<br> author.organisation_name<br> author.identifier<br> author.email author.type | `<pr:author>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:type>[author.type]</pr:type> `<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:id type=[author.identifier.type]>[author.identifier.id]</pr:id>`<br>  &nbsp;&nbsp;&nbsp;&nbsp; `<pr:email>[author.email]</pr:email>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:firstnames>[author.name.firstname]</pr:firstnames>`<br> &nbsp;&nbsp;&nbsp;&nbsp;  `<pr:surname>[author.name.surname]</pr:surname>` <br> `</pr:author>` |
| [pr:contributor](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L30) | contributor.type<br> contributor.name<br> contributor.organisation_name<br> contributor.identifier | `<pr:contributor>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:type>[contributor.type]</pr:type>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:id type=[author.identifier.type]>[author.identifier.id]</pr:id>`<br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:email>[contributor.email]</pr:email>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:surname>[contributor.name.surname]</pr:surname>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:firstnames>[contributor.name.firstname]</pr:firstnames>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<pr:org_name>[contributor.organisation_name]</pr:org_name>` <br> `</pr:contributor>` |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) | accepted_date | `<dcterms:dateAccepted> [accepted_date] </dcterms:dateAccepted>` | 
| [rioxxterms:publication_date](http://www.rioxx.net/profiles/v2-0-final/) | publication_date | `<rioxxterms:publication_date> [publication_date] </rioxxterms:publication_date>` |
| [dcterms:medium](http://dublincore.org/documents/dcmi-terms/#terms-medium) | publication_date.publication_format | `<dcterms:medium> [publication_date.publication_format] </dcterms:medium>` | 
| [pr:history_date](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L224) | history_date.type<br> history_date.date | `<pr:history_date type=[history_date.type]> [history_date.date] </pr:history_date>` |
| [rioxxterms:project](http://www.rioxx.net/profiles/v2-0-final/) | project.name<br> project.identifier<br>project.grant_number | `<rioxxterms:project funder_id=[project.identifier] funder_name=[project.name]> [project.grant_number] </rioxxterms:project>` |
| [pr:embargo](https://github.com/jisc-services/Public-Documentation/blob/b69603c7bf410e2a812c06d6facdaed509174968/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L275) | embargo.start<br> embargo.end | `<pr:embargo start_date=[embargo.start] end_date=[embargo.end]></pr:embargo>` |



## Example XML Output

An example Entry document containing the metadata listed above is shown here

```xml
<?xml version="1.0"?>
<entry xmlns:ali="http://www.niso.org/schemas/ali/1.0/" xmlns:dcterms="http://purl.org/dc/terms/" xmlns:pr="http://pubrouter.jisc.ac.uk/rioxxplus/" xmlns:rioxxterms="http://www.rioxx.net/schema/v2.0/rioxx/">
	<pr:download_link url="http://testing.no.real.jisc.ac.uk/api/v1/notification/1234567890/content/1" format="text/html" filename="1" primary="false"/>
	<pr:relation url="http://testing.no.real.jisc.ac.uk/api/v1/notification/1234567890/content/2" format="application/pdf"/>
	<pr:download_link url="http://testing.no.real.jisc.ac.uk/api/v1/notification/1234567890/content/2" format="application/pdf" public="true" filename="2.pdf" primary="true"/>
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
	<pr:start_page>Page number on which a document starts</pr:start_page>
	<pr:end_page>Page number on which a document ends</pr:end_page>
	<pr:page_range>Text describing discontinuous pagination.</pr:page_range>
	<pr:num_pages>Total number of pages </pr:num_pages>
	<dcterms:language>en</dcterms:language>
	<dcterms:language/>
	<dcterms:abstract>Abstract of the work </dcterms:abstract>
	<pr:identifier type="doi">55.aa/base.1</pr:identifier>
	<rioxxterms:version_of_record>55.aa/base.1</rioxxterms:version_of_record>
	<dcterms:subject>science</dcterms:subject>
	<dcterms:subject>technology</dcterms:subject>
	<dcterms:subject>arts</dcterms:subject>
	<dcterms:subject>medicine</dcterms:subject>
	<dcterms:dateAccepted>2014-09-01</dcterms:dateAccepted>
	<rioxxterms:publication_date>2015-01-01</rioxxterms:publication_date>
	<pr:history_date type="submitted">2014-07-03</pr:history_date>
	<rioxxterms:project funder_id="ringold:bbsrcid" funder_name="BBSRC">BB/34/juwef</rioxxterms:project>
	<pr:license url="http://url" start_date="12-11-2016" version="1">licence title</pr:license>
	<pr:embargo start_date="2015-01-01T00:00:00Z" end_date="2016-01-01T00:00:00Z"/>
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
