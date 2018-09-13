# PubRouter Notification to RIOXX Crosswalk

The table below lists the RIOXX terms that are provided by PubRouter's implementation of OAI-PMH, and shows how PubRouter's metadata elements are mapped to these.

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated PubRouter JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Any other text is output as it appears in the format.

| rioxx terms | PubRouter Metadata (source) | XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dc:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) | journal.publisher | `<dc:publisher>[journal.publisher]</dc:publisher>` |
| [dc:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.identifier.type <br> journal.identifier.id  | `<dc:source>[journal.identifier.type]: [journal.identifier.id]</dc:source>` |
| [dc:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.title | `<dc:source>[journal.title]</dc:source>` |
| [dc:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title | `<dc:title>[article.title]</dc:title>` |
| [dc:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dc:language>[article.language]</dc:language>` |
| [dc:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) |  article.subject | `<dc:subject>[article.subject]</dc:subject>` |
| [dc:identifier](http://dublincore.org/documents/dcmi-terms/#terms-identifier) | article.identifier | `<dc:identifier>[article.identifier.type]: [article.identifier.value]</dc:identifier>` |
| [rioxxterms:version](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version) | article.version | `<rioxxterms:version>[article.version] </rioxxterms:version>` |
| [rioxxterms:version_of_record](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version-of-record) | article.identifier.id (DOI) | `<rioxxterms:version_of_record>Version: [article.identifier.id] </rioxxterms:version_of_record>` |
| [rioxxterms:author](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#author) | author.firstname <br> author.surname <br> author.identifier | `<rioxxterms:author id=[author.identifier]>[author.surname], [author.firstname]</rioxxterms:author>` |
| [rioxxterms:contributor](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#contributor) | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier | `<rioxxterms:contributor id=[>[contributor.identifier]: [contributor.surname], [contributor.firstname] OR [contributor.organisation_name]</rioxxterms:contributor>`  |
| [rioxxterms:project](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#project) | funding.name <br> funding.identifier <br> funding.grant_number | `<rioxxterms:project funder_name="[funding.name]" funder_id="[funding.identifier.id (DOI)]">[funding.grant_number]</rioxxterms:project>`|
| [rioxxterms:publication_date](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#publication-date) | publication_date | `<rioxxterms:publication_date> [publication_date] </rioxxterms:publication_date>` | 
| [ali:license_ref](http://www.rioxx.net/schema/v2.0/rioxx/ali_1_0.html#license_ref)| license_ref.url <br> license_ref.url.start  | `<ali:license_ref start=”[license_ref.url.start]”> [license_ref.url] </ali:license_ref>` |
| [dc:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | embargo.start <br> embargo.end <br> embargo.duration | `<dc:rights>Embargo: starts [embargo.start], ends [embargo.end], duration [embargo.duration] months from publication </dc:rights>` |
| [dc:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.version <br> license_ref.start <br> article.version | `<dc:rights>License for [article.version] version of this article: starting on: [license_ref.start] [license_ref.url] [license_ref.type] [license_ref.title]</dc:rights>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description) | article.version | `<dc:description>Version: [article.version]</dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | provider_agent | `<dc:description>From [provider_agent] via Jisc Publications Router.</dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | publication_status | `<dc:description>Publication status: [publication_status]</dc:description>` | 
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | history_date.date_type <br> history_date.date | `<dc:description>History: [history_date.date_type], [history_date.date] </dc:description>` |
| [dc:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | funding.name <br> funding.grant_number <br> funding.identifier | `<dc:description>Funder: [funding.name], Grant no: [funding.grant_number], [funding.identifier.type]: [funding.identifier.id]</dc:description>` |
| [dc:type](http://dublincore.org/documents/dcmi-terms/#terms-type) | article.type | `<dc:type>[article.type]</dc:type>` |
| [dc:date](http://dublincore.org/documents/dcmi-terms/#terms-type) | publication_date | `<dc:date>[publication_date]</dc:date>` |

