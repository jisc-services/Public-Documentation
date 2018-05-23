# PubRouter OAI-PMH endpoint

This document describes how to interact with the PubRouter OAI-PMH endpoint to retrieve the metadata records of successfully
routed notifications.

You may also wish to read the [OAI-PMH documentation](http://www.openarchives.org/OAI/openarchivesprotocol.html).

This endpoint only supports oai_dc formatted metadata, and can only access notifications going back 3 months.

## Routed notifications for a specific repository

The base endpoint of all OAI-PMH requests to PubRouter is: 

    https://pubrouter.jisc.ac.uk/oaipmh/repo/<repo id>

Where "repo_id" is the account id of the repository of whose notifications to retrieve.

For example:

    GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=ListRecords&from=2016-01-01&metadataPrefix=oai_dc
	
This request would return a list of all records for repository of id 123456789 from 2016-01-01, and the results would be in oai_dc formatted xml. 

Read about OAI-PMH **verbs** supported by PubRouter [here](./VERBS.md).

Read about the mapping from our native PubRouter notification format into the oai_dc formatted xml [here](./XWALK.md)