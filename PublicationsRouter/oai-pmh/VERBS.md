# OAI-PMH Verbs implemented by Publications Router

This document describes the various [OAI-PMH](http://www.openarchives.org/OAI/openarchivesprotocol.html) verbs and their interpretation by Publications Router:
* [Identify](#identify)
* [ListMetadataFormats](#listmetadataformats)
* [ListIdentifiers](#listidentifiers)
* [ListRecords](#listrecords)
* [GetRecord](#getrecord)
* ListSets (unsupported)


The OAI-PMH endpoint must be targetted at a specific institution's dataset, like so:

* `https://pubrouter.jisc.ac.uk/oaipmh/repo/<institution_id>` where *<institution_id>* is the account identifier of the institution.

Note that OAI-PMH sets are not supported by Publications Router.

## Identify

This verb asks the OAI-PMH server to identify itself and provide some useful information for the client.

Incoming parameters:

* verb: Identify

Returned information:

* Repository Name: from configuration
* Email address: from configuration
* Earliest datestamp: 3 months before current date

Example request: 

```GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=Identify```

Example response: 
```xml
<?xml version='1.0' encoding='UTF-8'?>
<OAI-PMH xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.openarchives.org/OAI/2.0/" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/ http://www.openarchives.org/OAI/2.0/OAI-PMH.xsd">
    <responseDate>2018-05-23T14:18:25Z</responseDate>
    <request verb="Identify">http://pubrouter.jisc.ac.uk/repo/123456789</request>
    <Identify>
        <repositoryName>Jisc Publications Router OAI-PMH Endpoint</repositoryName>
        <baseURL>http://pubrouter.jisc.ac.uk/repo/123456789</baseURL>
        <protocolVersion>2.0</protocolVersion>
        <adminEmail>pubrouter@jisc.ac.uk</adminEmail>
        <earliestDatestamp>2018-02-22T13:18:25Z</earliestDatestamp>
        <deletedRecord>transient</deletedRecord>
        <granularity>YYYY-MM-DDThh:mm:ssZ</granularity>
    </Identify>
</OAI-PMH>
```

## ListMetadataFormats

This verb asks the OAI-PMH server to list the metadata formats that are available to the client

Incoming parameters:

* verb: ListMetadataFormats
* identifier: notification id (optional)

Returned information:

* Metadata Format: oai_dc (schema: http://www.openarchives.org/OAI/2.0/oai_dc.xsd, namespace: http://www.openarchives.org/OAI/2.0/oai_dc/)

Example request: 

```GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=ListMetadataFormats```

Example response: 
```xml
<?xml version='1.0' encoding='UTF-8'?>
<OAI-PMH xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.openarchives.org/OAI/2.0/" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/ http://www.openarchives.org/OAI/2.0/OAI-PMH.xsd">
    <responseDate>2018-05-23T14:21:04Z</responseDate>
    <request verb="ListMetadataFormats">http://pubrouter.jisc.ac.uk/repo/123456789</request>
    <ListMetadataFormats>
        <metadataFormat>
            <metadataPrefix>oai_dc</metadataPrefix>
            <schema>http://www.openarchives.org/OAI/2.0/oai_dc.xsd</schema>
            <metadataNamespace>http://www.openarchives.org/OAI/2.0/oai_dc/</metadataNamespace>
        </metadataFormat>
    </ListMetadataFormats>
</OAI-PMH>
```


## ListIdentifiers

This verb asks the OAI-PMH server to list the identifiers of all records which match the parameters of the request:

* verb: ListIdentifiers
* from: from-date lower bound for request (optional) this may be in one of two formats: YYYY-MM-DD or YYYY-MM-DDTHH:MM:SSZ (e.g. 2016-09-23T11:30:45Z)
* until: upper bound for request (UNSUPPORTED)
* metadataPrefix: metadata format supported by identifier
* set: set to retrieve from (UNSUPPORTED)
* resumptionToken: paging control from the previous request (exclusive)

Returned information:

* Identifiers: notification id
* Resumption Token: base64 encoded request parameters for next page

Example request: 

```GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=ListIdentifiers&from=2017-01-01&metadataPrefix=oai_dc```

Example response:
```xml
<?xml version='1.0' encoding='UTF-8'?>
<OAI-PMH xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.openarchives.org/OAI/2.0/" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/ http://www.openarchives.org/OAI/2.0/OAI-PMH.xsd">
    <responseDate>2018-05-23T14:23:36Z</responseDate>
    <request verb="ListIdentifiers" from="2017-01-01" metadataPrefix="oai_dc">http://pubrouter.jisc.ac.uk/repo/123456789</request>
    <ListIdentifiers>
        <header xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
            <identifier>oai:pubrouter.jisc.ac.uk/notification:987654321</identifier>
            <datestamp>2018-02-01T15:21:06Z</datestamp>
        </header>
        <header xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
            <identifier>oai:pubrouter.jisc.ac.uk/notification:987654322</identifier>
            <datestamp>2018-02-01T15:21:15Z</datestamp>
        </header>
        <header xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
            <identifier>oai:pubrouter.jisc.ac.uk/notification:987654323</identifier>
            <datestamp>2018-02-01T22:16:18Z</datestamp>
        </header>
        <resumptionToken completeListSize="4000" cursor="0" expirationDate="2018-05-24T14:54:04Z">eyJmIjogIjIwMTYtMDEtMDEiLCAibiI6IDEwMCwgIm0iOiAib2FpX2RjIn0=</resumptionToken>
    </ListIdentifiers>
</OAI-PMH>
```


## ListRecords

This verb asks the OAI-PMH server to list metadata for all records which match the parameters of the request:

* verb: ListRecords
* from: from-date lower bound for request (optional) this may be in one of two formats: YYYY-MM-DD or YYYY-MM-DDTHH:MM:SSZ (e.g. 2016-09-23T11:30:45Z)
* until: upper bound for request (UNSUPPORTED)
* metadataPrefix: metadata format supported by record
* set: set to retrieve from (UNSUPPORTED)
* resumptionToken: paging control from the previous request (exclusive)

Returned information:

* Records: notification metadata, only oai_dc metadata prefix is supported
* Resumption Token: base64 encoded request parameters for next page

Example request: 

```GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=ListRecords&from=2017-01-01&metadataPrefix=oai_dc```

Example response:
```xml
<?xml version='1.0' encoding='UTF-8'?>
<OAI-PMH xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.openarchives.org/OAI/2.0/" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/ http://www.openarchives.org/OAI/2.0/OAI-PMH.xsd">
    <responseDate>2018-05-23T14:26:53Z</responseDate>
    <request verb="ListRecords" from="2017-01-01" metadataPrefix="oai_dc">http://pubrouter.jisc.ac.uk/repo/123456789</request>
    <ListRecords>
        <record>
            <header xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
                <identifier>oai:pubrouter.jisc.ac.uk/notification:987654321</identifier>
                <datestamp>2018-02-01T15:21:06Z</datestamp>
            </header>
            <metadata xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
            ... (for example of metadata go to XWALK.md)
            </metadata>
        </record>
        <record>
            <header xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
                <identifier>oai:pubrouter.jisc.ac.uk/notification:987654322</identifier>
                <datestamp>2018-02-01T15:21:06Z</datestamp>
            </header>
            <metadata xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
            ... (for example of metadata go to XWALK.md)
            </metadata>
        </record>
        <resumptionToken completeListSize="4000" cursor="0" expirationDate="2018-05-24T14:54:04Z">eyJmIjogIjIwMTYtMDEtMDEiLCAibiI6IDEwMCwgIm0iOiAib2FpX2RjIn0=</resumptionToken>
    </ListRecords>
</OAI-PMH>
```

## Paging Control 
To use pagination, just make a request using the retrieved resumption token (together with verb ListRecords or ListIdentifiers).

Example request:

```
GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=ListRecords&resumptionToken=123781387136816813asdasd781813
```

## GetRecord

This verb asks the OAI-PMH server to provide the full metadata record for the identifier received:

* verb: GetRecord
* identifier: notification id
* metadataPrefix: metadata format supported by record

Returned information:

* Record: notification metadata, only oai_dc metadata prefix is supported

Example request: 

```
GET https://pubrouter.jisc.ac.uk/oaipmh/repo/123456789?verb=GetRecord&identifier=987654321&metadataPrefix=oai_dc
```

Example response: 
```xml
<?xml version='1.0' encoding='UTF-8'?>
<OAI-PMH xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.openarchives.org/OAI/2.0/" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/ http://www.openarchives.org/OAI/2.0/OAI-PMH.xsd">
    <responseDate>2018-05-23T14:29:54Z</responseDate>
    <request verb="GetRecord" identifier="oai:pubrouter.jisc.ac.uk/notification:d2b949b769e1451da6792fa10b1c420b" metadataPrefix="oai_dc">http://pubrouter.jisc.ac.uk/repo/123456789</request>
    <GetRecord>
        <record>
            <header xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
                <identifier>oai:pubrouter.jisc.ac.uk/notification:987654321</identifier>
                <datestamp>2018-02-01T15:21:06Z</datestamp>
            </header>
            <metadata xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/">
            ... (for example of metadata go to XWALK.md)
            </metadata>
        </record>
    </GetRecord>
</OAI-PMH>
```

## ListSets (unsupported)

This verb requests the OAI-PMH server to list the sets that are available to the client. Note that sets are not supported by Publications Router, so use of this verb will result in an empty list response.

Incoming parameters:

* verb: ListSets

Returned information

* An empty list - sets are not supported by Publications Router
