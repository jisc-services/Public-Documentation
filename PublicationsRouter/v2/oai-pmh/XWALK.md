# PubRouter Notification to OAI_DC Crosswalk

This table defines the mapping from the core PubRouter notification fields to the OAI_DC fields they are exposed under.

Note that given the simplicity of the OAI_DC format, only a limited subset of the PubRouter notification fields are available.

| PubRouter Field | OAI_DC field |
|------------|--------------|
| metadata.article.title | oai_dc:title |
| metadata.article.language | oai_dc:language |
| metadata.author | oai_dc:creator |
| metadata.article.subject | oai_dc:subject |
| metadata.article.version | oai_dc:description |
| metadata.publication_status | oai_dc:description |
| metadata.history_date | oai_dc:description |
| metadata.funding | oai_dc:description |
| provider.agent | oai_dc:description |
| metadata.journal.publisher | oai_dc:publisher |
| metadata.contributors | oai_dc:contributor |
| metadata.article.identifier | oai_dc:identifier |
| metadata.journal.identifier | oai_dc:source |
| metadata.journal.title | oai_dc:source |
| metadata.article.type | oai_dc:type |
| metadata.publication_date | oai_dc:date |
| metadata.license_ref | oai_dc:rights |
| metadata.embargo | oai_dc:rights |

Example Document
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

