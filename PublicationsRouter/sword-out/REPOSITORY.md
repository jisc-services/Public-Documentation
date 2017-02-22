# User Information for Repositories taking SWORDv2 deposits

This document provides information on how to get your repository set up to take deposits from Router.

## Account Setup

First you will need a Router account, and to configure it for SWORDv2 integration.  See your account page in the 
router for more information.  You need to provide the following:

1. A username and password for a user in your repository who has the rights to create content via SWORDv2
2. A collection URL into which the system will deposit the content
3. Your preferred packaging format for zip files being deposited.  By default (and the only option in this first version) this will be http://purl.org/net/sword/package/SimpleZip

## EPrints

Router is only certified to work with EPrints 3.3.

EPrints is configured by default to accept incoming requests via SWORDv2, so the router's deposit mechanism
will automatically work.  

### How the deposit will look

The behaviour during individual notification deposit you will see is as follows:

1. Metadata will be deposited and crosswalked as per the EPrints Atom Import plugin (see below) into the collection you specified in your account (most likely /id/content)

2. The metadata will also be supplied as an XML file attached to the eprint

3. If there are fulltext files associated, they will be attached to the eprint

4. The EPrint will be left in the sword user's workarea


### Metadata ingest and crosswalk

All metadata will be converted to EPrints metadata following the rules laid down in the Atom XSLT import plugin.
This crosswalk is part of your EPrints souce, and can be found in:

    /opt/eprints3/perl_lib/EPrints/Plugin/Import/XSLT/Atom.xsl

By default it will crosswalk the absolute basic Atom metadata, so you may wish to modify this file to fit with your EPrints schema as you see fit.

In order that you do not lose any information if your XSLT does not yet do what you want, Router will also deposit
a full copy of the Atom Entry XML as a file attached to the eprint.  For information about the metadata held in that
file see the [Crosswalk Documentation](https://github.com/JiscPER/jper-sword-out/blob/develop/docs/system/XWALK.md)

## DSpace/Other

Router has so far had limited use on DSpace, so documentation is subject to change.

### How the deposit will look

The behaviour during individual notification deposit you will see is as follows:

1. Metadata will be deposited and crosswalked as per the DSpace SWORDv2 configuration (see below) into the collection you specified in your account

2. If there are fulltext files associated, they will be attached to the Item

3. Upon completion, the item will be injected into the collection's workflow
 
### Metadata ingest and crosswalk

All the metadata will be converted to DSpace Item metadata following the rules laid down in the swordv2-server.cfg file, which
can be found in

    dspace/config/modules/swordv2-server.cfg
    
By default it will crosswalk a broad selection of Dublin Core fields, and you can customise that here.  If you wish to also support RIOXX you may need to
extend your EntryIngester implementation.




