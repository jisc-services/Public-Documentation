# PubRouter Notification to OAI-RIOXX Crosswalk

The table below lists the RIOXX terms that are provided by PubRouter's implementation of OAI-PMH, and shows how PubRouter's metadata elements are mapped to these.
 
For background information related to the schema see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX](http://www.rioxx.net/)
* [OAI_PMH](https://www.openarchives.org/pmh/)

## Namespaces ##

The OAI-PMH-RIOXX XML uses 4 namespaces:

| Namespace | URI | Description |
|:----------|:----|:------------|
| ali        | http://www.niso.org/schemas/ali/1.0/ | National Information Standards Organisation's Access and License Information schema  |
| dc    | http://purl.org/dc/elements/1.1/ | DublinCore |
| dcterms    | http://purl.org/dc/terms/ | DublinCore (extended) terms schema |
| rioxxterms | http://www.rioxx.net/schema/v2.0/rioxx/ | RIOXX schema |

## XML Elements ##

The table below lists:
* Column 1 - the XML elements output by PubRouter and cardinality:
	* {0..1} – zero or one
	* {0..n} – zero or more
	* {1..1} – exactly one (i.e. a mandatory element)
	* {1..n} – one or more (i.e. a mandatory element)
* Column 2 - PubRouter internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note. 

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated PubRouter JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Any other text is output as it appears in the format.

| rioxx terms | PubRouter Metadata (source) | XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [ali:license_ref](http://www.rioxx.net/schema/v2.0/rioxx/ali_1_0.html#license_ref) <br>{0..n} | license_ref.url <br> license_ref.url.start  | `<ali:license_ref start=”[license_ref.url.start]”> [license_ref.url] </ali:license_ref>` |
| [ali:free_to_read](http://www.rioxx.net/schema/v2.0/rioxx/ali_1_0.html#free_to_read) <br>{0..1} | free2read.start (optional) <br> free2read.end (optional) | `<ali:free_to_read start_date="[free2read.start]" end_date="[free2read.end]"></ali:free_to_read> |
| [dc:description](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__description)  <br>{0..n} | provider_agent | `<dc:description>From [provider_agent] via Jisc Publications Router.</dc:description>` |
| [dc:description](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__description)  <br>{0..n}  | publication_status | `<dc:description>Publication status: [publication_status]</dc:description>` | 
| [dc:description](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__description)  <br>{0..n}  | history_date.date_type <br> history_date.date | `<dc:description>History: [history_date.date_type], [history_date.date] </dc:description>` |
| [dc:identifier](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__identifier)  <br>{0..1} | links.url (pdf) <br> This field MUST contain an HTTP URI which is a persistent identifier for the resource. | `<dc:identifier>[links.url]</dc:identifier>` |
| [dc:language](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__language) <br>{0..n} | article.language | `<dc:language>[article.language]</dc:language>` |
| [dc:publisher](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__publisher)  <br>{0..1} | journal.publisher | `<dc:publisher>[journal.publisher]</dc:publisher>` |
| [dc:source](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__source)  <br>{0..n} | journal.identifier.type <br> journal.identifier.id  | `<dc:source>[journal.identifier.type]: [journal.identifier.id]</dc:source>` |
| [dc:source](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__source)  <br>{0..1} | journal.title | `<dc:source>[journal.title]</dc:source>` |
| [dc:subject](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__subject)  <br>{0..n} |  article.subject | `<dc:subject>[article.subject]</dc:subject>` |
| [dc:title](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#http___purl.org_dc_terms__title)  <br>{1..1} | article.title | `<dc:title>[article.title]</dc:title>` |
| [dcterms:dateAccepted](http://www.rioxx.net/schema/v2.0/rioxx/terms_.html#dateAccepted)  <br>{0..1} | accepted_date | `<dcterms:dateAccepted>[accepted_date]</dc:date>` |
| [rioxxterms:author](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#author)  <br>{1..n} | author.firstname <br> author.surname <br> author.identifier | `<rioxxterms:author id=[author.identifier]>[author.surname], [author.firstname]</rioxxterms:author>` |
| [rioxxterms:contributor](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#contributor)  <br>{0..n} | contributor.type <br> contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier | `<rioxxterms:contributor id=[contributor.identifier]>[contributor.type]: [contributor.surname], [contributor.firstname] OR [contributor.organisation_name]</rioxxterms:contributor>`  |
| [rioxxterms:project](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#project)  <br>{0..n} | funding.name <br> funding.identifier <br> funding.grant_number | `<rioxxterms:project funder_name="[funding.name]" funder_id="[funding.identifier.id (DOI)]">[funding.grant_number]</rioxxterms:project>`|
| [rioxxterms:publication_date](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#publication-date)  <br>{0..1} | publication_date <br> (yyyy-mm-dd) | `<rioxxterms:publication_date> [publication_date] </rioxxterms:publication_date>` | 
| [rioxxterms:type](http://www.rioxx.net/schema/v2.0/rioxx/rioxxterms_.html#type)  <br>{1..1} | article.type | `<rioxxterms:type>[article.type]</rioxxterms:type>` |
| [rioxxterms:version](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version)  <br>{0..1} | article.version | `<rioxxterms:version>[article.version] </rioxxterms:version>` |
[rioxxterms:version_of_record](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version-of-record)  <br>{1..1} | article.identifier.id (DOI) | `<rioxxterms:version_of_record>Version: [article.identifier.id] </rioxxterms:version_of_record>` |


```xml
<metadata xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:dcterms="http://purl.org/dc/terms/" xmlns:ali="http://ali.niso.org/2014/ali/1.0" xmlns:rioxx="http://www.rioxx.net/schema/v2.0/rioxx/">
  <rioxx:rioxx>
    <ali:free_to_read />
    <ali:license_ref ali:start_date="2016-07-06">http://creativecommons.org/licenses/by/4.0/</ali:license_ref>
    <dc:description>From FTP publisher via Jisc Publications Router.</dc:description>
    <dc:identifier>http://europepmc.org/backend/ptpmcrender.fcgi?accid=PMC6042627&blobtype=pdf</dc:identifier>
    <dc:language>en</dc:language>
    <dc:publisher>eLife Sciences Publications, Ltd</dc:publisher>
    <dc:subject>Pancreas</dc:subject>
    <dc:subject>PANCREATIC CANCER</dc:subject>
    <dc:subject>Developmental Biology and Stem Cells</dc:subject>
    <dc:title>test deposit 2</dc:title>
    <dcterms:dateAccepted>2016-07-06</dcterms:dateAccepted>
    <rioxxterms:author rioxxterms:id="http://orcid.org/0000-0000-0000-0001">Sendler, Matthias</rioxxterms:author>
    <rioxxterms:author rioxxterms:id="http://orcid.org/0000-0000-0000-0002">Palankar, Raghavendra</rioxxterms:author>
    <rioxxterms:author>Kühn, Jens-Peter</rioxxterms:author>
    <rioxxterms:author rioxxterms:id="http://orcid.org/0000-0000-0000-0004">Evert, Matthias</rioxxterms:author>
    <rioxxterms:contributor>Editor: Benet, Jonh</rioxxterms:contributor>
    <rioxxterms:contributor>Translator: Mayerle,Julia</rioxxterms:contributor>
    <rioxxterms:publication_date>2016-07-06</rioxxterms:publication_date>
    <rioxxterms:type>Journal Article/Review</rioxxterms:type>
    <rioxxterms:version>VoR</rioxxterms:version>
    <rioxxterms:version_of_record>http://doi.org/10.7554/elife.14093</rioxxterms:version_of_record>
    <rioxxterms:project rioxxterms:funder_name="National Science Foundation" rioxxterms:funder_id="http://dx.doi.org/10.13039/100000001">RGP0000-2010</rioxxterms:project>
    <rioxxterms:project rioxxterms:funder_name="National Institutes of Health">RGP5555-2015</rioxxterms:project>
  </rioxx:rioxx>
</metadata>
```

