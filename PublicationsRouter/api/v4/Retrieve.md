# API for retrieving notifications

The current version of the API is v4, and is accessed at:

    https://pubrouter.jisc.ac.uk/api/v4

**All URL paths provided in this document extend from this base url.**

A repository or CRIS, consuming notifications from Publications Router, has access to 3 endpoints:

1. **[notification list feed](#notification-list-feed-endpoint)** - which lists all notifications routed to a repository in date order, with pagination.
2. **[notification](#individual-notification-endpoint)** - retrieves an individual notification.
2. **[notification content](#packaged-content-endpoint)** - retrieves packaged zip content associated with an individual notification.

**A valid API-Key is required to access all endpoints**

Notifications are represented in Router's native JSON format as an [Outgoing Notification](./OutgoingNotification.md#outgoing-notification).

Packaged content (e.g. containing article PDFs) is available as a zipped file in one of the supported [Packaging Formats](./Packaging.md#packaging).

The following sections describe the HTTP methods, headers, body content and expected responses for each of the above endpoints and content.

Also see the **[API Swagger documentation](https://jisc-services.github.io/Public-Documentation/)** (which also enables you to try out the API).

### Possible API Responses

* **401 - authentication failure**: For invalid api_key or other problems authenticating.

```JSON
    HTTP 1.1  401 Unauthorized
```

* **400 - Bad Request**: In the event of a malformed HTTP request.
```JSON
    HTTP 1.1  400 Bad Request
    Content-Type: application/json
    {
        "error" : "human readable error message"
    }
```

- **200 - OK**: for successful requests.
```JSON
    HTTP 1.1  200 OK
    Content-Type: application/json
    {
        Body depends on endpoint
    }
```

---
## Notification List Feed Endpoint

This endpoint lists routed notifications in "analysis_date" order (the date Publications Router analysed the content to determine its routing to your repository), oldest first.

Note that as notifications are never updated (only created and, after 3 months, deleted), this sorted list is guaranteed to be complete and include the same notifications each time for the same request (and any extra notifications created in the time period).  This is the reason for sorting by "analysis_date" rather than "created_date", as the rate at which items pass through the analysis may vary.

    GET /routed/<repo_id>?<parameter list>

Here, **repo_id** is the Publications Router *Account ID*, which may be obtained from the *Account details* panel at the top of your Publications Router account page.

### Parameter list

Required parameters (* Only one of `since` or `since_id` is needed):

* **api_key** - May be used for tracking API usage, but no authentication is required for this endpoint
* **since_id*** - ID of the last notification you retrieved - API will return all notifications with an ID value greater than this 
* **since***- Time-stamp from which to provide notifications, of the form YYYY-MM-DD or YYYY-MM-DDThh:mm:ssZ (in UTC time zone); YYYY-MM-DD is considered equivalent to YYYY-MM-DDT00:00:00Z.  The response will include all records with an `analysis_date` greater than or equal to the `since` date


Optional parameters:
* **page** - Page number of results to return, defaults to 1. Where the number of results exceeds the `pageSize` it will be necessary to make successive requests, incrementing the `page` value each time
* **pageSize** - Number of results per page to return, defaults to 25, maximum 100.

### Successful Response
- **200 - OK**: for successful requests.
```JSON
    HTTP 1.1  200 OK
    Content-Type: application/json
    {
        "since" : "Since date from which results start in the form YYYY-MM-DDThh:mm:ssZ (if provided in API request)",
        "since_id" : "Since ID (if provided in API request)",
        "page" : "page number of results",
        "pageSize" : "number of results per page",
        "timestamp" : "timestamp of this request in the form YYYY-MM-DDThh:mm:ssZ",
        "total" : "total number of results at this time",
        "notifications" : [
            List of 'Outgoing Notification' JSON objects ordered by ascending ID value 
        ]
    }
```
NOTES:
* Only one of `since` and `since_id` will be set - depending on which was provided as a parameter
* The `total` value may increase between requests, as new notifications are added to the end of the list
* The ordering of the JSON elements may vary (for example notifications may appear first)
* See the [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) data model for more information on notification data structure.

---
## Individual Notification Endpoint

This endpoint will return the JSON record for an individual notification.

    GET /notification/<notification_id>?api_key=...

Here **notification_id** is the system's identifier for an individual notification.  You may get this identifier from, for example, the **[Notification List Feed](#notification-list-feed-endpoint)**.

Parameter **api_key** is required.

If the api_key is not supplied or is not valid you will receive a 401 (Unauthorized) response.

If the notification does not exist, you will receive a 404 (Not Found) with a JSON error document response body.

If the notification is found and has been routed, you will receive a 200 (OK) and the following response body:

- **200 - OK**: for successful requests.
```JSON
    HTTP 1.1  200 OK
    Content-Type: application/json
    { 
        ...Outgoing Notification JSON... 
    }
```
See the [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) data model for details.

## Retrieving notification content

Notifications may have content, e.g. article PDF, associated with them.  This may either exist on Publications Router (where it has been supplied by a publisher) or at an external location, such as a publisher's website.

This is indicated by the presence of the **links** array, containing a JSON object for each item available for download. 
 
The link will be to:
* either packaged binary content (sometimes also unpacked content) stored (for 90 days) by Publications Router on behalf of the publisher,
* or a link pointing at an external resource (likely the full text). 

You can issue a GET request on the URL contained within a link element to receive the content.

The **links array** may contain objects like those shown below.  It is IMPORTANT to analyse the links, by examining the values of `type`, `access` and `format`, to select the optimum `url` to use to retrieve the content you are interested in.  

NOTE that URLs to Publications Router will require a valid API key to be provided in order to successfully retrieve the content. 

#### Example links array 
A description of each different type of link object is provided below the data structure.

    "links" : [
        {
            "type" : "package",
            "access": "router",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v4/notification/123456789/content",
            "packaging" : "https://pubrouter.jisc.ac.uk/FilesAndJATS"
        },
        {
            "type": "fulltext",
            "access": "public",
            "format": "application/pdf",
            "url": "https://some_publisher_site.com/some_file_name.pdf"
        },
        {
            "url": "https://pubrouter.jisc.ac.uk/api/v4/notification/123456789/content/eprints-rioxx/article.pdf",
            "format": "application/pdf",
            "type": "unpackaged",
            "access": "special"
        },
        {
            "url": "https://pubrouter.jisc.ac.uk/api/v4/notification/123456789/content/eprints-rioxx/non-pdf-files.zip",
            "format": "application/zip",
            "type": "unpackaged",
            "access": "special"
        }
    ]
    
The first link is a packaged binary link, and includes the packaging type used to package the binary. Packaging types are explained in further detail in the next section.  The `"access": "router"` and `"type": "package"` elements indicate that the content is stored by Publications Router (as the URL attests) - this content may only be downloaded if a valid API key is provided.

The second link is to an external site, this will not have a packaging type. The `type` element describes the type of content (likely _fulltext_), and `format` describes the content's format (likely _application/pdf_, or _text/html_).  The `"access": "public"` element indicates that the content is located on a publicly accessible third party site.

The last two links are also to content stored by Publications Router. In this case `"access": "special"` and `"type": "unpackaged"` indicate that the original zip package has been unpacked to extract all enclosed PDF files; with everything else repackaged into a file named **non-pdf-files.zip**.  All content of this type will also require a valid API key to be provided.  NOTE:  such links will only exist where a notification has been matched to a repository that requires access to unpacked files, such as Eprints with the RIOXXplus plugin.

### Packaged Content Endpoint

For notifications which have binary content stored by Publications Router this can be downloaded using one of these endpoints:
* `GET /notification/<notification-id>/content`
* `GET /notification/<notification-id>/content/<filename>`.

IMPORTANT: Router stores full-text content for a temporary period (currently 90 days, subject to review) from the date of receipt from publisher and so it must be retrieved by a repository within this timescale.

Notifications with binary content will contain contain a links section like:

    "links" : [
        {
            "type" : "package",
            "access": "router",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v4/notification/123456789/content",
            "packaging" : "https://pubrouter.jisc.ac.uk/FilesAndJATS"
        },
        {
            "type" : "package",
            "access": "router",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v4/notification/123456789/content/SimpleZip.zip",
            "packaging" : "http://purl.org/net/sword/package/SimpleZip"
        }
    ]

In this case there are 2 packages available (both representing the same content).  One is in the "FilesAndJATS" format that the publisher originally provided to Publications Router, and the other is in the "SimpleZip" format to which Publications Router has converted the incoming package.

See the documentation on [Packaging Formats](./Packaging.md#packaging) to understand what each of the formats looks like.

You may then choose one of these links to download to receive all of the content (e.g. publisher's PDF, JATS XML, additional image files) as a single zip file.  To request it, you will also need to provide your API key (shown on your Publications Router account page):

    GET <package url>?api_key=<API-key>

&nbsp;
#### Possible HTTP responses to `GET /notification/<notification-id>/content` endpoint
Authorisation failure will result in either a 401 (Unauthorised) or 403 (Forbidden) return code:
&nbsp;
* **401 - authentication failure**: For invalid api_key or other problems authenticating.

```JSON
    HTTP 1.1  401 Unauthorized
```
&nbsp;
* **403 - forbidden**: Where:
  * you are a Publisher and you were not the original creator of this notification
  * you are a Repository and this notification has not yet been routed or does not belong to you.

```JSON
    HTTP 1.1  403 Forbidden
    Content-Type: application/json
    {
        "error" : "Only an admin or repository user can access this endpoint."
    }
```
&nbsp;
* **404 - Not Found**: Where the content was not found - This will happen if you try to access content for a notification that was received more than 90 days ago.
```JSON
    HTTP 1.1  404 Not Found
    Content-Type: application/json
    {
        "error" : "...Error message..."
    }
```
&nbsp;
* **200 - OK**: If the notification content is found and authentication succeeds you will receive a 200 (OK) and the binary content:
```JSON
    HTTP 1.1  200 OK
    Content-Type: application/zip

    Binary Package
```
Note that a successful retrieval of content by a repository account will be recorded by Publications Router (used for reporting on Publications Router's ability to support REF compliance).
