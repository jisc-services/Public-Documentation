# PubRouter DSPACE XML Schema Description

This document describes the XML output by PubRouter for ingestion into DSPACE repositories via the SWORDv2 interface. 
 
For information about the schema see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [DSPACE schema](https://wiki.duraspace.org/display/DSDOC4x/Metadata+and+Bitstream+Format+Registries)

The following table lists the PubRouter internal metadata JSON fields (left hand column) and the output XML elements they are transformed to (middle column), with explanatory notes in the last column.

| DC terms | PubRouter Metadata | XML Template |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dcterms:bibliographicCitation](http://dublincore.org/documents/dcmi-terms/#terms-bibliographicCitation) | journal.title <br> journal.abbrevTitle <br> journal.volume <br> journal.issue <br> article.page_range <br>  | `<dcterms:bibliographicCitation> [journal.title], volume [journal.volume], issue [journal.issue], page [article.page_range] </dcterms:bibliographicCitation>` |
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) | journal.publisher | `<dcterms:publisher> [journal.publisher] </dcterms:publisher>` |
| [dcterms:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.identifier.type <br> journal.identifier.id  | `<dcterms:source> [journal.identifier.type]: [journal.identifier.id] </dcterms:source>` |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title | `<dcterms:title> [article.title] </dcterms:title>` |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dcterms:language> [article.language] </dcterms:language>` |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract) | article.abstract | `<dcterms:abstract> [article.abstract] </dcterms:abstract>` |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) |  article.subject | `<dcterms:subject> [article.subject] </dcterms:subject>` |
| [dcterms:creator](http://dublincore.org/documents/dcmi-terms/#terms-creator) | author.firstname <br> author.surname <br> author.organisation_name <br> author.identifier | `<dcterms:creator> [author.surname], [author.firstname]; [author.identifier.type]: [author.identifier.id]; [author.organisation_name] </dcterms:creator>` |
| [dcterms:contributor](http://dublincore.org/documents/dcmi-terms/#terms-contributor) | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier <br> contributor.type | `<dcterms:contributor> [contributor.type]: [contributor.surname], [contributor.firstname]; [contributor.identifier.type]: [contributor.identifier.id]; [contributor.organisation_name] </dcterms:creator>`  |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) | accepted_date | `<dcterms:dateAccepted> [accepted_date] </dcterms:dateAccepted>` |
| [dcterms:issued](http://dublincore.org/documents/dcmi-terms/#terms-issued) | publication_date | `<dcterms:issued> [publication_date] </dcterms:issued>` |
| [dcterms:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | embargo.start <br> embargo.end <br> embargo.duration | `<dcterms:rights> Embargo: starts [embargo.start], ends [embargo.end], duration [embargo.duration] months from publication </dcterms:rights>` |
| [dcterms:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.version <br> license_ref.start <br> article.version | `<dcterms:rights> License for [article.version] version of this article: starting on: [license_ref.start] [license_ref.url] [license_ref.type] [license_ref.title] </dcterms:rights>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description) | article.version | `<dcterms:description> Version: [article.version] </dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | provider_agent | `<dcterms:description> From [provider_agent] via Jisc Publications Router. </dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | publication_status | `<dcterms:description> Publication status: [publication_status] </dcterms:description>` | 
|[dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | history_date.date_type <br> history_date.date | `<dcterms:description> History: [history_date.date_type], [history_date.date] </dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | funding.name <br> funding.grant_number <br> funding.identifier | `<dcterms:description> Funder: [funding.name], Grant no: [funding.grant_number], [funding.identifier.type]: [funding.identifier.id] </dcterms:description>` |


## Example XML Output

An example Atom Entry document containing the metadata listed above is shown here

```xml
<?xml version="1.0"?>
<entry xmlns="http://www.w3.org/2005/Atom" 
	   xmlns:dcterms="http://purl.org/dc/terms/">
	<generator uri="http://pubrouter.jisc.ac.uk/python-sword2" version="0.2"/>
	<dcterms:language>en</dcterms:language>
	<dcterms:source>pissn: 0017-5749</dcterms:source>
	<dcterms:source>eissn: 1468-3288</dcterms:source>
	<dcterms:publisher>BMJ Publishing Group</dcterms:publisher>
	<dcterms:description>From Publisher via Jisc Publications Router.</dcterms:description>
	<dcterms:creator>Mahajan,Ujjwal M</dcterms:creator>
	<dcterms:creator>Teller,Steffen</dcterms:creator>
	<dcterms:creator>Sendler,Matthias</dcterms:creator>
	<dcterms:creator>Palankar,Raghavendra</dcterms:creator>
	<dcterms:creator>van den Brandt,Cindy</dcterms:creator>
	<dcterms:creator>Schwaiger,Theresa</dcterms:creator>
	<dcterms:creator>K&#252;hn,Jens-Peter</dcterms:creator>
	<dcterms:creator>Ribback,Silvia</dcterms:creator>
	<dcterms:creator>Gl&#246;ckl,Gunnar</dcterms:creator>
	<dcterms:creator>Evert,Matthias</dcterms:creator>
	<dcterms:creator>Weitschies,Werner</dcterms:creator>
	<dcterms:creator>Hosten,Norbert</dcterms:creator>
	<dcterms:creator>Dombrowski,Frank</dcterms:creator>
	<dcterms:creator>Delcea,Mihaela</dcterms:creator>
	<dcterms:creator>Weiss,Frank-Ulrich</dcterms:creator>
	<dcterms:creator>Lerch,Markus M</dcterms:creator>
	<dcterms:creator>Mayerle,Julia</dcterms:creator>
	<dcterms:title>Tumour-specific delivery of siRNA-coupled superparamagnetic iron oxide nanoparticles, targeted against PLK1, stops progression of pancreatic cancer</dcterms:title>
	<dcterms:identifier>publisher-id: gutjnl-2016-311393</dcterms:identifier>
	<dcterms:identifier>doi: 10.1136/gutjnl-2016-311393</dcterms:identifier>
	<dcterms:subject>Pancreas</dcterms:subject>
	<dcterms:subject>PANCREATIC CANCER</dcterms:subject>
	<dcterms:description>Publication status: Published</dcterms:description>
	<dcterms:description>History: received 2016-01-03, rev-recd 2016-04-01, accepted 2016-04-18, ppub 2016-05, epub 2016-05-12</dcterms:description>
	<dcterms:rights>Licence for this article: https://testing.org/licenses/by/4.0/ uat lic 3 License uat testing</dcterms:rights>
	<dcterms:rights>Embargo: starts 2016-05-12, ends 2016-12-12, duration 7 months from publication.</dcterms:rights>
	<dcterms:abstract>Objective Pancreatic ductal adenocarcinoma (PDAC) is one of the most aggressive malignancies and is projected to be the second leading cause of cancer-related death by 2030.</dcterms:abstract>
	<dcterms:issued>2016-05-12</dcterms:issued>
	<dcterms:dateAccepted>2016-04-18</dcterms:dateAccepted>
	<dcterms:bibliographicCitation>Gut, page gutjnl-2016-311393</dcterms:bibliographicCitation>
	<dcterms:type>article</dcterms:type>
	<dcterms:dateSubmitted>2016-01-03</dcterms:dateSubmitted>
</entry>
```
