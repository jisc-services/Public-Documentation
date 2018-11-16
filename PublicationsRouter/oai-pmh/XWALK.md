# PubRouter Notification to OAI_DC Crosswalk

The table below lists the OAI_DC terms that are provided by PubRouter's implementation of OAI-PMH, and shows how PubRouter's metadata elements are mapped to these.

Note that given the simplicity of the OAI_DC format, only a subset of the PubRouter notification fields are available.

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated PubRouter JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Any other text is output as it appears in the format.

| oai_dc terms | PubRouter Metadata (source) | XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dc:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) | journal.publisher | `<dc:publisher>[journal.publisher]</dc:publisher>` |
| [dc:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.identifier.type <br> journal.identifier.id  | `<dc:source>[journal.identifier.type]: [journal.identifier.id]</dc:source>` |
| [dc:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.title | `<dc:source>[journal.title]</dc:source>` |
| [dc:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title | `<dc:title>[article.title]</dc:title>` |
| [dc:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dc:language>[article.language]</dc:language>` |
| [dc:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) |  article.subject | `<dc:subject>[article.subject]</dc:subject>` |
| [dc:identifier](http://dublincore.org/documents/dcmi-terms/#terms-identifier) | article.identifier | `<dc:identifier>[article.identifier.type]: [article.identifier.value]</dc:identifier>`
| [dc:creator](http://dublincore.org/documents/dcmi-terms/#terms-creator) | author.firstname <br> author.surname <br> author.organisation_name <br> author.identifier | `<dc:creator>[author.surname], [author.firstname]; [author.identifier.type]: [author.identifier.id]; [author.organisation_name]</dc:creator>` |
| [dc:contributor](http://dublincore.org/documents/dcmi-terms/#terms-contributor) | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier <br> contributor.type | `<dc:contributor>[contributor.type]: [contributor.surname], [contributor.firstname]; [contributor.identifier.type]: [contributor.identifier.id]; [contributor.organisation_name]</dc:contributor>`  |
| [dc:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | embargo.start <br> embargo.end <br> embargo.duration | `<dc:rights>Embargo: starts [embargo.start], ends [embargo.end], duration [embargo.duration] months from publication </dc:rights>` |
| [dc:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.version <br> license_ref.start <br> article.version | `<dc:rights>License for [article.version] version of this article: starting on: [license_ref.start] [license_ref.url] [license_ref.type] [license_ref.title]</dc:rights>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description) | article.version | `<dc:description>Version: [article.version]</dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | provider_agent | `<dc:description>From [provider_agent] via Jisc Publications Router.</dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | publication_status | `<dc:description>Publication status: [publication_status]</dc:description>` | 
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | history_date.date_type <br> history_date.date | `<dc:description>History: [history_date.date_type], [history_date.date] </dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | funding.name <br> funding.grant_number <br> funding.identifier | `<dc:description>Funder: [funding.name], Grant no: [funding.grant_number], [funding.identifier.type]: [funding.identifier.id]</dc:description>` |
| [dc:type](http://dublincore.org/documents/dcmi-terms/#terms-type) | article.type | `<dc:type>[article.type]</dc:type>` |
| [dc:date](http://dublincore.org/documents/dcmi-terms/#terms-type) | publication_date | `<dc:date>[publication_date]</dc:date>` |



Example xml returned
```xml
<metadata>
	<oai_dc:dc xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/ http://www.openarchives.org/OAI/2.0/OAI-PMH.xsd" xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
	<generator uri="http://pubrouter.jisc.ac.uk/python-sword2" version="0.2"/>
		<dc:language>en</dc:language>
		<dc:source>pissn: 0017-5749</dc:source>
		<dc:source>eissn: 1468-3288</dc:source>
		<dc:source>Journal of Interesting Things</dc:source>
		<dc:publisher>BMJ Publishing Group</dc:publisher>
		<dc:description>From Publisher via Jisc Publications Router.</dc:description>
		<dc:creator>Mahajan,Ujjwal M</dc:creator>
		<dc:creator>Teller,Steffen</dc:creator>
		<dc:contributor>Sendler,Matthias</dc:contributor>
		<dc:title>Tumour-specific delivery of siRNA-coupled superparamagnetic iron oxide nanoparticles, targeted against PLK1, stops progression of pancreatic cancer</dc:title>
		<dc:identifier>publisher-id: gutjnl-2016-311393</dc:identifier>
		<dc:identifier>doi: 10.1136/gutjnl-2016-311393</dc:identifier>
		<dc:subject>Pancreas</dc:subject>
		<dc:subject>PANCREATIC CANCER</dc:subject>
		<dc:description>Publication status: Published</dc:description>
		<dc:description>History: received 2016-01-03, rev-recd 2016-04-01, accepted 2016-04-18, ppub 2016-05, epub 2016-05-12</dc:description>
		<dc:rights>Licence for this article: https://testing.org/licenses/by/4.0/ uat lic 3 License uat testing</dc:rights>
		<dc:rights>Embargo: starts 2016-05-12, ends 2016-12-12, duration 7 months from publication.</dc:rights>
		<dc:type>article</dc:type>
		<dc:date>2018-01-01</dc:date>
	</oai_dc:dc>
</metadata>
```

