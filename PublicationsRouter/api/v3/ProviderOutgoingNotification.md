# Provider Outgoing Notification

The PubRouter ProviderOutgoingNotification is the structure available to publishers of meta-data output by PubRouter for notifications that have been matched (designated for transmission) to at least one Repository.

It is identical to the [OutgoingNotification](./OutgoingNotification.md) except that the:
 * **provider** object may contain the following additional elements: *id*, *route* and *ref* (i.e. provider.id, provider.ref and provider.route).
 * **links** array objects may contain an additional element: *proxy* (i.e. links[x].proxy).
 
## Partial JSON Data Structure

The full ProviderOutgoingNotification JSON structure is identical to that shown for the [OutgoingNotification](./OutgoingNotification.md#json-data-structure) except for two objects which differ (having additional elements) as shown here.

IMPORTANT: the structure returned by an API request will only have elements for which data exists; where no data is available the element will be omitted from the structure. 

```json
{
...,
	"provider": {
		"id" : "<user account id of the provider>",
		"agent": "<string defining the software/process which put the content here, provided by provider>",
		"ref": "<provider's globally unique reference for this research object>",
		"route" : "<method by which notification was received: native api, sword, ftp>"
	},
...,
	"links": [
	    {
		"type": "<link type: splash|fulltext>",
		"format": "<text/html|application/pdf|application/xml|application/zip|...>",
		"url": "<provider's splash, fulltext or machine readable page>",
	    "packaging": "<package format identifier string>",
		"access":"<permitted access level, one of: public|router|special >",
		"proxy": "<provider's secret URL to article content>"
             }
	], 
...
}
```

## Field Definitions

Each of the fields in the partial JSON structure above is defined in the table below in the order that they appear (dot notation is used to qualify them).

NOTE that fields which will always be populated are indicated with an asterisk (*) in the Field column, all other fields are optional.

| Field | Description | Datatype | Format |
| ----- | ----------- | -------- | ------ |
| ... | | |
| provider * | Object describing the source of this notification | object | | 
| provider.agent * | The notification source - either a Publisher or a consolidator (like PubMed) that Router harvests from | unicode | text |
| provider.id * | User account ID of provider | unicode | free text |  |
| provider.ref | Publisher's own identifier for the notification | unicode | free text |  |
| provider.route * | Method by which notification was received: native api, sword, ftp | unicode | free text |  |
| ... | | |
| links.type | Keyword for type of resource (e.g. "package" - indicates a zip file, "unpackaged" - indicates an individual file, "fulltext" - article file, "splash" - splash screen) | unicode |  |
| links.format | The mimetype of the resource available at the URL (e.g. text/html) | unicode |  |
| links.url | URL to the associated resource. The links.access value (see below) determines how long the resource will be available at the URL | unicode | URL |
| links.packaging | Package format identifier for the resource available at the URL | unicode |  |
| links.access | URL access type, one of: "public" - indicates the content is available from public URL for at least 3 months from *created_date*; "router" and "special" - content is in temporary PubRouter store, kept for 3 months from *created_date"; "special" indicates unpackaged content that will duplicate other content contained in a package  | unicode |  |
| links.proxy | URL to the associated resource on provider's server, where it should be available for at least 3 months. PubRouter keeps this value secret. | unicode | URL |
| ... | | |

