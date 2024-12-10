# Publications Router DSpace-RIOXX XML Description

This page describes the Dspace-RIOXX XML output by Publications Router for ingest via the SWORDv2 interface into DSpace repositories with the [RIOXX patch](https://github.com/atmire/RIOXX) installed. (Note that Publications Router can output a completely different XML document for "vanilla" DSpace repositories that do not use the RIOXX patch.)
 
For background information related to the schema see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX](http://www.rioxx.net/)
* [DSpace schema](https://wiki.duraspace.org/display/DSDOC4x/Metadata+and+Bitstream+Format+Registries).

## Namespaces ##

The DSpace-RIOXX XML uses 4 namespaces:

| Namespace | URI | Description |
|:----------|:----|:------------|
| ali        | http://www.niso.org/schemas/ali/1.0/ | National Information Standards Organisation's Access and License Information schema  |
| dcterms    | http://purl.org/dc/terms/ | DublinCore (extended) terms schema |
| rioxxterms | http://www.rioxx.net/schema/v2.0/rioxx/ | RIOXX schema |
| pubr       | http://pubrouter.jisc.ac.uk/dspacerioxx/ | Jisc Publications Router DSpace schema |

## XML Elements ##

The table below lists:
* Column 1 - the XML elements output by Publications Router and cardinality:
	* {0..1} – zero or one
	* {0..n} – zero or more
	* {1..1} – exactly one (i.e. a mandatory element)
	* {1..n} – one or more (i.e. a mandatory element)
* Column 2 - Publications Router internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note. 

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Conditional phrases are shown within `{curly brackets}` these will be omitted if there is no data to display; within such phrases choices in data to display are indicated by `|` character, the first non-blank data item is displayed. Any other text is output as it appears in the format.

| XML element and {Cardinality} | Publications Router Metadata (source) | XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dcterms:bibliographicCitation](http://dublincore.org/documents/dcmi-terms/#terms-bibliographicCitation)  <br>{0..1}| journal.title <br> journal.abbrevTitle <br> journal.volume <br> journal.issue <br> article.start_page  <br> article.end_page  <br> article.page_range <br> article.article_e_num <br> | `<dcterms:bibliographicCitation>[journal.title], volume [journal.volume], issue [journal.issue], page [article.start_page]-[article.end_page] or [article.page_range], article-number [article.article_e_num]</dcterms:bibliographicCitation>`|
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) <br>{0..1}| journal.publisher | `<dcterms:publisher>[journal.publisher] </dcterms:publisher>` |
| [dcterms:source](http://dublincore.org/documents/dcmi-terms/#terms-source)<br>{0..n} | journal.identifier.type <br> journal.identifier.id  | `<dcterms:source>[journal.identifier.type]: [journal.identifier.id] </dcterms:source>` |
| [dcterms:type](http://dublincore.org/documents/dcmi-terms/#type)<br>{0..1} | article.type | `<dcterms:type>[article.type]</dcterms:type>` |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title)<br>{1..1} | article.title | `<dcterms:title> [article.title] </dcterms:title>` |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language)<br>{0..1} | article.language | `<dcterms:language>[article.language] </dcterms:language>` |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract)<br>{0..1} | article.abstract | `<dcterms:abstract>[article.abstract] </dcterms:abstract>` |
| [dcterms:identifier](http://dublincore.org/documents/dcmi-terms/#terms-identifier)<br>{0..n} | article.identifier.type <br> article.identifier.id  | `<dcterms:identifier>[article.identifier.type]: [article.identifier.id] </dcterms:identifier>` |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject)<br>{0..n} |  article.subject | `<dcterms:subject>[article.subject]</dcterms:subject>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)<br>{1..1}  | provider.agent | `<dcterms:description>From [provider.agent] via Jisc Publications Router</dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description) <br>{1..n} | history_date.date_type <br> history_date.date | `<dcterms:description>History: [history_date.date_type], [history_date.date] </dcterms:description>` |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) <br>{0..1} | accepted_date | `<dcterms:dateAccepted>[accepted_date]</dcterms:dateAccepted>` |
| [dcterms:issued](http://dublincore.org/documents/dcmi-terms/#terms-issued) <br>{0..1} | publication_date | `<dcterms:issued>[publication_date]</dcterms:issued>` |
| [dcterms:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights)<br>{0..n} | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.start <br> article.version | `<dcterms:rights>License for{ [article.version] version of} this article{ starting on [license_ref.start]}: {[license_ref.url]\|[license_ref.title]\|[license_ref.type]}</dcterms:rights>` <br> <sub>Note: The output string contains {conditional phrases} that are included only if the field they contain is not empty; the '\|' character separates alternative values, where the first non-empty value is used.</sub> |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)<br>{0..1}  | peer_reviewed | `<dcterms:description>Peer reviewed: {True\|False}</dcterms:description>` |
| [dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)<br>{0..1}  | ack | `<dcterms:description>Acknowledgements:  [ack]</dcterms:description>` |
| [rioxxterms:version](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version)<br>{0..1} | article.version | `<rioxxterms:version>[article.version] </rioxxterms:version>` |
| [rioxxterms:version_of_record](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version-of-record)<br>{1..1} | article.identifier.id (DOI) | `<rioxxterms:version_of_record>Version: [article.identifier.id] </rioxxterms:version_of_record>` |
| [rioxxterms:type](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#type)<br>{1..1}  | article.type | `<rioxxterms:type>[article.type]</rioxxterms:type>` |
| [rioxxterms:publication_date](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#publication_date) <br>{0..1} | publication_date | `<rioxxterms:publication_date> [publication_date] </rioxxterms:publication_date>` | 
| [rioxxterms:project](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#project) <br>{0..n} | funding.name <br> funding.identifier <br> funding.grant_numbers  | `<rioxxterms:project funder_name="[funding.name]" {funder_id="[funding.identifier.id]}">[funding.grant_numbers]</rioxxterms:project>`  <br><sub>The `funder_id` attribute will only be present where it is a DOI. </sub>|
| [ali:license_ref](http://www.rioxx.net/schema/v2.0/rioxx/ali_1_0.html#license_ref)<br>{0..1}<br><sub>Note that while RIOXX allows for multiple ali:license_ref elements, the DSpace RIOXX patch can only handle one, so if there are multiple licenses we send an open-access license in preference to a closed license.</sub>| license_ref.url <br> license_ref.url.start  | `<ali:license_ref {start=”[license_ref.url.start]”}> [license_ref.url] </ali:license_ref>` <br><sub>The `start` attribute will only be present where the license has a start date. </sub> |
| [pubr:openaccess_uri](./pubrouter-xsd/dspace-rioxx.xsd)<br>{0..1} | links.url | `<pubr:openaccess_uri>[links.url]</pubr:openaccess_uri>` <br> <sub>A link to publicly accessible article full text (PDF).</sub>| |
| [pubr:author](./pubrouter-xsd/dspace-rioxx.xsd)<br>{0..n} | author.firstname <br> author.surname <br> author.organisation_name <br> author.identifier.orcid <br> author.identifier.email | `<pubr:author id="[author.identifier.id (orcid)]" email="[author.identifier.id (email)]">[author.surname], [author.firstname] OR [author.organisation_name] </pubr:author>` |
| [pubr:contributor](./pubrouter-xsd/dspace-rioxx.xsd)<br>{0..n} | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier.orcid <br> contributor.identifier.email <br> contributor.type | `<pubr:contributor id="[contributor.identifier.id (orcid)] email="[contributor.identifier.id (email)]">[contributor.type]: [contributor.surname], [contributor.firstname]; [contributor.organisation_name] </pubr:contributor>`  |
| [pubr:sponsorship](./pubrouter-xsd/dspace-rioxx.xsd)<br>{0..n} | funding.name <br> funding.grant_numbers <br> funding.identifier | `<pubr:sponsorship>Funder: [funding.name], [funding.identifier.type]: [funding.identifier.id]{;[funding.identifier.type]: [funding.identifier.id]…}, Grant(s):  [funding.grant_numbers]{,[funding.grant_numbers]…} </pubr:sponsorship>` <br><sub>There may be multiple funder identifiers, in form "type: id" separated by semi-colons. There may be multiple grant-numbers, separated by comma.  </sub>|
| [pubr:embargo_date](./pubrouter-xsd/dspace-rioxx.xsd)<br>{0..1} | embargo.end | `<pubr:embargo_date>[embargo.end]</pubr:embargo_date>` <br><sub>Date in YYYY-MM-DD format.</sub> |



## Example Publications Router DSpace-RIOXX XML ##

This is an example Dspace-RIOXX Entry XML document output by Publications Router that contains the metadata elements listed above.

```xml

<?xml version="1.0"?>
<entry xmlns="http://www.w3.org/2005/Atom" 
	xmlns:ali="http://www.niso.org/schemas/ali/1.0/" 
	xmlns:pubr="http://pubrouter.jisc.ac.uk/dspacerioxx/" 
	xmlns:dcterms="http://purl.org/dc/terms/" 
	xmlns:rioxxterms="http://www.rioxx.net/schema/v2.0/rioxx/">
	
	<dcterms:abstract>Exploration of developmental mechanisms classically relies on analysis of pattern regularities. Whether disorders induced by biological noise may carry information on building principles of developmental systems is an important debated question. Here, we addressed theoretically this question using phyllotaxis, the geometric arrangement of plant aerial organs, as a model system. Phyllotaxis arises from reiterative organogenesis driven by lateral inhibitions at the shoot apex. Motivated by recurrent observations of disorders in phyllotaxis patterns, we revisited in depth the classical deterministic view of phyllotaxis. We developed a stochastic model of primordia initiation at the shoot apex, integrating locality and stochasticity in the patterning system. This stochastic model recapitulates phyllotactic patterns, both regular and irregular, and makes quantitative predictions on the nature of disorders arising from noise. We further show that disorders in phyllotaxis instruct us on the parameters governing phyllotaxis dynamics, thus that disorders can reveal biological watermarks of developmental systems.</dcterms:abstract>
	<dcterms:bibliographicCitation>Gut, page gutjnl-2016-311393, article-number 311393</dcterms:bibliographicCitation>
	<dcterms:dateAccepted>2016-07-06</dcterms:dateAccepted>
        <dcterms:issued>2016-05-12</dcterms:issued>
  	<dcterms:description>From eLife via Jisc Publications Router</dcterms:description>
	<dcterms:description>Peer reviewed: True</dcterms:description>
	<dcterms:description>Acknowledgements: ...acknowledgement text...</dcterms:description>
        <dcterms:description>History: received 2016-01-03, rev-recd 2016-04-01, accepted 2016-04-18, ppub 2016-05, epub 2016-05-12</dcterms:description>
	<dcterms:identifier>publisher-id: gutjnl-2016-311393</dcterms:identifier>
        <dcterms:identifier>doi: 10.1136/gutjnl-2016-311393</dcterms:identifier>
	<dcterms:language>en</dcterms:language>
	<dcterms:publisher>eLife Sciences Publications, Ltd</dcterms:publisher>
	<dcterms:source>pissn: 0017-5749</dcterms:source>
	<dcterms:source>eissn: 1468-3288</dcterms:source>
	<dcterms:subject>Pancreas</dcterms:subject>
	<dcterms:subject>PANCREATIC CANCER</dcterms:subject>
	<dcterms:subject>Developmental Biology and Stem Cells</dcterms:subject>
	<dcterms:title>A stochastic multicellular model identifies biological watermarks from disorders in self-organized patterns of phyllotaxis</dcterms:title>
	<dcterms:rights>Licence for VoR version of this article starting on 06-07-2016: http://www.psychoceramics.org/license_v1.html/ </dcterms:rights>
	<dcterms:rights>Licence for VoR version of this article starting on 06-07-2017: http://creativecommons.org/licenses/by/4.0/</dcterms:rights>

	<ali:license_ref start='2017-07-06'>http://creativecommons.org/licenses/by/4.0/</ali:license_ref> 
	
	<pubr:openaccess_uri>http://doi.org/123456789/article.pdf</pubr:openaccess_uri> 
	<pubr:embargo_date>2017-07-06</pubr:embargo_date> 
	<pubr:author id="http://orcid.org/0000-0002-8257-4088" email="teva@yahoo.com">Vernoux, Teva </pubr:author>
	<pubr:contributor id="http://orcid.org/0000-0002-8257-7777" email="bobsmith@yahoo.com">Smith, Bob </pubr:contributor>
	<pubr:sponsorship>Funder: Human Frontier Science Program, Funder ID: http://dx.doi.org/10.13039/100004412, Project: RGP0054-2013  </pubr:sponsorship>
	<pubr:sponsorship>Funder: Human Frontier Science Program, Funder ID: http://dx.doi.org/10.13039/100004412, Project: RGP8888-2013  </pubr:sponsorship>

	<rioxxterms:version>VoR</rioxxterms:version>
	<rioxxterms:version_of_record>http://doi.org/10.7554/elife.14093</rioxxterms:version_of_record>
	<rioxxterms:type>Journal Article/Review</rioxxterms:type>
	<rioxxterms:publication_date>2016-07-06</rioxxterms:publication_date>
	<rioxxterms:project funder_name="Human Frontier Science Program" funder_id="http://dx.doi.org/10.13039/100004412">RGP0054-2013</rioxxterms:project>
	<rioxxterms:project funder_name="Human Frontier Science Program" funder_id="http://dx.doi.org/10.13039/100004412">RGP8888-2013</rioxxterms:project>

</entry>

```

