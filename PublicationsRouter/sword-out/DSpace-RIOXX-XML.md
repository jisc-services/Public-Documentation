# PubRouter DSpace RIOXX XML Description

This page describes the Dspace-RIOXX XML output by PubRouter for ingest via the SWORDv2 interface into DSpace repositories with the [RIOXX patch](https://github.com/atmire/RIOXX) installed. (Note that PubRouter can output a completely different XML document for "vanilla" DSpace repositories that do not use the RIOXX patch.)
 
For background information related to the schema see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX](http://www.rioxx.net/)
* [DSpace schema](https://wiki.duraspace.org/display/DSDOC4x/Metadata+and+Bitstream+Format+Registries).

The following table lists:
* Column 1 - the XML elements output by PubRouter and cardinality:
	* {0..1} – zero or one
	* {0..n} – zero or more
	* {1..1} – exactly one (i.e. a mandatory element)
	* {1..n} – one or more (i.e. a mandatory element)
* Column 2 - PubRouter internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note. 

**NOTE: RIOXX XML format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated PubRouter internal JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Any other text is output as it appears in the format.

## Namespaces ##

The DSpace RIOXX XML uses 4 namespaces:

| Namespace | URI | Description |
|:----------|:----|:------------|
| ali        | http://www.niso.org/schemas/ali/1.0/ | National Information Standards Organisation's Access and License Information schema  |
| dcterms    | http://purl.org/dc/terms/ | DublinCore (extended) terms schema |
| rioxxterms | http://www.rioxx.net/schema/v2.0/rioxx/ | RIOXX schema |
| pubr       | http://pubrouter.jisc.ac.uk/dspacerioxx/ | Jisc PubRouter-DSpace schema |

## XML Elements ##

| XML element and {Cardinality} | PubRouter Metadata (source) | RIOXX XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dcterms:bibliographicCitation](http://dublincore.org/documents/dcmi-terms/#terms-bibliographicCitation)<br>{0..1} | journal.title <br> journal.abbrevTitle <br> journal.volume <br> journal.issue <br> article.start_page  <br> article.end_page  <br> article.page_range <br>  | `<dcterms:bibliographicCitation>[journal.title], volume [journal.volume], issue [journal.issue], page [article.start_page]-[article.end_page] or [article.page_range] </dcterms:bibliographicCitation>`|
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) <br>{0..1}| journal.publisher | `<dcterms:publisher>[journal.publisher] </dcterms:publisher>` |
| [dcterms:source](http://dublincore.org/documents/dcmi-terms/#terms-source)<br>{0..n} | journal.identifier.type <br> journal.identifier.id  | `<dcterms:source>[journal.identifier.type]: [journal.identifier.id] </dcterms:source>` |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title)<br>{1..1} | article.title | `<dcterms:title> [article.title] </dcterms:title>` |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language)<br>{0..1} | article.language | `<dcterms:language>[article.language] </dcterms:language>` |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract)<br>{0..1} | article.abstract | `<dcterms:abstract>[article.abstract] </dcterms:abstract>` |
| [dcterms:identifier](http://dublincore.org/documents/dcmi-terms/#terms-identifier)<br>{0..n} | article.identifier.type <br> article.identifier.id  | `<dcterms:identifier>[article.identifier.type]: [article.identifier.id] </dcterms:identifier>` |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject)<br>{0..n} |  article.subject | `<dcterms:subject>[article.subject] </dcterms:subject>` |
|[dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description) <br>{1..n} | history_date.date_type <br> history_date.date | `<dcterms:description>History: [history_date.date_type], [history_date.date] </dcterms:description>` |
| [dcterms:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights)<br>{0..n} | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.version <br> license_ref.start <br> article.version | `<dcterms:rights>License for [article.version] version of this article: starting on: [license_ref.start] [license_ref.url] [license_ref.type] [license_ref.title] </dcterms:rights>` |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted)<br>{0..1} | accepted_date | `<dcterms:dateAccepted>[accepted_date] </dcterms:dateAccepted>` |
| [rioxxterms:version](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version)<br>{0..1} | article.version | `<rioxxterms:version>[article.version] </rioxxterms:version>` |
| [rioxxterms:version_of_record](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version-of-record)<br>{1..1} | article.identifier.id (DOI) | `<rioxxterms:version_of_record>Version: [article.identifier.id] </rioxxterms:version_of_record>` |
| [rioxxterms:type](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#type)<br>{1..1}  | article.type | `<rioxxterms:type>[article.type]</rioxxterms:type>` |
| [rioxxterms:publication_date](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#publication_date) <br>{0..1} | publication_date | `<rioxxterms:publication_date> [publication_date] </rioxxterms:publication_date>` | 
| [rioxxterms:project](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#project) <br>{0..n} | funding.name <br> funding.identifier <br> funding.grant_number  | `<rioxxterms:project funder_name="[funding.name]" funder_id="[funding.identifier.id (DOI)]">[funding.grant_number]</rioxxterms:project>` |
| [ali:license_ref](http://www.rioxx.net/schema/v2.0/rioxx/ali_1_0.html#license_ref)<br>{0..1}<br><sub>Note that while RIOXX allows for multiple ali:license_ref elements, the DSpace RIOXX patch can only handle one; hence we recommend that the earliest open access license is sent in this element.</sub>| license_ref.url <br> license_ref.url.start  | `<ali:license_ref start=”[license_ref.url.start]”> [license_ref.url] </ali:license_ref>` |
| [pubr:openaccess_uri]()<br>{0..1} | TBC | `<pubr:openaccess_uri>[TBC]</pubr:openaccess_uri>` |
| [pubr:author]()<br>{0..n} | author.firstname <br> author.surname <br> author.organisation_name <br> author.identifier.orcid <br> author.identifier.email | `<pubr:author id="[author.identifier.id (orcid)]" email="[author.identifier.id (email)]">[author.surname], [author.firstname]; [author.organisation_name] </pubr:author>` |
| [pubr:contributor]()<br>{0..n} | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier.orcid <br> contributor.identifier.email <br> contributor.type | `<pubr:contributor id="[contributor.identifier.id (orcid)] email="[contributor.identifier.id (email)]">[contributor.type]: [contributor.surname], [contributor.firstname]; [contributor.organisation_name] </pubr:contributor>`  |
| [pubr:sponsorship]()<br>{0..n} | funding.name <br> funding.grant_number <br> funding.identifier | `<pubr:sponsorship>Funder: [funding.name], Grant no: [funding.grant_number], Funder ID: [funding.identifier.id] </pubr:sponsorship>` |
| [pubr:embargo_date]()<br>{0..1} | embargo.end | `<pubr:embargo_date>[embargo.end]</pubr:embargo_date>` |



## Example PubRouter-DSpace-RIOXX XML ##

This is an example Dspace-RIOXX Entry XML document output by PubRouter that contains the metadata elements listed above.

```xml

<?xml version="1.0"?>
<entry xmlns="http://www.w3.org/2005/Atom" 
	xmlns:ali="http://www.niso.org/schemas/ali/1.0/" 
	xmlns:pubr="http://pubrouter.jisc.ac.uk/dspacerioxx/" 
	xmlns:dcterms="http://purl.org/dc/terms/" 
	xmlns:rioxxterms="http://www.rioxx.net/schema/v2.0/rioxx/">
	
	<dcterms:abstract>Exploration of developmental mechanisms classically relies on analysis of pattern regularities. Whether disorders induced by biological noise may carry information on building principles of developmental systems is an important debated question. Here, we addressed theoretically this question using phyllotaxis, the geometric arrangement of plant aerial organs, as a model system. Phyllotaxis arises from reiterative organogenesis driven by lateral inhibitions at the shoot apex. Motivated by recurrent observations of disorders in phyllotaxis patterns, we revisited in depth the classical deterministic view of phyllotaxis. We developed a stochastic model of primordia initiation at the shoot apex, integrating locality and stochasticity in the patterning system. This stochastic model recapitulates phyllotactic patterns, both regular and irregular, and makes quantitative predictions on the nature of disorders arising from noise. We further show that disorders in phyllotaxis instruct us on the parameters governing phyllotaxis dynamics, thus that disorders can reveal biological watermarks of developmental systems.</dcterms:abstract>
	<dcterms:bibliographicCitation>Gut, page gutjnl-2016-311393</dcterms:bibliographicCitation>
	<dcterms:dateAccepted>2016-07-06</dcterms:dateAccepted>
	<dcterms:description>** From FTP publisher via Jisc Publications Router.</dcterms:description>
	<dcterms:identifier>publisher-id: gutjnl-2016-311393</dcterms:identifier>
	<dcterms:language>en</dcterms:language>
	<dcterms:publisher>eLife Sciences Publications, Ltd</dcterms:publisher>
	<dcterms:source>pissn: 0017-5749</dcterms:source>
	<dcterms:source>eissn: 1468-3288</dcterms:source>
	<dcterms:subject>Pancreas</dcterms:subject>
	<dcterms:subject>PANCREATIC CANCER</dcterms:subject>
	<dcterms:subject>Developmental Biology and Stem Cells</dcterms:subject>
	<dcterms:title>A stochastic multicellular model identifies biological watermarks from disorders in self-organized patterns of phyllotaxis</dcterms:title>
	<dcterms:rights>Licence for this article on 2016-07-06: http://www.psychoceramics.org/license_v1.html/ </dcterms:rights>
	<dcterms:rights>Licence for this article starting on 2017-07-06: http://creativecommons.org/licenses/by/4.0/</dcterms:rights>
	<dcterms:rights>Embargo: starts 2016-07-06, ends 2017-07-06, duration 12 months from publication.</dcterms:rights>

	<ali:license_ref start='2017-07-06'>http://creativecommons.org/licenses/by/4.0/</ali:license_ref> 
	
	<pubr:openaccess_uri>!!!! TO BE CONFIRMED !!!!</pubr:openaccess_uri> 
	<pubr:embargo_date>2017-07-06</pubr:embargo_date> 
	<pubr:author id="http://orcid.org/0000-0002-8257-4088" email="teva@yahoo.com">Vernoux, Teva </pubr:author>
	<pubr:contributor id="http://orcid.org/0000-0002-8257-7777" email="johnsmith@yahoo.com">Smith, Bob </pubr:contributor>
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

