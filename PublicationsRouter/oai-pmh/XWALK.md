# JPER Notification to OAI_DC Crosswalk

This table defines the mapping from the core JPER notification fields to the OAI_DC fields they are exposed under.

Note that given the simplicity of the OAI_DC format, only a limited subset of the JPER notification fields are available.

| JPER Field | OAI_DC field |
|------------|--------------|
| metadata.title | oai_dc:title |
| metadata.publisher | oai_dc:publisher |
| metadata.source.name | oai_dc:source |
| metadata.source.identifier | oai_dc:source |
| metadata.identifier | oai_dc:identifier |
| metadata.type | oai_dc:type |
| metadata.author | oai_dc:creator |
| metadata.author.affiliation | oai_dc:contributor |
| metadata.language | oai_dc:langauage |
| metadata.publication_date | oai_dc:date |
| metadata.license_ref.title | oai_dc:rights |
| metadata.subject | oai_dc:subject |