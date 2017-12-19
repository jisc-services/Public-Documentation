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
