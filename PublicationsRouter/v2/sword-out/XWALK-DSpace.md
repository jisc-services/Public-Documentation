# JPER Core Metadata to Dublin Core for DSPACE

This document describes the crosswalk from the notification metadata fields received via the JPER API to the
 XML format supplied to repositories via SWORDv2.
 
For information about the schema see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)

The following table lists the notification fields in the JSON, then the DC terms fields it will be transformed to, and
any notes regarding the transformation:

| PubRouter Metadata | DC terms | Description |
|:-----------------------------:|:-------------------------:|:------------------------------------------------------------:|
| journal.title | bibliographicCitation |  |
| journal.abbrevTitle | bibliographicCitation |  |
| journal.volume | bibliographicCitation |  |
| journal.issue | bibliographicCitation |  |
| journal.publisher | publisher |  |
| journal.identifier.issn | source  | (issn: value) |
| journal.identifier.eissn | source | (eissn: value) |
| journal.identifier.pissn | source  | (pissn: value) |
| journal.identifier.doi | source | (doi: value) |
| article.title | title | A name given to the resource. |
| article.subtitle | title | A name given to the resource. |
| article.type | type | The nature or genre of the resource. |
| article.version | description |  |
| article.start_page | bibliographicCitation |  |
| article.end_page | bibliographicCitation |  |
| article.page_range | bibliographicCitation |  |
| article.num_pages | ---- |   |
| article.language | language |  |
| article.abstract | abstract |  |
| article.identifier.type | identifier  | (type: id) |
| article.identifier.id | identifier  | (type: id) |
| article.subject | subject |  |
| author.type | creator |  |
| author.name | creator |  |
| author.organisation_name | creator |  |
| author.identifier.orcid | creator |  |
| author.identifier.email | creator |  |
| author.affiliation | ---- |   |
| contributor.type | contributor |  |
| contributor.name | contributor |  |
| contributor.organisation_name | contributor |  |
| contributor.identifier.orcid | contributor |  |
| contributor.identifier.email | contributor |  |
| contributor.affiliation | ---- |   |
| accepted_date | dateAccepted |  |
| publication_date | issued | Date of formal issuance (e.g., publication) of the resource. |
| history_date.type | description | History: {type} {date}, {type} {date}, ...' |
| history_date.date | description dateSubmitted | Dspace uses date type of "submitted" rather than "received" |
| publication_status | description |  |
| project.name  | description | Funding information |
| project.identifier | description | Funding information |
| project.grant_number | description | Funding information |
| embargo.start | rights |  |
| embargo.end | rights |  |
| embargo.duration | rights |  |
| license_ref.title | rights |  |
| license_ref.type | rights |  |
| license_ref.url | rights |  |
| license_ref.version | rights |  |
| license_ref.start | rights |  |
| provider_agent | description | From  {provider_agent} via Jisc Publications Router.' |

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
