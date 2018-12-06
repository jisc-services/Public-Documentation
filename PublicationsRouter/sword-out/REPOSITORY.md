# User Information for Repositories taking SWORDv2 deposits

This document provides information on how to get your repository set up to take deposits from Router.

## PubRouter Account Setup

Firstly, you will need to contact [help@jisc.ac.uk](mailto:help@jisc.ac.uk?subject=PubRouter%20-%20account%20creation%20(institution)), mentioning Publications Router in your query, to have an account created. For that, you must provide:

* The matching parameters using this [Excel template](https://pubrouter.jisc.ac.uk/static/csvtemplate_router_matching_params_XLS_FORMAT.xlsx) or equivalent [csv template](https://pubrouter.jisc.ac.uk/static/csvtemplate.csv). IMPORTANT: if using the Excel template it should be saved as a CSV file upon completion, as this is the only format supported by PubRouter. 
* A *username* and *password* for an account in your repository which has a role with the rights to create content via SWORDv2:
   * For Eprints deposits into *Manage Deposits* the account needs *User* role
   * For Eprints deposits into the *Review queue* an *Editor* or *Admin* role is required
   * For DSpace deposits into a collection an *Admin* role on the collection is required
* A collection URL into which the system will deposit the content
   * Eprints collection: `http://eprints.<domain-name>.ac.uk/id/contents`
   * DSpace collection: `http://dspace.<domain-name>.ac.uk/swordv2/collection/<handle-id>/<collection-id>` (usually obtained from the collection URL `http://dspace.domain.ac.uk/handle/<handle-id>/<collection-id>` find out more here: [DSpace URLs and Identifiers](https://wiki.duraspace.org/display/DSDOC5x/Functional+Overview#FunctionalOverview-PersistentURLsandIdentifiers)).

Once account setup is complete, notifications which satisfy your matching parameters will be automatically deposited into your repository.

Any configuration can be updated at any time via your PubRouter Account page.

## EPrints

Router is only certified to work with EPrints 3.3. 

Two SWORD2 integration options exist:

* Eprints Native – this results in deposits with a basic set of meta-data
* Eprints RIOXXplus – this will populate additional RIOXX fields with meta-data.

### Eprints Native Integration

EPrints Native is configured by default to accept incoming requests via [SWORDv2](https://wiki.eprints.org/w/SWORD_2.0), so the router's deposit mechanism will automatically work with your repository.

### Eprints RIOXXplus Integration

Eprints RIOXXplus integration is appropriate only where your repository has the RIOXX plugin installed, it enables additional fields required by the [RIOXX application profile](http://rioxx.net/v2-0-final/) to be automatically populated by PubRouter.

In order to take advantage of PubRouter's RIOXXplus option your repository will need the [Jisc PubRouter RIOXXplus Connector Plugin](http://wiki.eprints.org/w/Jisc_Publications_Router) to be installed, and your PubRouter account set to "Eprints RIOXX” (via your Account Admin page).

### Deposit Process

The behaviour during an individual notification deposit is as follows:

1. Metadata will be deposited as a new item into the collection you specified in your PubRouter account settings

2. The raw metadata provided by the publisher may also be supplied as an XML file attached to the item

3. If article fulltext files, such as PDFs, are available they will be attached to the item

4. The item will be left in the sword user's workarea or review queue (depending on the deposit location selected on your PubRouter Account Admin page).

## DSpace

Router is certified to work with Dspace 5.x.

Two SWORD2 integration options exist:

* DSpace Native – this results in deposits with a basic set of meta-data
* DSpace-RIOXX – this will populate additional RIOXX fields with meta-data (if the RIOXX patch is installed).

Dspace needs to be configured and enabled following [these instructions](https://wiki.duraspace.org/display/DSDOC5x/SWORDv2+Server).

Router will create a new item in your repository that includes an Atom Entry XML file containing all [PubRouter metadata](./).  This means that even if your repository's crosswalk (XSLT) does not yet automatically extract all metadata, the full set will remain available.

### DSpace Native Integration
DSpace Native will populate your repository with a broad selection of [Dublin Core](http://dublincore.org/documents/dcmi-terms/) fields; see [here](https://wiki.duraspace.org/display/DSDOC5x/Metadata+and+Bitstream+Format+Registries) for more information.  This mode of integration should work with any existing DSpace v5.x repository.

### DSpace RIOXX Integration
If you wish to receive [RIOXX](http://rioxx.net/v2-0-final/) metadata in addition to the standard [Dublin Core](http://dublincore.org/documents/dcmi-terms/) fields from PubRouter, then you must have applied the [DSpace RIOXX patch](https://github.com/atmire/RIOXX) to your repository.

### Deposit Process

The behaviour during an individual notification deposit is as follows:

1. Metadata will be deposited as a new item into the collection you specified in your account settings

2. The raw metadata provided by the publisher may also be supplied as an XML file attached to the item

3. If article fulltext files are available they will be attached to the item

4. Upon completion, the item will be injected into the collection's workflow.
 
### Metadata Customisation

All the metadata will be converted to DSpace Item metadata following the rules laid down in the swordv2-server.cfg file, which
can be found in

    dspace/config/modules/swordv2-server.cfg
    
It is possible to edit this configuration file to affect which DSpace Item metadata fields are populated by PubRouter- see [here](https://wiki.duraspace.org/display/DSDOC5x/Metadata+and+Bitstream+Format+Registries) for more information.


If you need any help with repository plugins then refer to [JISC Repository Technical Support](https://www.jisc.ac.uk/repository-technical-support).
