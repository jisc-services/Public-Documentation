# User Information for Repositories taking SWORDv2 deposits

This document provides information on how to get your repository set up to take deposits from Router.

## Account Setup

Firstly, you will need to contact help@jisc.ac.uk, mentioning Publications Router in your query, to have an account created. For that, you must provide:

* The matching parameters using this [Excel template](https://pubrouter.jisc.ac.uk/static/csvtemplate_router_matching_params_XLS_FORMAT.xlsx) or equivalent [csv template](https://pubrouter.jisc.ac.uk/static/csvtemplate.csv). IMPORTANT - if using the Excel template, then when completed you should save it as a CSV file. 
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

Router is only certified to work with EPrints 3.3. Two SWORD2 integration options exist:

* Eprints Native – this results in deposits with a basic set of meta-data
* Eprints RIOXXplus – this will populate additional RIOXX fields with meta-data.

### Eprints Native

EPrints Native is configured by default to accept incoming requests via [SWORDv2](https://wiki.eprints.org/w/SWORD_2.0), so the router's deposit mechanism will automatically work with your repository.

### Eprints RIOXXplus

Eprints RIOXXplus integration is appropriate only where your repository has the RIOXX plugin installed, it enables additional fields required by the [RIOXX application profile](http://rioxx.net/v2-0-final/) to be automatically populated by PubRouter.

In order to take advantage of PubRouter's RIOXXplus option your repository will need the [*Jisc PubRouter RIOXXplus Connector Plugin*](http://wiki.eprints.org/w/Jisc_Publications_Router) to be installed, and your PubRouter account set to "Eprints RIOXX” (via your Account Admin page).

### Deposit process

The behaviour during an individual notification deposit is as follows:

1. Metadata will be deposited as a new item into the collection you specified in your account settings

2. The raw metadata provided by the publisher may also be supplied as an XML file attached to the item

3. If article fulltext files, such as PDFs, are available they will be attached to the item

4. The item will be left in the sword user's workarea or review queue (depending on the deposit location selected on your PubRouter Account Admin page).

## DSpace

Router is certified to work with Dspace 5.x


Dspace needs to be configured and enabled following [these instructions](https://wiki.duraspace.org/display/DSDOC5x/SWORDv2+Server).

Router will create a new item in your repository that includes an Atom Entry XML file containing all [PubRouter metadata](./).  This means that even if your repository's crosswalk (XSLT) does not yet automatically extract all metadata, the full set will remain available.


### Deposit process

The behaviour during an individual notification deposit is as follows:

1. Metadata will be deposited as a new item into the collection you specified in your account settings

2. The raw metadata provided by the publisher may also be supplied as an XML file attached to the item

3. If article fulltext files are available they will be attached to the item

4. Upon completion, the item will be injected into the collection's workflow.
 
### Metadata customisation

All the metadata will be converted to DSpace Item metadata following the rules laid down in the swordv2-server.cfg file, which
can be found in

    dspace/config/modules/swordv2-server.cfg
    
By default the conversion will crosswalk a broad selection of Dublin Core fields, but this may be customised. See [here](https://wiki.duraspace.org/display/DSDOC5x/Metadata+and+Bitstream+Format+Registries) for more information.

If you wish to also support RIOXX into your DSpace repository, there are some repository patches (versions 3, 4 or 5) available on request by email to info@atmire.com.



If you need any help with repository plugins then refer to [JISC Repository Technical Support](https://www.jisc.ac.uk/repository-technical-support).
