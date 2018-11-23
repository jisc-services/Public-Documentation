# API for sending notifications (Publishers)

The current version of the API is v3, and it can be accessed at

    https://pubrouter.jisc.ac.uk/api/v3

All URL paths provided in this document will extend from this base url.

If you are a publisher (also referred to here as a "provider") providing content to PubRouter, you have access to 2 endpoints:

1. **Validation endpoint** - for use during initial set-up and testing of your API client, to validate the content your send to PubRouter
2. **Notification endpoint** - for "live" use, sending real notifications to PubRouter

You can create content in 2 ways in PubRouter:

1. As a **metadata-only notification** - which allows you to provide publication information in our native JSON format as an [Incoming Notification](./IncomingNotification.md#incoming-notification).

   There are 2 endpoints for this, one submits a single notification, the other a list of notifications.

2. As a **metadata + binary package notification** - which allows you to give us a multi-part request containing the publication information which complies with our native JSON format as an [Incoming Notification](./IncomingNotification.md#incoming-notification) plus a zipped binary package containing content in a supported [Packaging Format](./Packaging.md#packaging).

   These can be sent using only the single notification endpoint (not via the list of notifications endpoint).

The following sections describe the HTTP methods, headers, body content and expected responses for each of the above endpoints and content.

#### Important information about notification metadata

If you are providing metadata, you should include as much institution and author identifying metadata as possible to give us the best chance of routing the content to a suitable repository; and as much bibliographic data as possible to provide institutions with a rich set of information.

For effective routing, this includes:
* Author identifiers (e.g. ORCID or email address or other)
* Grant numbers
* Institution identifiers (e.g. post-codes, domain names)
* Institution name(s)

These fields are highlighted in the [Incoming Notification table of fields](./IncomingNotification.md#field-definitions).

#### Embargo

If you are applying an embargo to the content this can be indicated via the **embargo.end** field, or alternatively the **embargo.start** and **embargo.duration**.

#### Links to your publicly hosted content

If you have publicly hosted content (e.g. splash pages, full-text web pages, or PDFs) that you want to share with PubRouter, so that repositories can download the content directly, you may provide these in a **links** element.  For example:

    "links" : [
        {
            "type" : "splash",
            "format" : "text/html",
            "url" : "http://example.com/article1/index.html",
        },
        {
            "type" : "fulltext",
            "format" : "text/html",
            "url" : "http://example.com/article1/fulltext.html",
        },
        {
            "type" : "fulltext",
            "format" : "application/pdf",
            "url" : "http://example.com/article1/fulltext.pdf",
        }
    ]

---

## Validation Endpoints

You must have **Publisher account** to access this endpoint. The Validation API allows you to test that your data feed to the system will be successful.

### Possible HTTP Responses

Any of the validation endpoints listed below will return one of these responses.

- On **authentication failure** (e.g. invalid api_key, incorrect user role) the API will respond with a **401 (Unauthorised)** and no response body.
```
    HTTP 1.1  401 Unauthorized
```

- On **validation failure** the system will respond with the following:
```
    HTTP 1.1  400 Bad Request
    Content-Type: application/json

    {
        "status": "error",
        "error" : "human readable error message"
    }
```
- On **validation success** the system will respond with 204 (No Content) and no response body.
```
    HTTP 1.1  204 No Content
```

NOTE: in the following sections reference to "{Incoming notification JSON object}" means the Incoming Notification [JSON data structure](./IncomingNotification.md#json-data-structure).

### 1. Validate Metadata-only request

If you are sending only the notification JSON, the request must take the form:

    POST /validate?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        {Incoming notification JSON object}

### 2. Validate Metadata + Package request

If you are sending binary content as well as the metadata, the request should be formed using [RFC 2387](https://www.ietf.org/rfc/rfc2387.txt):

    POST /validate?api_key=<api_key>
    Header:
        Content-Type: multipart/related; boundary=------------------------586e648803c83e39
        
    Body:
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="metadata"; filename="metadata.json"
        Content-Type: application/json
        
        {{Incoming notification JSON}}
        
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="content"; filename="content.zip"
        Content-Type: application/zip
        
        {{Zip Content}}

        --------------------------586e648803c83e39---

This is quite simple to do in curl by using the -F flag:

```bash
curl -H 'Content-Type: multipart/related' -F 'metadata=@metadata.json;type=application/json;filename="metadata.json"' -F 'content=@myzip.zip;type=application/zip;filename="content.zip"' https://pubrouter.jisc.ac.uk/api/v3/validate?api_key=<my_api_key>
```

If you are carrying out this request you MUST include the **content.packaging_format** field in the notification metadata and populate it with the appropriate format identifier as per the [Packaging Format](./Packaging.md#packaging) documentation.

### 3. Validate Minimum Metadata + Package request

It is possible to send a request with virtually no JSON metadata, instead relying on metadata embedded in an XML file in the binary Package (e.g. in a JATS XML structure).

To do this, send the bare-minimum JSON notification, with only the format identifier of the [package](./Packaging.md#packaging) included.  For example:

    POST /validate?api_key=<api_key>
    Header:
        Content-Type: multipart/related; boundary=------------------------586e648803c83e39
    
    Body:
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="metadata"; filename="metadata.json"
        Content-Type: application/json
        
        {
            "content": {
                "packaging_format": "https://pubrouter.jisc.ac.uk/FilesAndJATS"
            }
        }
        
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="content"; filename="content.zip"
        Content-Type: application/zip

        {{Zip Content}}       
        
        --------------------------586e648803c83e39---
    

### 4. Validate List of notifications with Metadata-only request

If you are sending a list of notifications, the request must take the form shown below.  Note that only metadata can be sent in this way (binary content is not supported):

    POST /validate/list?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        # List of Incoming Notification JSON
        [{"notification": {Incoming notification JSON object}, "id": 1},
         {"notification": {Incoming notification JSON object}, "id": 2},
         {"notification": {Incoming notification JSON object}, "id": 3} ... ]

NOTE: Make sure that an ID is sent for each Incoming notification as these will be returned in the success or error list.  You can have any value for the ID (we have shown integers, but you may use something else, to be useful they should be unique within the list you are submitting).  These IDs are not stored by PubRouter but simply used in reporting the success/failure of the submissions in the response to the API call HTTP request.

---

## Notification Endpoints (for sending notifications to PubRouter)

The Notification API endpoints takes an identical request (header and body) to the Validation API endpoints, so that you can develop against the Validation API and then switch seamlessly over to live notifications. However, there will be a difference in the response body that is received.

Again, you must have "Publisher account" to access this endpoint.

The system will not attempt to aggressively validate the request, but the request must still be well-formed in order to succeed, so you may still receive a validation error.

On a successful call to this endpoint, your notification will be accepted into PubRouter where it will be queued for subsequent processing and routing to matched repositories.


### Responses

Any of the notification endpoints listed below will return one of these responses.

Note these are different from the Validation endpoint.

#### Error Responses ####
* On **authentication failure** (e.g. invalid api_key, incorrect user role) the system will respond with a **401 (Unauthorised)** and no response body.

```
    HTTP 1.1  401 Unauthorized
```

* In the event of a **malformed HTTP request**, the system will respond with a **400 (Bad Request)** and the response body:

```
    HTTP 1.1  400 Bad Request
    Content-Type: application/json

    {
        "status": "error",
        "error" : "human readable error message"
    }
```

#### Responses only for Notification List endpoint ####
* If the list contains something that is not a JSON object and none of the submitted notifications were successfully processed then the system will respond with a **406 (Not Acceptable)** and the response body:

```
    HTTP 1.1  406 Not Acceptable
    Content-Type: application/json

    {
        "successful": 0,
        "total": <the number of items received in the list>,
        "success_ids": [],
        "fail_ids": [ <list of IDs of notifications that could not be processed> ],
        "last_error": "human readable error message"
    }
```

* If the list contains something that is not a JSON object, but before it is encountered at least one notification in the list was successfuly processed then the system will respond with a **206 (Partial Content)** and the response body:

```
    HTTP 1.1  206 Partial Content
    Content-Type: application/json

    {
        "successful": <number of successfully processed notifications>,
        "total": <the number of items received in the list>,
        "success_ids": [ <list of IDs of successfully processed notifications> ],
        "fail_ids": [ <list of IDs of notifications that could not be processed> ],
        "last_error": "A notification in the list is not a JSON object, the id of the latest
                    notification processed was '5'. Error: <human readable error message>"
    }
```

#### Success Response - Single Notification ####
* On **successful completion** of the request, the system will respond with 202 (Accepted) and the following response body

```
    HTTP 1.1  202 Accepted
    Content-Type: application/json
    Location: <url for api endpoint for accepted notification>

    {
        "status" : "accepted",
        "id" : "<unique identifier for the notification>",
        "location" : "<url path for api endpoint for newly created notification>"
    }
```

#### Success Response - Notification List ####
* On **successful completion** of the request, the system will respond with 202 (Accepted) and the following response body.  Note you may obtain this successful notification even if some of the notifications in the list could not be processed - these are identified in the fail_ids list.

```
    HTTP 1.1  202 Accepted
    Content-Type: application/json

    {
        "successful": <number of successfully processed notifications>,
        "total": <the number of items received in the list>,
        "success_ids": [ <list of IDs of successfully processed notifications> ],
        "fail_ids": [ <list of IDs of notifications that could not be processed> ],
        "last_error": "<Last error message>"
    }
```

### 1. Notification Metadata-only request

If you are sending only the notification JSON, the request must take the form:

    POST /notification?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        {Incoming Notification JSON}

### 2. Notification Metadata + Package request

If you are sending binary content as well as the metadata, the request must take the form:

    POST /notification?api_key=<api_key>
    Header:
        Content-Type: multipart/related; boundary=------------------------586e648803c83e39
    
    Body:
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="metadata"; filename="metadata.json"
        Content-Type: application/json
        
        {{Incoming Notification JSON}}
        
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="content"; filename="content.zip"
        Content-Type: application/zip

        {{Zip Content}}       
        
        --------------------------586e648803c83e39---

If you are carrying out this request you MUST include the **content.packaging_format** field in the notification metadata and populate it with the appropriate format identifier as per the [Packaging Format](./Packaging.md#packaging) documentation.

### 3. Notification Minimum Metadata + Package request

It is possible to send a request with virtually no JSON metadata, instead relying on metadata embedded in an XML file in the binary Package (e.g. in a JATS XML structure).

To do this, send the bare-minimum JSON notification, with only the format identifier of the [package](./Packaging.md#packaging) included.

For example:

    POST /notification?api_key=<api_key>
    Header:
        Content-Type: multipart/related; boundary=------------------------586e648803c83e39
    
    Body:
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="metadata"; filename="metadata.json"
        Content-Type: application/json
        
        {
            "content": {
                "packaging_format": "https://pubrouter.jisc.ac.uk/FilesAndJATS"
            }
        }
        
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="content"; filename="content.zip"
        Content-Type: application/zip

        {{Zip Content}}       
        
        --------------------------586e648803c83e39---

### 4. Notification List with Metadata-only request

If you are sending a list of notifications, the request must take the form:

    POST /notification/list?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        # List of Incoming Notification JSON
        [{"notification": {Incoming notification JSON object}, "id": 1},
         {"notification": {Incoming notification JSON object}, "id": 2},
         {"notification": {Incoming notification JSON object}, "id": 3} ... ]

NOTE: Make sure that an ID is sent for each Incoming notification as these will be returned in the success or error list.  You can have any value for the ID (we have shown integers, but you may use something else, to be useful they should be unique within the list you are submitting).  These IDs are not stored by PubRouter but simply used in reporting the success/failure of the submissions in the response to the API call HTTP request.


---
