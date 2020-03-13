# Publications Router XML Schema Definitiions

Two XML Schemas have been defined for the purpose of sending article metadata to Eprints and Dspace repositories that are "RIOXX enabled" through the installation of their respective RIOXX plugin / patch.  This enables maximum information harvested by Publications Router to be passed on to these repositories.

The two Publications Router schemas build upon elements from industry standard schemas:

| Description | Namespace | URI |
|:------------|:----------|:----|
| National Information Standards Organisation's Access and License Information schema  | ali | http://www.niso.org/schemas/ali/1.0/ |
| DublinCore (extended) terms schema | dcterms | http://purl.org/dc/terms/ |
| RIOXX schema | rioxxterms | http://www.rioxx.net/schema/v2.0/rioxx/ |

However, existing elements within these schemas are not sufficient to easily transmit more complex metadata that Publications Router
provides, such as:
* Author information, including ORCIDs and Emails
* Grant funding
* License information
* etc.

Accordingly we defined Publications Router XML elements to support easy transfer of additional information to Eprints and DSpace.  The differences
between the two repositories necessitated development of different schemas for this purpose:

| Schema | Namespace | URI | Description |
|:--|:----------|:----|:------------|
| [eprints-rioxx.xsd](./eprints-rioxx.xsd) | pr    | http://pubrouter.jisc.ac.uk/rioxxplus/  | Jisc Publications Router-Eprints schema |
| [dspace-rioxx.xsd](./dspace-rioxx.xsd) | pubr   | http://pubrouter.jisc.ac.uk/dspacerioxx/ | Jisc Publications Router-DSpace schema |


**NOTE** that these schemas are still **work-in-progress** as they currently define only the new elements, and not the elements 
included from the industry standard schemas noted above.
