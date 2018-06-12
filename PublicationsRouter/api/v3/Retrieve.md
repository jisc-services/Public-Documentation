# API for retrieving notifications

The current version of the API is v3, and it can be accessed at

    https://pubrouter.jisc.ac.uk/api/v3

All URL paths provided in this document will extend from this base url.

If you are a repository, consuming notifications from PubRouter, you have access to 2 endpoints:

1. The **[notification list feed](#notification-list-feed-endpoint)** endpoint - which allows you to list all notifications routed to your repository and page through them in date order.
2. The **[notification](#individual-notification-endpoint)** endpoint - allows retrieval of an individual notification and any binary/packaged content associated with it

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
NOTES:
* the "total" may increase between requests, as new notifications are added to the end of the list
* the ordering of the JSON elements may vary (for example notifications may appear first).

See the [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) data model for more information.

---
## Notification List Feed Endpoint

This endpoint lists routed notifications in "analysis_date" order (the date PubRouter analysed the content to determine its routing to your repository), oldest first.

You may list the notifications routed to just your repository or, alternatively, all notifications that were routed to any repository.

Note that as notifications are never updated (only created), this sorted list is guaranteed to be complete and include the same notifications each time for the same request (and any extra notifications created in the time period).  This is the reason for sorting by "analysis_date" rather than "created_date", as the rate at which items pass through the analysis may vary.

### Request parameters

The REST call must include the `since` parameter and may include additional parameters:

* **since** - [required] - Analysis date Time-stamp from which to provide notifications, of the form YYYY-MM-DD or YYYY-MM-DDThh:mm:ssZ (in UTC time zone); YYYY-MM-DD is considered equivalent to YYYY-MM-DDT00:00:00Z.  The response will include all records with an `analysis_date` greater than or equal to the `since` date
* **api_key** - [optional] - May be used for tracking API usage, but no authentication is required for this endpoint
* **page** - [optional] - Page number of results to return, defaults to 1. Where the number of results exceeds the `pageSize` it will be necessary to make successive requests, incrementing the `page` value each time
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
## Individual Notification Endpoint

This endpoint will return to you the JSON record for an individual notification.

The JSON metadata associated with a notification is publicly accessible, so anyone can access this endpoint.

    GET /notification/<notification_id>

Here **notification_id** is the system's identifier for an individual notification.  You may get this identifier from, for example, the **[Notification List Feed](#notification-list-feed-endpoint)**.

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
            "url" : "https://pubrouter.jisc.ac.uk/api/v3/notification/123456789/content",
            "packaging" : "https://pubrouter.jisc.ac.uk/FilesAndJATS"
        },
        {
            "type" : "fulltext",
            "format" : "application/pdf",
            "url" : "https://pubrouter.jisc.ac.uk/api/v3/notification/123456789/content/publisher.pdf",
        }
    ]

The first link has type "package" and also has an element **packaging** which tells you this is of the format "https://pubrouter.jisc.ac.uk/FilesAndJATS".

The second link does not contain a **packaging** element at all, and does not have "package" as its type.

This means the first link is a link to package held by PubRouter, and the second is a proxy for a URL hosted by the publisher.

### Packaged Content

Some notifications may have binary content associated with them.  If this is the case, you will see one or more **links** elements
appearing in the [Outgoing Notification](./OutgoingNotification.md#outgoing-notification) JSON that you retrieve via either the **Notification List Feed** or the **Individual Notification**.

Router stores full-text content for a temporary period (currently 90 days, subject to review) from the date of receipt from publisher and so it must be retrieved by a repository within this timescale.

You need to have a PubRouter account (either Repository or Publisher) to access this endpoint.

Notifications with binary content will contain contain a links section like:

    "links" : [
        {
            "type" : "package",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v3/notification/123456789/content",
            "packaging" : "https://pubrouter.jisc.ac.uk/FilesAndJATS"
        },
        {
            "type" : "package",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v3/notification/123456789/content/SimpleZip",
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
