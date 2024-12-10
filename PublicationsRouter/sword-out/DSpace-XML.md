# Publications Router DSpace XML Description

This document describes the XML output by Publications Router for ingestion into DSpace repositories via the SWORDv2 interface. 
 
For background information related to the schema see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [DSpace schema](https://wiki.duraspace.org/display/DSDOC4x/Metadata+and+Bitstream+Format+Registries).

The following table lists:
* Column 1 - the XML elements output by Publications Router 
* Column 2 - Publications Router internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note. 

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Conditional phrases are shown within `{curly brackets}` these will be omitted if there is no data to display; within such phrases choices in data to display are indicated by `|` character, the first non-blank data item is displayed. Any other text is output as it appears in the format.

| DC terms | Publications Router Metadata (source) | XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dcterms:bibliographicCitation](http://dublincore.org/documents/dcmi-terms/#terms-bibliographicCitation) | journal.title <br> journal.abbrevTitle <br> journal.volume <br> journal.issue <br> article.start_page  <br> article.end_page  <br> article.page_range <br> article.article_e_num <br> | `<dcterms:bibliographicCitation>[journal.title], volume [journal.volume], issue [journal.issue], page [article.start_page]-[article.end_page] or [article.page_range], article-number [article.article_e_num]</dcterms:bibliographicCitation>`|
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) | journal.publisher | `<dcterms:publisher>[journal.publisher]</dcterms:publisher>` |
| [dcterms:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.identifier.type <br> journal.identifier.id  | `<dcterms:source>[journal.identifier.type]: [journal.identifier.id]</dcterms:source>` |
| [dcterms:type](http://dublincore.org/documents/dcmi-terms/#type) | article.type | `<dcterms:type>[article.type]</dcterms:type>` |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title | `<dcterms:title> [article.title]</dcterms:title>` |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dcterms:language>[article.language]</dcterms:language>` |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract) | article.abstract | `<dcterms:abstract>[article.abstract]</dcterms:abstract>` |
| [dcterms:identifier](http://dublincore.org/documents/dcmi-terms/#terms-identifier) | article.identifier.type <br> article.identifier.id  | `<dcterms:identifier>[article.identifier.type]: [article.identifier.id]</dcterms:identifier>` |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) |  article.subject | `<dcterms:subject>[article.subject]</dcterms:subject>` |
| [dcterms:creator](http://dublincore.org/documents/dcmi-terms/#terms-creator) | author.firstname <br> author.surname <br> author.organisation_name <br> author.identifier | `<dcterms:creator>[author.surname], [author.firstname]; [author.identifier.type]: [author.identifier.id]; [author.organisation_name]</dcterms:creator>` |
| [dcterms:contributor](http://dublincore.org/documents/dcmi-terms/#terms-contributor) | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier <br> contributor.type | `<dcterms:contributor>[contributor.type]: [contributor.surname], [contributor.firstname]; [contributor.identifier.type]: [contributor.identifier.id]; [contributor.organisation_name]</dcterms:creator>`  |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) | accepted_date | `<dcterms:dateAccepted>[accepted_date]</dcterms:dateAccepted>` |
| [dcterms:issued](http://dublincore.org/documents/dcmi-terms/#terms-issued) | publication_date | `<dcterms:issued>[publication_date]</dcterms:issued>` |
| [dcterms:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | embargo.start <br> embargo.end <br> embargo.duration | `<dcterms:rights>Embargo: {starts [embargo.start], }{ends [embargo.end], }{duration [embargo.duration] months from publication}</dcterms:rights>` <br> <sub>Note: One or more of the start, end, duration phrases may be omitted if the relevant data is not available.</sub> |
| [dcterms:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.start <br> article.version | `<dcterms:rights>License for{ [article.version] version of} this article{ starting on [license_ref.start]}: {[license_ref.url]\|[license_ref.title]\|[license_ref.type]}</dcterms:rights>` <br> <sub>Note: The output string contains {conditional phrases} that are included only if the field they contain is not empty; the '\|' character separates alternative values, where the first non-empty value is used.</sub>|
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description) | article.version | `<dcterms:description>Article version: [article.version]</dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | provider.agent | `<dcterms:description>From [provider.agent] via Jisc Publications Router</dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | publication_status | `<dcterms:description>Publication status: [publication_status]</dcterms:description>` | 
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | history_date.date_type <br> history_date.date | `<dcterms:description>History: [history_date.date_type], [history_date.date]</dcterms:description>` |
| [dcterms:dateSubmitted](http://dublincore.org/documents/dcmi-terms/#terms-dateSubmitted)  | history_date.date | `<dcterms:dateSubmitted>[history_date.date]</dcterms:dateSubmitted>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | funding.name <br> funding.grant_numbers <br> funding.identifier | `<dcterms:description>Funder: [funding.name], [funding.identifier.type]: [funding.identifier.id], Grant(s):  [funding.grant_numbers]</dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | peer_reviewed | `<dcterms:description>Peer reviewed: {True\|False}</dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | ack | `<dcterms:description>Acknowledgements:  [ack]</dcterms:description>` |
dcterms:dateSubmitted

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
	<dc:description>Article version: VoR</dc:description>
	<dcterms:description>From BMJ via Jisc Publications Router</dcterms:description>
	<dcterms:creator>Mahajan,Ujjwal M</dcterms:creator>
	<dcterms:creator>Teller,Steffen</dcterms:creator>
	<dcterms:creator>Sendler,Matthias</dcterms:creator>
	<dcterms:creator>Palankar,Raghavendra</dcterms:creator>
	<dcterms:creator>van den Brandt,Cindy</dcterms:creator>
	<dcterms:creator>Schwaiger,Theresa</dcterms:creator>
	<dcterms:creator>Kohn,Jens-Peter</dcterms:creator>
	<dcterms:creator>Ribback,Silvia</dcterms:creator>
	<dcterms:creator>Gluckl,Gunnar</dcterms:creator>
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
        <dcterms:description>Funder: Rotary Club of Eureka; ringgold: rot-club-eurek, Fundref: https://doi.org/10.13039/100008650; Grant(s): BB/34/juwef</dcterms:description>
	<dcterms:description>Peer reviewed: True</dcterms:description>
	<dcterms:description>Acknowledgements: ...acknowledgement text...</dcterms:description>
	<dcterms:description>History: received 2016-01-03, rev-recd 2016-04-01, accepted 2016-04-18, ppub 2016-05, epub 2016-05-12</dcterms:description>
	<dcterms:rights>Licence for VoR version of this article starting on 12-12-2016: https://testing.org/licenses/by/4.0/</dcterms:rights>
	<dcterms:rights>Embargo: starts 2016-05-12, ends 2016-12-12, duration 7 months from publication.</dcterms:rights>
	<dcterms:abstract>Objective Pancreatic ductal adenocarcinoma (PDAC) is one of the most aggressive malignancies and is projected to be the second leading cause of cancer-related death by 2030.</dcterms:abstract>
	<dcterms:issued>2016-05-12</dcterms:issued>
	<dcterms:dateAccepted>2016-04-18</dcterms:dateAccepted>
	<dcterms:bibliographicCitation>Gut, page gutjnl-2016-311393, article-number 311393</dcterms:bibliographicCitation>
	<dcterms:type>article</dcterms:type>
	<dcterms:dateSubmitted>2016-01-03</dcterms:dateSubmitted>
</entry>
```
