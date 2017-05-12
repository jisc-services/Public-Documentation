# PubRouter Notification to OAI_DC Crosswalk

This table defines the mapping from the core PubRouter notification fields to the OAI_DC fields they are exposed under.

Note that given the simplicity of the OAI_DC format, only a limited subset of the PubRouter notification fields are available.

| PubRouter Field | OAI_DC field |
|------------|--------------|
| metadata.article.title | oai_dc:title |
| metadata.journal.publisher | oai_dc:publisher |
| metadata.journal.title | oai_dc:source |
| metadata.journal.identifier | oai_dc:source |
| metadata.article.identifier | oai_dc:identifier |
| metadata.article.type | oai_dc:type |
| metadata.author | oai_dc:creator |
| metadata.author.affiliation | oai_dc:contributor |
| metadata.article.language | oai_dc:langauage |
| metadata.publication_date | oai_dc:date |
| metadata.license_ref.title | oai_dc:rights |
| metadata.article.subject | oai_dc:subject |

(May 2015: Note that this output will be reviewed shortly with a view to expanding the information available via OAI-PMH so it is comparable to that which is available via the [API](https://github.com/sherpaservices/Public-Documentation/blob/master/PublicationsRouter/v2/api/OutgoingNotification.md).)
