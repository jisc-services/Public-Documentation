# PubRouter OAI-PMH endpoint

This document describes how to interact with the PubRouter OAI-PMH endpoint to retrieve the metadata records of successfully
routed notifications.

You may also wish to read the [OAI-PMH documentation](http://www.openarchives.org/OAI/openarchivesprotocol.html).

## The OAI-PMH endpoint

The base endpoint of all OAI-PMH requests to PubRouter is: 

    https://pubrouter.jisc.ac.uk/oaipmh
    
Though this endpoint can currently only be used with reference to a specific repository. So the effective endpoint would actually be: 
```
https://pubrouter.jisc.ac.uk/oaipmh/repo/<institution id>
```

Where "institution_id" is the account id of the Institution whose notifications are to be retrieved.

For example:

    GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=ListRecords&from=2016-01-01&metadataPrefix=oai_dc
	
This request would return a list of all records for institution with id 123456789 from 2016-01-01, and the results would be in oai_dc formatted xml. (Note that due to PubRouter's 3 month retention period, this would not find any records older than that.)

This endpoint currently only supports oai_dc formatted metadata, and can only access notifications going back 3 months.

## Using OAI-PMH with PubRouter
Read about [OAI-PMH **verbs**](./VERBS.md) supported by PubRouter.

## Data returned by PubRouter
Read about the mapping from our native PubRouter notification format into the [oai_dc formatted xml](./XWALK.md).
