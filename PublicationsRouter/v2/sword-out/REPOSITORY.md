# User Information for Repositories taking SWORDv2 deposits

This document provides information on how to get your repository set up to take deposits from Router.

## Account Setup

Firstly, you will need to contact pubrouter@jisc.ac.uk to create an account. For that, you must provide:

* The matching parameters using the template linked [here](https://pubrouter.jisc.ac.uk/static/csvtemplate.csv).
* A *username* and *password* for a user in your repository who has the rights to create content via SWORDv2.
   * For Eprints deposits into *Manage Deposits* a User role is enough.
   * For Eprints deposits into the *Review queue* an Editor or Admin role is required.
   * For DSpace deposits into a collection an Admin role on the collection is required.
* A collection URL into which the system will deposit the content
   * Eprints collection: `http://eprints.<domain-name>.ac.uk/id/contents`
   * DSpace collection: `http://dspace.<domain-name>.ac.uk/swordv2/collection/<handle-id>/<collection-id>` (usually obtained from the collection URL `http://dspace.domain.ac.uk/handle/<handle-id>/<collection-id>` find out more: [DSpace URLs and Identifiers](https://wiki.duraspace.org/display/DSDOC5x/Functional+Overview#FunctionalOverview-PersistentURLsandIdentifiers))

Once setup is complete, notifications which satisfy your matching parameters will be automatically deposited into your repository.

Any configuration can be updated at any time in the PubRouter Account page.

## EPrints

Router is only certified to work with EPrints 3.3. Two modes of integration are available:

* Eprints Native – This results in deposits with a basic set of meta-data.
* Eprints RIOXX - This will populate the additional RIOXX fields with meta-data.

### Eprints Native

EPrints Native is configured by default to accept incoming requests via [SWORDv2](https://wiki.eprints.org/w/SWORD_2.0), so the router's deposit mechanism will automatically work.  

### Eprints RIOXX Plugin

Eprints RIOXX format provides metadata consistency by capturing additional fields required by the [RIOXX application profile](http://rioxx.net/v2-0-final/).

If your Eprints repository has the RIOXX plugin installed, then PubRouter can better populate the RIOXX fields if you also install the [*Jisc PubRouter RIOXXplus Connector Plugin*](http://wiki.eprints.org/w/Jisc_Publications_Router) and set PubRouter account repository configuration to "Eprints RIOXX”.

### Deposit process

The behaviour during an individual notification deposit is as follows:

1. Metadata will be deposited as a new item into the collection you specified in your account settings.

2. The raw metadata provided by the publisher may also be supplied as an XML file attached to the item.

3. If there are fulltext files associated, such as PDFs of the  they will be attached to the item.

4. The item will be left in the sword user's workarea or review queue, depending on the deposit location that has been selected in the user's PubRouter Admin page.

## DSpace

Router is certified to work with Dspace 5.x


Dspace needs to be configured and enabled following [these instructions](https://wiki.duraspace.org/display/DSDOC5x/SWORDv2+Server).
In order that you do not lose any information if your XSLT does not yet do what you want, Router will also deposit
a full copy of the Atom Entry XML as a file attached to the eprint.  For information about the metadata held in that
file see the [Crosswalk Documentation](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/XWALK.md#jper-core-metadata-to-dublin-corerioxx-xml)


### Deposit process

The behaviour during an individual notification deposit is as follows:

1. Metadata will be deposited as a new item into the collection you specified in your account settings.

2. The raw metadata provided by the publisher may also be supplied as an XML file attached to the item.

3. If there are fulltext files associated, they will be attached to the item.

4. Upon completion, the item will be injected into the collection's workflow
 
### Metadata customisation

All the metadata will be converted to DSpace Item metadata following the rules laid down in the swordv2-server.cfg file, which
can be found in

    dspace/config/modules/swordv2-server.cfg
    
By default the conversion will crosswalk a broad selection of Dublin Core fields, but this may be customised. See [here](https://wiki.duraspace.org/display/DSDOC5x/Metadata+and+Bitstream+Format+Registries) for more information.

If you wish to also support RIOXX into your DSpace repository, there are some repository patches (versions 3, 4 or 5) available on request by email to info@atmire.com.



If you need any help with repository plugins then refer to [JISC Repository Technical Support](https://www.jisc.ac.uk/repository-technical-support)
