# API for sending notifications (Publishers)

The current version of the API is v3, and it can be accessed at

    https://pubrouter.jisc.ac.uk/api/v3

All URL paths provided in this document will extend from this base url.

Also see the **[API Swagger documentation](https://jisc-services.github.io/Public-Documentation/)** (which also enables you to try out the API).

### Validation and Live endpoints

If you are a publisher (also referred to here as a "provider") providing content to Publications Router, you have access to 2 endpoints:

1. **Validation endpoint** - for use during initial set-up and testing of your API client, to validate the content your send to Publications Router

2. **Notification endpoint** - for "live" use, sending real notifications to Publications Router.


### Sending Metadata only or Metadata plus full-text article

You can create content in 2 ways in Publications Router:

1. As a **metadata-only notification** - which allows you to provide publication information in our native JSON format as an [Incoming Notification](./IncomingNotification.md#incoming-notification).

   There are 2 endpoints for this, one submits a single notification, the other a list of notifications.

2. As a **metadata + binary package notification** - which allows you to give us a multi-part request containing the publication information which complies with our native JSON format as an [Incoming Notification](./IncomingNotification.md#incoming-notification) plus a zipped binary package containing content in a supported [Packaging Format](./Packaging.md#packaging).

   These can be sent using only the single notification endpoint (not via the list of notifications endpoint).

The following sections describe the HTTP methods, headers, body content and expected responses for each of the above endpoints and content.

### Important information about notification metadata

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

If you have publicly hosted content (e.g. splash pages, full-text web pages, or PDFs) that you want to share with Publications Router, so that repositories can download the content directly, you may provide these in a **links** element.  For example:

```JSON

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
```

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Validation POST endpoints

The Validation API allows you to test that your data feed to the system will be successful.  You must have a **Publisher account** to access either of these endpoints:
* **POST /validate** - to validate submission of a single notification (which may be metadata only or metadata plus file(s))
* **POST /validate/list** - to validate submission of a list of metadata only notifications

### Possible HTTP responses to `POST /validate` or `POST /validate/list`

Either of the validation endpoints will return one of these responses.

- On **authentication failure** (e.g. invalid api_key) the API will respond with a **401 (Unauthorised)**:
```JSON
    HTTP 1.1  401 Unauthorized
```
&nbsp;
- On **validation failure** the system will respond with a **400 (Bad request)** and the JSON body below. See the [Validation](./Validation.md) page for help with error/issue messages.
```JSON
    HTTP 1.1  400 Bad Request
    Content-Type: application/json
    {
       "status" : "error",
       "summary" : "Validation failed with N errors and M issues",  (where N and M are integer counts)
       "errors" : ["List of human readable error messages"],
       "issues" :  ["List of human readable warning messages"]
    }
```
&nbsp;
- If Account **is not permitted** to use the endpoint (e.g. has wrong user role, or is turned off), the API will respond with a **403 (Forbidden)** and a JSON error body:
```JSON
    HTTP 1.1  403 Forbidden
    Content-Type: application/json
    {
        "error" : "Your account does not have permission to perform this function."
    }
```
&nbsp;

- On **validation success** the system will respond with **200 (Success)** and the response body shown below. Note that there will never be any errors, but there may be issues. 
```JSON
    HTTP 1.1  200 OK
    Content-Type: application/json
    {
       "errors" : [],
       "issues" : ["List of human readable warning messages"],
       "status" : "ok",
       "summary" : "Validated OK"
    }
```

NOTE: in the following sections reference to "{Incoming notification JSON object}" means the Incoming Notification [JSON data structure](./IncomingNotification.md#json-data-structure).

## Single notification validation - `POST /validate`



### 1. Validate Metadata-only request

If you are sending only the notification JSON, the request must take the form:

    POST /validate?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        {Incoming notification JSON object}

### 2. Validate Metadata + Package request

If you are sending binary content as well as the metadata, then a multi-part request must be formed using [RFC 2387](https://www.ietf.org/rfc/rfc2387.txt):

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

If you are carrying out this request you MUST include the **content.packaging_format** field in the notification metadata and populate it with the appropriate format identifier as per the [Packaging Format](./Packaging.md#packaging) documentation.

### 3. Validate minimum Metadata + Package request

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
            "content" : {
                "packaging_format": "https://pubrouter.jisc.ac.uk/FilesAndJATS"
            }
        }
        
        --------------------------586e648803c83e39
        Content-Disposition: form-data; name="content"; filename="content.zip"
        Content-Type: application/zip

        {{Zip Content}}       
        
        --------------------------586e648803c83e39---
    

## List of Metadata-only notifications validation - `POST /validate/list`

If you are sending a list of notifications, the request must take the form shown below.  Note that only metadata can be sent in this way (binary content is not supported):

    POST /validate/list?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        # List of Incoming Notification JSON
        [{"notification" : {Incoming notification JSON object}, "id": 1},
         {"notification" : {Incoming notification JSON object}, "id": 2},
         {"notification" : {Incoming notification JSON object}, "id": 3} ... ]

NOTE: Make sure that an ID is sent for each Incoming notification as these will be returned in the success or error list.  You can have any value for the ID (we have shown integers, but you may use something else, to be useful they should be unique within the list you are submitting).  These IDs are not stored by Publications Router but simply used in reporting the success/failure of the submissions in the response to the API call HTTP request.
	
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Notification endpoints (for sending notifications to Publications Router)

The Notification API endpoints:
* **POST /notification** - to create a single notification (metadata only or metadata plus files)
* **POST /notification/list** - to create multiple metadata only notifications.

These take the same request header and body as the Validation API endpoints, so that you can develop against the Validation API and then switch seamlessly over to live notifications. However, there will be a difference in the response body that is received.

Again, you must have "Publisher account" to access these endpoints.

The system will not attempt to aggressively validate the request, but the request must still be well-formed in order to succeed, so you may still receive a (single) validation error.

On a successful call to this endpoint, your notification will be accepted into Publications Router where it will be queued for subsequent processing and routing to matched repositories.

## Single notification submission - `POST /notification`

### Responses to `POST /notification` endpoint

Note some of these are different from the Validation endpoint.
&nbsp;
* **201 - Success**: if the request is successful then a HTTP **201 (Created)** code is provided with the JSON response body shown:

```JSON
    HTTP 1.1  201 Created
    Content-Type: application/json
    {
        "id" : "<unique identifier for the notification>",
        "location" : "<url path for api endpoint for newly created notification>"
    }
```
&nbsp;
* **401 - authentication failure**: for invalid api_key, incorrect user role, or other problems authenticating the system will respond with HTTP **401 (Unauthorised)** and nothing else.
```JSON
    HTTP 1.1  401 Unauthorized
```
&nbsp;
* **400 - malformed request**: where the request is malformed in some way the system will return a HTTP **400 (Bad Request)** and the JSON response body shown:

```JSON
    HTTP 1.1  400 Bad Request
    Content-Type: application/json
    {
        "error" : "human readable error message"
    }
```
&nbsp;
* **403 - forbidden**: Where an authenticated user as an invalid role (for example is a publisher) or account is turned off:

```JSON
    HTTP 1.1  403 Forbidden
    Content-Type: application/json
    {
        "error" : "Only an admin or publisher user can access this endpoint."
    }
```
&nbsp;
&nbsp;

### 1. Notification Metadata-only request

If you are sending only the notification JSON, the request must take the form:

    POST /notification?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        {Incoming Notification JSON}
	
&nbsp;
&nbsp;

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
	
&nbsp;
&nbsp;

### 3. Notification minimum Metadata + Package request

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
	
&nbsp;
&nbsp;

## Multiple Metadata-only notifications submission - `POST /notification/list`

### Responses to `POST /notification/list`

Note some of these are different from the Validation endpoint.
&nbsp;
* **202 - Partial success**: when some notifications in the list succeed and some fail then an HTTP **202 (Accepted)** code is provided with the JSON response body shown:

```JSON
    HTTP 1.1  202 Accepted
    Content-Type: application/json
    {
        "successful" : <number of successfully processed notifications>,
        "total" : <the number of items received in the list>,
        "created_ids" : [ <list of Publications Router notification IDs of created notifications> ],
        "success_ids" : [ <list of submitted IDs of successfully processed notifications> ],
        "fail_ids" : [ <list of submitted IDs of notifications that could not be processed> ],
        "last_error" : <error message describing the error which caused the last failed notification to fail>
    }
```
&nbsp;
* **201 - Success**: when the entire list is successfully processed then an HTTP **201 (Created)** code is provided with the JSON response body shown:  

```JSON
    HTTP 1.1  201 Created
    Content-Type: application/json
    {
        "successful" : <number of successfully processed notifications>,
        "total" : <the number of items received in the list>,
        "created_ids" : [ <list of Publications Router notification IDs of created notifications> ],
        "success_ids" : [ <list of IDs of successfully processed notifications> ],
        "fail_ids" : [ <list of IDs of notifications that could not be processed> ],
        "last_error" : "<Last error message>"
    }
```
&nbsp;
* **401 - authentication failure**: for invalid api_key, incorrect user role, or other problems authenticating the system will respond with HTTP **401 (Unauthorised)** and nothing else.
```JSON
    HTTP 1.1  401 Unauthorized
```
&nbsp;
* **400 - malformed request**: where the request is malformed in some way the system will return an HTTP **400 (Bad Request)** and the JSON response body shown:

```JSON
    HTTP 1.1  400 Bad Request
    Content-Type: application/json
    {
        "error" : "human readable error message"
    }
```
&nbsp;
* **403 - forbidden**: Where an authenticated user as an invalid role (for example is a publisher) or account is turned off:

```JSON
    HTTP 1.1  403 Forbidden
    Content-Type: application/json
    {
        "error" : "Only an admin or publisher user can access this endpoint."
    }
```
&nbsp;
&nbsp;

### Notification List (Metadata-only) request

If you are sending a list of notifications, the request must take the form:

    POST /notification/list?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        # List of Incoming Notification JSON
        [{"notification": {Incoming notification JSON object}, "id": 1},
         {"notification": {Incoming notification JSON object}, "id": 2},
         {"notification": {Incoming notification JSON object}, "id": 3} ... ]

NOTE: Make sure that an ID is sent for each Incoming notification as these will be returned in the success or error list.  You can have any value for the ID (we have shown integers, but you may use something else, to be useful they should be unique within the list you are submitting).  These IDs are not stored by Publications Router but simply used in reporting the success/failure of the submissions in the response to the API call HTTP request.
	
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Sending multipart requests with Curl

The multipart requests are quite complex, but they can be easily formed using curl's -F flag.

Note in the following examples you would need to replace `<my_api_key>` by your actual API key value, and files named after the `@` symbol would need to exist (e.g. for `@metadata.json`a file named "metadata.json" containing JSON metadata, would need to exist in the current directory).

### Validate endpoint

```bash
# Validate endpoint
curl -XPOST -H 'Content-Type: multipart/related' \
-F 'metadata=@metadata.json;type=application/json;filename="metadata.json"' \ 
-F 'content=@myzip.zip;type=application/zip;filename="content.zip"' \
https://pubrouter.jisc.ac.uk/api/v3/validate?api_key=<my_api_key>

```

### Notification endpoint

```bash

# Notification endpoint
curl -XPOST -H 'Content-Type: multipart/related' \
-F 'metadata=@metadata.json;type=application/json;filename="metadata.json"' \
-F 'content=@myzip.zip;type=application/zip;filename="content.zip"' \
https://pubrouter.jisc.ac.uk/api/v3/notification?api_key=<my_api_key>
```
