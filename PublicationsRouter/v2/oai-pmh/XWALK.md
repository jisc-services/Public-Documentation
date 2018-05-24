# PubRouter Notification to OAI_DC Crosswalk

This table defines the mapping from the core PubRouter notification fields to the OAI_DC fields they are exposed under.

Note that given the simplicity of the OAI_DC format, only a limited subset of the PubRouter notification fields are available.

| oai_dc terms | PubRouter Metadata (source) | XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dc:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) | journal.publisher | `<dc:publisher>[journal.publisher] </dc:publisher>` |
| [dc:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.identifier.type <br> journal.identifier.id  | `<dc:source>[journal.identifier.type]: [journal.identifier.id] </dc:source>` |
| [dc:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title | `<dc:title> [article.title] </dc:title>` |
| [dc:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dc:language>[article.language] </dc:language>` |
| [dc:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) |  article.subject | `<dc:subject>[article.subject] </dc:subject>` |
| [dc:identifier](http://dublincore.org/documents/dcmi-terms/#terms-identifier) | article.identifier | `<dc:identifier>[article.identifier.type]: [article.identifier.value]</dc:identifier>`
| [dc:creator](http://dublincore.org/documents/dcmi-terms/#terms-creator) | author.firstname <br> author.surname <br> author.organisation_name <br> author.identifier | `<dc:creator>[author.surname], [author.firstname]; [author.identifier.type]: [author.identifier.id]; [author.organisation_name] </dc:creator>` |
| [dc:contributor](http://dublincore.org/documents/dcmi-terms/#terms-contributor) | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier <br> contributor.type | `<dc:contributor>[contributor.type]: [contributor.surname], [contributor.firstname]; [contributor.identifier.type]: [contributor.identifier.id]; [contributor.organisation_name] </dc:creator>`  |
| [dc:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | embargo.start <br> embargo.end <br> embargo.duration | `<dc:rights>Embargo: starts [embargo.start], ends [embargo.end], duration [embargo.duration] months from publication </dc:rights>` |
| [dc:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.version <br> license_ref.start <br> article.version | `<dc:rights>License for [article.version] version of this article: starting on: [license_ref.start] [license_ref.url] [license_ref.type] [license_ref.title] </dc:rights>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description) | article.version | `<dc:description>Version: [article.version] </dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | provider_agent | `<dc:description>From [provider_agent] via Jisc Publications Router. </dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | publication_status | `<dc:description>Publication status: [publication_status] </dc:description>` | 
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | history_date.date_type <br> history_date.date | `<dc:description>History: [history_date.date_type], [history_date.date] </dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | funding.name <br> funding.grant_number <br> funding.identifier | `<dc:description>Funder: [funding.name], Grant no: [funding.grant_number], [funding.identifier.type]: [funding.identifier.id] </dc:description>` |


Example Metadata Document, values in square brackets refer to the field in PubRouter's metadata, values in brackets indicate the cardinality of the field. 
```xml
<metadata xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
	<oai_dc:dc xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/ http://www.openarchives.org/OAI/2.0/OAI-PMH.xsd">
		<dc:language>[metadata.article.language](0..n)</dc:language>
		<dc:title>[metadata.article.title](1)</dc:title>
		<dc:creator>[metadata.authors](1..n)</dc:creator>
		<dc:subject>[metadata.article.subject](0..n)</dc:subject>
		<dc:description>[metadata.article.version](0..1)</dc:description>
		<dc:description>[metadata.publication_status](0..1)</dc:description>
		<dc:description>[metadata.history_date](0..n)</dc:description>
		<dc:description>[metadata.funding](0..n)</dc:description>
		<dc:description>[provider.agent](1)</dc:description>
		<dc:publisher>[metadata.journal.publisher](0..1)</dc:publisher>
		<dc:contributor>[metadata.contributor](0..n)</dc:contributor>
		<dc:identifier>[metadata.article.identifier](0..n)</dc:identifier>
		<dc:source>[metadata.journal.identifier](0..n)</dc:source>
		<dc:source>[metadata.journal.title](0..1)</dc:source>
		<dc:type>[metadata.article.type](0..1)</dc:type>
		<dc:date>[metadata.publication_date](1)</dc:date>
		<dc:rights>[metadata.license_ref](0..n)</dc:rights>
		<dc:rights>[metadata.embargo](0..1)</dc:rights>
	</oai_dc:dc>
</metadata>
```

