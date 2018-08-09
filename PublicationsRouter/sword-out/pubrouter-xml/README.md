# PubRouter XML Schema Definitiions

## Overview

Two XML Schemas have been defined for the purpose of sending article metadata to Eprints and Dspace repositories that have
been "RIOXX enabled" through the installation of their respective RIOXX plugin / patch (which were developed and enhanced 
with Jisc funding).  This enables maximum information harvested by PubRouter to be passed on to these repositories.

The two schemas build upon elements from industry standard schemas:

| Namespace | URI | Description |
|:----------|:----|:------------|
| ali        | http://www.niso.org/schemas/ali/1.0/ | National Information Standards Organisation's Access and License Information schema  |
| dcterms    | http://purl.org/dc/terms/ | DublinCore (extended) terms schema |
| rioxxterms | http://www.rioxx.net/schema/v2.0/rioxx/ | RIOXX schema |

However, existing elements within these schemas are not sufficient to easily transmit more complex metadata that PubRouter
provides, such as:
* Author information, including ORCIDs and Emails
* Grant funding
* License information
* etc.

Accordingly we developed PubRouter schemas to support easy transfer of additional information to Eprints and DSpace.  The differences
between the two repositories necessitated development of different schemas for this purpose:

| Schema | Namespace | URI | Description |
|:--|:----------|:----|:------------|
| eprints-rioxx.xsd | pr    | http://pubrouter.jisc.ac.uk/rioxxplus/  | Jisc PubRouter-Eprints schema |
| dspace-rioxx.xsd | pubr   | http://pubrouter.jisc.ac.uk/dspacerioxx/ | Jisc PubRouter-DSpace schema |


NOTE that these schemas are still work-in-progress as they currently define only the new elements, and not the elements 
included from the industry standard schemas noted above.
