# User Information for Repositories taking SWORDv2 deposits

This document provides information on how to get your repository set up to take deposits from Router.

## Account Setup

First you will need a Router account, and set up the SWORDv2 credentials in the 'Manage connection settings' section on your account.  You need to provide the following:

* The repository URL
* Select a Repository Configuration ([Eprints Native](#eprints), [Eprints RIOXX](#eprints-rioxx-plugin), [DSpace native](#dspaceother))
* Select an Eprints deposit location if required (Manage Deposit, Review Queue)
* A username and password for a user in your repository who has the rights to create content via SWORDv2.
   * For Eprints deposits into *Manage Deposits* a User role is required.
   * For Eprints deposits into *Review* queue an Editor or Admin role is required.
   * For DSpace deposits into a collection an Admin role on the collection is required.
* A collection URL into which the system will deposit the content
   * Eprints collection: http://eprints.domain.ac.uk/id/contents
   * DSpace collection: http://dspace.domain.ac.uk/swordv2/collection/<handle-id>/<collection-id> (usually obtained from the collection URL http://dspace.domain.ac.uk/handle/<handle-id>/<collection-id> find out more: [DSpace URLs and Identifiers](https://wiki.duraspace.org/display/DSDOC5x/Functional+Overview#FunctionalOverview-PersistentURLsandIdentifiers))
* Your preferred packaging format for zip files being deposited. [more info](https://github.com/sherpaservices/Public-Documentation/blob/master/PublicationsRouter/v2/api/Packaging.md#what-package-formats-are-supported)
   * http://purl.org/net/sword/package/SimpleZip (by default)
   * https://pubrouter.jisc.ac.uk/FilesAndJATS

## EPrints

Router is only certified to work with EPrints 3.3. There are 2 different formats to deposit notifications into Eprints: Vanilla Eprints and Eprints RIOXX. 

### Vanilla Eprints

Vanilla EPrints is the native format and it is configured by default to accept incoming requests via [SWORDv2](https://wiki.eprints.org/w/SWORD_2.0), so the router's deposit mechanism will automatically work.  

### Eprints RIOXX Plugin

Eprints RIOXX format provides consistency to the metadata and is designed to support the consistent tracking of open-access research publications across scholarly systems.

The final version of the [*Jisc PubRouter RIOXXplus Connector Plugin*](http://wiki.eprints.org/w/Jisc_Publications_Router) is now available and it is necessary to install it on your Eprints repository in order to receive notifications in this format.

In summary, the following would need to be done:

1.	Install the Jisc PubRouter RIOXXplus Connector Plugin from the Eprints Bazaar (this would normally be done by an Eprints administrator)

2.	Configure the plugin - it requires your PubRouter API key to be entered (again a job for an Eprints administrator)

3.	Update your PubRouter account to  change the repository configuration from Eprints Native to Eprints RIOXXplus.


### How the deposit will look

The behaviour during individual notification deposit you will see is as follows:

1. Metadata will be deposited and crosswalked as per the EPrints Atom Import plugin (see below) into the collection you specified in your account (most likely /id/content)

2. The metadata will also be supplied as an XML file attached to the eprint

3. If there are fulltext files associated, they will be attached to the eprint

4. The EPrint will be left in the sword user's workarea or review queue, depending on the deposit location that has been selected in PubRouter Admin page.


### Metadata ingest and crosswalk

All metadata will be converted to EPrints metadata following the rules laid down in the Atom XSLT import plugin.
This crosswalk is part of your EPrints souce, and can be found in:

    /opt/eprints3/perl_lib/EPrints/Plugin/Import/XSLT/Atom.xsl

By default it will crosswalk the absolute basic Atom metadata, so you may wish to modify this file to fit with your EPrints schema as you see fit.

In order that you do not lose any information if your XSLT does not yet do what you want, Router will also deposit
a full copy of the Atom Entry XML as a file attached to the eprint.  For information about the metadata held in that
file see the [Crosswalk Documentation](https://github.com/sherpaservices/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/XWALK.md#jper-core-metadata-to-dublin-corerioxx-xml)

## DSpace/Other

Router is certified to work with Dspace 5.x

Dspace needs to be configured and enabled following these instructions: [SWORDv2 Server](https://wiki.duraspace.org/display/DSDOC5x/SWORDv2+Server)


### How the deposit will look

The behaviour during individual notification deposit you will see is as follows:

1. Metadata will be deposited and crosswalked as per the DSpace SWORDv2 configuration (see below) into the collection you specified in your account

2. If there are fulltext files associated, they will be attached to the Item

3. Upon completion, the item will be injected into the collection's workflow
 
### Metadata ingest and crosswalk

All the metadata will be converted to DSpace Item metadata following the rules laid down in the swordv2-server.cfg file, which
can be found in

    dspace/config/modules/swordv2-server.cfg
    
By default it will crosswalk a broad selection of Dublin Core fields, but this may be customised. See [here](https://wiki.duraspace.org/display/DSDOC5x/Metadata+and+Bitstream+Format+Registries) for more information.

If you wish to also support RIOXX into your DSpace repository, there are some repository pacthes (versions 3, 4 or 5) available on request by email to info@atmire.com.




