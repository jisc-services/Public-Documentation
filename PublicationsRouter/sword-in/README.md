# Publications Router SWORDv2 Deposit Endpoint

Publications Router provides a SWORD2 interface, known as SWORD-IN, that enables publishers to deposit article metadata and content into Publications Router using a SWORD client (as an alternative to the Publications Router REST API or FTP).

This page tells you all you need to know to get connected to Publications Router using this approach.

The interface is built on the SWORDv2 specification, which you can [read about here](http://swordapp.github.io/SWORDv2-Profile/SWORDProfile.html), but with specific limitations - see following section.

The Publications Router SWORD2 service endpoint is:

    https://pubrouter.jisc.ac.uk/sword2


## Limitations of SWORD-IN - Publications Router SWORDv2 implementation

Publications Router is a messaging system that routes and conveys articles (including metadata, PDFs and related media) received from publishers to institutional repositories.  It is NOT a persistent store of articles and hence is not a fully-fledged repository; accorsdingly it's implementation of SWORDv2 reflects this. 

The Router SWORD-IN interface has the following features and limitations.

### SWORD-IN Features

1. Allows deposit of a single Zip file package containing:
   * JATS XML article metadata (this is the ONLY metadata format that Jisc supported)
   * Article PDFs
   * Related media files e.g. illustration JPEGs, spreadsheet data etc.
   

2. Allows the separate submission of individual files (providing the In-Progress flag is True and the appropriate EM-IRI URL from the initial Deposit receipt is used)
   * JATS XML article metadata (this is the ONLY metadata format that Jisc supported)
   * Article PDFs
   * Related media files e.g. illustration JPEGs, spreadsheet data etc.


3. While a submission is In-Progress it is possible to add or delete files


4. Once a deposit is completed (i.e. a request is made with In-Progress flag set False) then Router immediately **ingests** the deposited article package or separate files into its workstream as a new *article notification*.  After this point the following are NOT possible:
   * Modify the submission in any way


### SWORD-IN Limitations

1. A submission may not contain a mix of Zip and non-zip files
2. A submission can contain 1 zip file at most
3. A previously deposited submission may be retrieved (but not otherwise modified) for a period of 5 days (subject to change) while it remains in temporary storage on the SWORD-IN server.

## Getting Started: retrieving the Service Document

The Service Document tells you all you need to know about the SWORDv2 endpoint that you're going to be interacting with.

When you start working with the endpoint, you'll probably want to retrieve it to take a look at the server's capabilities,
but you won't need it much after that.

The Service Document lists the collections that you can send your notifications to, the mimetypes and the packaging formats
that are supported.

You will have access to a single collection, with your Publications Router Account ID as its name.

You can get the URL for your collection by retrieving the service document, which you can do like this:

    GET /

This will require you to provide your Publications Router credentials (email and password) via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i --user <my_account_id>:<my_api_key> https://pubrouter.jisc.ac.uk/sword2/

## Deposit an article as a zip package

If you want to send an article (*notification*) as a package, the zip should be in the https://pubrouter.jisc.ac.uk/FilesAndJATS format listed [here](../api/v4/Packaging.md#httpspubrouterjiscacukfilesandjats).

Get the URL of the collection from the Service Document (see above), and then do the following

    POST /collections/<YOUR_ACCOUNT_ID>
    Content-Disposition: filename=filename.zip
    Content-Type: application/zip
    
    [binary content]

This will require you to provide your Publications Router credentials (email and password) via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i --data-binary "@article.zip" -H "Content-Disposition: attachment; filename=article.zip" -H "Content-Type: application/zip" --user <my_account_id>:<my_api_key> https://pubrouter.jisc.ac.uk/sword2/collections/<MY_ACCOUNT_ID>

In response to this you will receive either:

* a SWORDv2 error document detailing any problems with the request, or 
* a successful 201 (Created) response, which includes an HTTP "Location" header giving the "Edit-IRI", together with an XML *Deposit Receipt*
document, which contains various URLs to access parts of your created notification.

## Deposit metadata and files separately

You can also deposit JATS XML metadata and files separately using the Continued Deposit functionality of SWORD2 (i.e. using In-Progress flag).

Get the URL of the collection from the Service Document the same in the previous section. 

Instead of submitting a package, you submit a files with the In-Progress header value set to *true* to the Collection URL (.../collections/<YOUR_ACCOUNT_ID>) with your credentials. An example of this, using curl is below:

    curl -i --data-binary "@jats.xml" -H "Content-Disposition: attachment; filename=jats.xml" -H "Content-Type: application/xml" -H "In-Progress: true" --user <my_account_id>:<my_api_key> https://pubrouter.jisc.ac.uk/sword2/collections/<MY_ACCOUNT_ID>

After this, you can continue your deposit by using the "Edit-Media" IRI as per the SWORD2 spec, which will be listed in the deposit receipt of the previous request. Just remember to set the In-Progress header to false (or not set it at all) when you have finished depositing files, or it will never be processed:

    curl -i --data-binary "@article.pdf" -H "Content-Disposition: attachment; filename=article.pdf" -H "Content-Type: application/xml" --user <my_account_id>:<my_api_key> https://pubrouter.jisc.ac.uk/sword2/collections/<MY_ACCOUNT_ID>/<NOTIFICATION_ID>/media

You can repeat this for as many files as you like, but note that while any number of XML files may be submitted, only the first that is recognised as JATS XML will be processed as meta-data and used to create the Router *notification*; the remainder will be treated as supplementary article files.

## Retrieving details of previous submissions

Once you've completed the deposit of an article, Router ingests it as a *notification*, after which your options are limited:

1. You can retrieve a summary of the last 50 submissions (NB. the number of submissions returned is a configurable value & is subject to change)
2. You can retrieve a summary of any submission for which the entry ID is known (using the Edit-IRI)
3. For the limited time (currently 5 days, but subject to change) that the deposit remains in the SWORD-IN server temporary store you can retrieve the submitted package

When you deposit the notification for the first time, you will receive back a URL in the Location header of the response.
This contains the "Edit-IRI" of the notification, and as long as you still have this, you can request information about
your notification.

### Retrieving the original deposit receipt

This is obtained by making a request to the Edit-IRI directly, which will return the original deposit receipt:

    GET Edit-IRI
    
This will require you to provide your email and password via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i --user <my_account_id>:<my_api_key> https://pubrouter.jisc.ac.uk/sword2/collections/<MY_ACCOUNT_ID>/<NOTIFICATION_ID>

This will give back an XML deposit receipt.

### Retrieving the binary content

To get the binary content (which is only available for a limited time - see above), you need the "Edit Media IRI" from the deposit receipt.  If you don't have the deposit
receipt, see the section above on how to get it.

In the deposit receipt you'll see a section that looks like this:

    <link rel="edit-media" href="http://pubrouter.jisc.ac.uk/sword2/collections/<MY_ACCOUNT_ID>/<NOTIFICATION_ID>/media"/>

This is the "Edit Media IRI" (EM-IRI), and you can do:

    GET EM-IRI
    
This will require you to provide your Publications Router credentials (email and password) via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i --user <my_account_id>:<my_api_key> https://pubrouter.jisc.ac.uk/sword2/collections/<MY_ACCOUNT_ID>/<NOTIFICATION_ID>/media

This will return to you the binary content you originally deposited if it still exists in SWORD-IN temporary data store.
