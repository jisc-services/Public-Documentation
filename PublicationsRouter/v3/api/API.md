# Jisc Publications Router API (PubRouter API)

Jisc Publications Router exposes an REST API that may be used to submit and/or retrieve publication notifications.  These pages document the API interfaces and data formats.

The current version of the API is v2, and it can be accessed at

    https://pubrouter.jisc.ac.uk/api/v2

All URL paths provided in this document will extend from this base url.

In many cases you will need an API key to access the API.  This can be obtained from your PubRouter account page.

### Contents ###
This page has the following contents:

* [API for sending notifications (Publishers)](./API.md#api-for-sending-notifications-publishers)

  * Validation Endpoints - for use during development to check your API requests
 
    * Validate single notification metadata only submission
    * Validate single notification Metadata + Document (binary package) submission
    * Validate single notification minimum Metadata + Document (binary package) submission
    * Validate list of notification metadata submissions

  * Notification Endpoints - for "live" submission of notifications to PubRouter
  
    * Submit single notification metadata only
    * Submit single notification Metadata + Document (binary package)
    * Submit single notification minimum Metadata + Document (binary package)
    * Submit  list of notification metadata
  
 
* [API for retrieving notifications](./API.md#api-for-retrieving-notifications)

### Alternatives to REST API ###

These pages describe PubRouter's REST API, however there are other means by which Publishers may send information to PubRouter (SWORD2 or FTP); and by which Repositories may receive information (OAI-PMH and SWORD2).  More information on these is available on the  PubRouter website [introduction](https://pubrouter.jisc.ac.uk/about/) and [technical overview](https://pubrouter.jisc.ac.uk/about/resources/).

---

# API for sending notifications (Publishers)

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

If you are sending binary content as well as the metadata, the request must take the form:

    POST /validate?api_key=<api_key>
    Header:
        Content-Type: multipart/form-data; boundary=FulltextBoundary
    
    Body:
        --FulltextBoundary

        Content-Disposition: form-data; name="metadata"
        Content-Type: application/json

        {Incoming Notification JSON}

        --FulltextBoundary

        Content-Disposition: form-data; name="content"
        Content-Type: application/zip

        Binary Package

        --FulltextBoundary--

If you are carrying out this request you MUST include the **content.packaging_format** field in the notification metadata and populate it with the appropriate format identifier as per the [Packaging Format](./Packaging.md#packaging) documentation.

### 3. Validate Minimum Metadata + Package request

It is possible to send a request with virtually no JSON metadata, instead relying on metadata embedded in an XML file in the binary Package (e.g. in a JATS XML structure).

To do this, send the bare-minimum JSON notification, with only the format identifier of the [package](./Packaging.md#packaging) included.  For example:

    POST /validate?api_key=<api_key>
    Header:
        Content-Type: multipart/form-data; boundary=FulltextBoundary
    
    Body:
        --FulltextBoundary

        Content-Disposition: form-data; name="metadata"
        Content-Type: application/json

        {
            "content" : {
                "packaging_format" : "https://pubsrouter.jisc.ac.uk/FilesAndJATS"
            },
        }

        --FulltextBoundary

        Content-Disposition: form-data; name="content"
        Content-Type: application/zip

        Binary Package

        --FulltextBoundary--
        

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
        Content-Type: multipart/form-data; boundary=FulltextBoundary
    
    Body:
        --FulltextBoundary

        Content-Disposition: form-data; name="metadata"
        Content-Type: application/json

        {Incoming Notification JSON}

        --FulltextBoundary

        Content-Disposition: form-data; name="content"
        Content-Type: application/zip

        Binary Package

        --FulltextBoundary--

If you are carrying out this request you MUST include the **content.packaging_format** field in the notification metadata and populate it with the appropriate format identifier as per the [Packaging Format](./Packaging.md#packaging) documentation.

### 3. Notification Minimum Metadata + Package request

It is possible to send a request with virtually no JSON metadata, instead relying on metadata embedded in an XML file in the binary Package (e.g. in a JATS XML structure).

To do this, send the bare-minimum JSON notification, with only the format identifier of the [package](./Packaging.md#packaging) included.  

For example:

    POST /notification?api_key=<api_key>
    Header:
        Content-Type: multipart/form-data; boundary=FulltextBoundary
    
    Body:
        --FulltextBoundary

        Content-Disposition: form-data; name="metadata"
        Content-Type: application/json

        {
            "content" : {
                "packaging_format" : "https://pubsrouter.jisc.ac.uk/FilesAndJATS"
            },
        }

        --FulltextBoundary

        Content-Disposition: form-data; name="content"
        Content-Type: application/zip

        Binary Package

        --FulltextBoundary--


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


# API for retrieving notifications

If you are a repository, consuming notifications from PubRouter, you have access to 2 endpoints:

1. The **[notification list feed](./API.md#notification-list-feed-endpoint)** endpoint - which allows you to list all notifications routed to your repository and page through them in date order.
2. The **[notification](./API.md#notification-endpoint)** endpoint - allows retrieval of an individual notification and any binary/packaged content associated with it

Notifications are represented in our native JSON format as an [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) (or a [Provider's Outgoing Notification](./ProviderOutgoingNotification.md#provider-outgoing-notification) if you are the publisher who created it).

Packaged content is available as a zipped file whose contents conform to a supported [Packaging Format](./Packaging.md#packaging).

The following sections describe the HTTP methods, headers, body content and expected responses for each of the above endpoints and content.

### Possible Responses

- In the event of a **malformed HTTP request**, you will receive a 400 (Bad Request) and an error
message in the body:
```
    HTTP 1.1  400 Bad Request
    Content-Type: application/json
    
    {
        "error" : "human readable error message"
    }
```

- On **successful** request, the response will be a 200 OK, with the following body
```
    HTTP 1.1  200 OK
    Content-Type: application/json
    
    {
        "since" : "date from which results start in the form YYYY-MM-DDThh:mm:ssZ",
        "page" : "page number of results",
        "pageSize" : "number of results per page",
        "timestamp" : "timestamp of this request in the form YYYY-MM-DDThh:mm:ssZ",
        "total" : "total number of results at this time",
        "notifications" : [
            "ordered list of 'Outgoing Notification' JSON objects"
        ]
    }
```
Note that the "total" may increase between requests, as new notifications are added to the end of the list.

See the [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) data model for more information.

---
### Notification List Feed Endpoint

This endpoint lists routed notifications in "analysed_date" order (the date PubRouter analysed the content to determine its routing to your repository), oldest first.

You may list the notifications routed to just your repository or, alternatively, all notifications that were routed to any repository.

Note that as notifications are never updated (only created), this sorted list is guaranteed to be complete and include the same notifications each time for the same request (and any extra notifications created in the time period).  This is the reason for sorting by "analysed_date" rather than "created_date", as the rate at which items pass through the analysis may vary.

Allowed parameters for each request are:

* **api_key** - [optional] - May be used for tracking API usage, but no authentication is required for this endpoint.
* **since** - [required] - Timestamp from which to provide notifications, of the form YYYY-MM-DD or YYYY-MM-DDThh:mm:ssZ (in UTC timezone); YYYY-MM-DD is considered equivalent to YYYY-MM-DDT00:00:00Z
* **page** - [optional] - Page number of results to return, defaults to 1.
* **pageSize** - [optional] - Number of results per page to return, defaults to 25, maximum 100.

### 1. Repository routed notifications

This endpoint lists all notifications routed to your repository.  

    GET /routed/<repo_id>[?<parameter list>]

Here, **repo_id** is your PubRouter *Account ID*, which may be obtained from the *Account details* panel at the top of your PubRouter account page.

You will not be able to tell from this endpoint which other repositories have been identified as targets for this notification.

### 2. All routed notifications

This endpoint lists all routed notifications irrespective of the repositories they were routed to (it excludes notifications which were not matched to any repository).

    GET /routed[?<parameter list>]

You will not be able to tell from this endpoint which repositories have been identified as targets for this notification.

---
### Individual Notification Endpoint

This endpoint will return to you the JSON record for an individual notification.

The JSON metadata associated with a notification is publicly accessible, so anyone can access this endpoint.

    GET /notification/<notification_id>

Here **notification_id** is the system's identifier for an individual notification.  You may get this identifier from, for example, the **[Notification List Feed](./API.md#notification-list-feed-endpoint)**.

If the notification does not exist, you will receive a 404 (Not Found), and no response body.

If the you are not authenticated as the original publisher of the notification, and the notification has not yet been routed, you will also receive a 404 (Not Found) and html error page in the body.

If the notification is found and has been routed, you will receive a 200 (OK) and the following response body:

    HTTP 1.1  200 OK
    Content-Type: application/json
    
    {Outgoing Notification JSON}

See the [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) data model for more info.

Some notifications may contain one or more **links** elements.  In this event, this means that there is binary content associated with the notification available for download.  Each of the links could be one of two kinds:

1. Packaged binary content held by PubRouter on behalf of the publisher (see the next section)
2. A proxy-URL (proxying through the Router) for public content hosted on the web by the publisher

In either case you can issue a GET request on the URL to receive the content.

In order to tell the difference between (1) and (2), compare the following two links:

    "links" : [
        {
            "type" : "package",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v2/notification/123456789/content",
            "packaging" : "https://pubsrouter.jisc.ac.uk/FilesAndJATS"
        },
        {
            "type" : "fulltext",
            "format" : "application/pdf",
            "url" : "https://pubrouter.jisc.ac.uk/api/v2/notification/123456789/content/publisher.pdf",
        }
    ]

The first link has type "package" and also has an element **packaging** which tells you this is of the format "https://pubsrouter.jisc.ac.uk/FilesAndJATS".

The second link does not contain a **packaging** element at all, and does not have "package" as its type.

This means the first link is a link to package held by PubRouter, and the second is a proxy for a URL hosted by the publisher.

#### Packaged Content

Some notifications may have binary content associated with them.  If this is the case, you will see one or more **links** elements
appearing in the [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) JSON that you retrieve via either the **Notification List Feed** or the **Individual Notification**.

Router stores full-text content for a temporary period (currently 90 days, subject to review) from the date of receipt from publisher and so it must be retrieved by a repository within this timescale.

You need to have a PubRouter account (either Repository or Publisher) to access this endpoint.

Notifications with binary content will contain contain a links section like:

    "links" : [
        {
            "type" : "package",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v2/notification/123456789/content",
            "packaging" : "https://pubsrouter.jisc.ac.uk/FilesAndJATS"
        },
        {
            "type" : "package",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v2/notification/123456789/content/SimpleZip",
            "packaging" : "http://purl.org/net/sword/package/SimpleZip"
        }
    ]

In this case there are 2 packages available (both representing the same content).  One is in the "FilesAndJATS" format that the publisher originally provided to PubRouter, and the other is in the "SimpleZip" format to which PubRouter has converted the incoming package.

See the documentation on [Packaging Formats](./Packaging.md#packaging) to understand what each of the formats looks like.

You may then choose one of these links to download to receive all of the content (e.g. publisher's PDF, JATS XML, additional image files) as a single zip file.  To request it, you will also need to provide your API key (shown on your PubRouter account page):

    GET <package url>?api_key=<api_key>

Authentication failure will result in a 401 (Unauthorised), and no response body.  Authentication failure can happen for the following reasons:
* api_key is invalid
* You are a Publisher and you were not the original creator of this notification
* You are a Repository and this notification has not yet been routed.

If the notification content is not found, you will receive a 404 (Not Found) and no response body.  This will happen if you try to access content for a notification that was received more than 90 days ago.

If the notification content is found and authentication succeeds you will receive a 200 (OK) and the binary content:

    HTTP 1.1  200 OK
    Content-Type: application/zip
    
    Binary Package

Note that a successful access by a Repository account user will log a successful delivery of content notification into PubRouter (used for reporting on PubRouter's ability to support REF compliance).
