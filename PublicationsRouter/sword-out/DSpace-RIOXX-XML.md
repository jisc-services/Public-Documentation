# PubRouter DSpace RIOXX XML Description

This document describes the RIOXX XML output by PubRouter for ingestion into DSpace repositories via the SWORDv2 interface. 
 
For background information related to the schema see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX](http://www.rioxx.net/)
* [DSpace schema](https://wiki.duraspace.org/display/DSDOC4x/Metadata+and+Bitstream+Format+Registries).

The following table lists:
* Column 1 - the XML elements output by PubRouter 
* Column 2 - PubRouter internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note. 

**NOTE: RIOXX XML format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Any other text is output as it appears in the format.


| DC terms | PubRouter Metadata (source) | RIOXX XML Format |
|:-----------------------------|:-------------------------|:------------------------------------------------------------|
| [dcterms:bibliographicCitation](http://dublincore.org/documents/dcmi-terms/#terms-bibliographicCitation) | journal.title <br> journal.abbrevTitle <br> journal.volume <br> journal.issue <br> article.start_page  <br> article.end_page  <br> article.page_range <br>  | `<dcterms:bibliographicCitation>[journal.title], volume [journal.volume], issue [journal.issue], page [article.start_page]-[article.end_page] or [article.page_range] </dcterms:bibliographicCitation>`|

| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher) | journal.publisher | `<dcterms:publisher>[journal.publisher] </dcterms:publisher>` |

| [dcterms:source](http://dublincore.org/documents/dcmi-terms/#terms-source) | journal.identifier.type <br> journal.identifier.id  | `<dcterms:source>[journal.identifier.type]: [journal.identifier.id] </dcterms:source>` |

| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title | `<dcterms:title> [article.title] </dcterms:title>` |

| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | `<dcterms:language>[article.language] </dcterms:language>` |

| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract) | article.abstract | `<dcterms:abstract>[article.abstract] </dcterms:abstract>` |

| [dcterms:identifier](http://dublincore.org/documents/dcmi-terms/#terms-identifier) | article.identifier.type <br> article.identifier.id  | `<dcterms:identifier>[article.identifier.type]: [article.identifier.id] </dcterms:identifier>` |

| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) |  article.subject | `<dcterms:subject>[article.subject] </dcterms:subject>` |

| [dcterms:issued](http://dublincore.org/documents/dcmi-terms/#terms-issued) | publication_date | `<dcterms:issued>[publication_date] </dcterms:issued>` |

|[dcterms:description](http://dublincore.org/documents/dcmi-terms/#terms-description)  | history_date.date_type <br> history_date.date | `<dcterms:description>History: [history_date.date_type], [history_date.date] </dcterms:description>` |

| [dcterms:rights](http://dublincore.org/documents/dcmi-terms/#terms-rights) | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.version <br> license_ref.start <br> article.version | `<dcterms:rights>License for [article.version] version of this article: starting on: [license_ref.start] [license_ref.url] [license_ref.type] [license_ref.title] </dcterms:rights>` |

-----------RIOXX------------

| [rioxxterms:author](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#author) | author.firstname <br> author.surname <br> author.organisation_name <br> author.identifier | `<rioxxterms:author id="[author.identifier.id (orcid)]">[author.surname], [author.firstname]; [author.organisation_name] </rioxxterms:author>` |

| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) | accepted_date | `<dcterms:dateAccepted>[accepted_date] </dcterms:dateAccepted>` |

| [rioxxterms:contributor](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#contributor) | contributor.firstname <br> contributor.surname <br> contributor.organisation_name <br> contributor.identifier <br> contributor.type | `<rioxxterms:contributor id="[contributor.identifier.id (orcid)]">[contributor.type]: [contributor.surname], [contributor.firstname]; [contributor.organisation_name] </dcterms:creator>`  |

| [rioxxterms:version](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version) | article.version | `<rioxxterms:version>[article.version] </rioxxterms:version>` |

| [rioxxterms:version_of_record](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#version-of-record) | article.identifier.id (DOI) | `<rioxxterms:version_of_record>Version: [article.identifier.id] </rioxxterms:version_of_record>` |

| [rioxxterms:type](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#type)  | article.type | `<rioxxterms:type>[article.type]</rioxxterms:type>` |

| [rioxxterms:publication_date](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#publication_date)  | publication_date | `<rioxxterms:publication_date> [publication_date] </rioxxterms:publication_date>` | 

| [rioxxterms:project](http://www.rioxx.net/schema/v2.0/rioxxterms/rioxxterms_.html#project)  | funding.name <br> funding.identifier <br> funding.grant_number  | `<rioxxterms:project funder_name="[funding.name]" funder_id="[funding.identifier.id (DOI)]">[funding.grant_number]</rioxxterms:project>` |

| **[dc:description_sponsorship]**(http://)  | funding.name <br> funding.grant_number <br> funding.identifier | `<dc:description_sponsorship>Funder: [funding.name], Grant no: [funding.grant_number], [funding.identifier.type]: [funding.identifier.id] </dc:description_sponsorship>` |

| **[dcterms:rights_uri]**(http://dublincore.org/documents/dcmi-terms/#terms-rights) | license_ref.url | `<dcterms:rights_uri> [license_ref.url] </dcterms:rights_uri>` |


| **[dcterms:embargodate]**(http://) | embargo.end | `<dcterms:embargodate>Embargo end date: [embargo.end] </dcterms:embargodate>` |
