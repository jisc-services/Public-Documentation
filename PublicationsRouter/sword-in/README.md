# PubRouter SWORDv2 Deposit Endpoint

PubRouter provides a SWORD2 interface that enables publishers to deposit article metadata and content into PubRouter using a SWORD client (as an alternative to the PubRouter REST API or FTP).

This page tells you all you need to know to get connected to PubRouter using this approach.

The interface is built on the SWORDv2 specification, which you can [read about here](http://swordapp.github.io/SWORDv2-Profile/SWORDProfile.html)

The PubRouter SWORD2 service endpoint is:

    https://pubrouter.jisc.ac.uk/sword
    

## Getting Started: retrieving the Service Document

The Service Document tells you all you need to know about the SWORDv2 endpoint that you're going to be interacting with.

When you start working with the endpoint, you'll probably want to retrieve it to take a look at the server's capabilities,
but you won't need it much after that.

The Service Document lists the collections that you can send your notifications to, the mimetypes and the packaging formats
that are supported.

You will have access to a single collection, with your PubRouter Account ID as its name.

You can get the URL for your collection by retrieving the service document, which you can do like this:

    GET /

This will require you to provide your PubRouter credentials (email and password) via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i https://email:password@pubrouter.jisc.ac.uk/sword/

## Send a package as a new notification

If you want to send a notification as a package, the zip should be in a format listed [here.](../api/Packaging.md#a-guide-to-the-formats)

Get the URL of the collection from the Service Document (see above), and then do the following

    POST /collections/<YOUR_ACCOUNT_ID>
    Content-Disposition: filename=filename.zip
    Content-Type: application/zip
    
    [binary content]

This will require you to provide your PubRouter credentials (email and password) via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i --data-binary "@article.zip" -H "Content-Disposition: attachment; filename=article.zip" -H "Content-Type: application/zip" https://email:password@pubrouter.jisc.ac.uk/sword/collections/<YOUR_ACCOUNT_ID>

In response to this you will either receive a SWORDv2 error document detailing any problems with the request, or a
successful 201 (Created) response.  If you get a different response, the returned XML should provide an explanation of your error.

Along with the successful response you will receive an HTTP header "Location" which gives you the "Edit-IRI" for your
notification.  If you want to access your notification again, you'll need this.  You will also get an XML deposit receipt
document, which provides you with a variety of other URLs to access parts of your created notification (see below for more
details).

## Deposit metadata and files separately

You can also deposit metadata and files separately using the Continued Deposit functionality of SWORD2.

Get the URL of the collection from the Service Document the same in the previous section. Instead of submitting a package, you can submit multiple files. An example of this, using curl is below:

    curl -i --data-binary "@jats.xml" -H "Content-Disposition: attachment; filename=jats.xml" -H "Content-Type: application/xml" -H "In-Progress: true" https://email:password@pubrouter.jisc.ac.uk/sword/collections/<YOUR_ACCOUNT_ID>

After this, you can continue your deposit by using the "Edit-Media" IRI as part of the SWORD2 spec, which will be listed in the deposit receipt of the previous request. Just remember to set the In-Progress header to false (or not set it at all) when you have finished depositing files, or it will never be processed:

    curl -i --data-binary "@article.pdf" -H "Content-Disposition: attachment; filename=article.pdf" -H "Content-Type: application/xml" https://email:password@pubrouter.jisc.ac.uk/sword/collections/<YOUR_ACCOUNT_ID>/<YOUR_NOTIFICATION_ID>/media

You can repeat this for as many files as you like, but note that while any number of XML files may be submitted, only the first - which MUST be JATS XML - will be processed as meta-data and used to create the notification; the remainder will be treated as supplementary article files.

## Retrieving details of previously created notifications

Once you've deposited a notification, you may want to retrieve it again to see what happened to it.

There are a number of options for ways you can retrieve it:

1. You can request a copy of the original deposit receipt you got when you first sent the notification
2. You can request a copy of the binary content package you originally deposited
3. You can request a status report of the notification (a Statement), which will tell you whether it has been routed yet or not

When you deposit the notification for the first time, you will receive back a URL in the Location header of the response.
This contains the "Edit-IRI" of the notification, and as long as you still have this, you can request information about
your notification.

### Retrieving the original deposit receipt

This is obtained by making a request to the Edit-IRI directly, which will return the original deposit receipt:

    GET Edit-IRI
    
This will require you to provide your email and password via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i http://username:api_key@pubrouter.jisc.ac.uk/sword/collections/<YOUR_ACCOUNT_ID>/<YOUR_NOTIFICATION_ID>

This will give back the XML deposit receipt, which contains the identifiers that you need to access the binary
content and the staus report.

### Retrieving the binary content

To get the binary content, you need the "Edit Media IRI" from the deposit receipt.  If you don't have the deposit
receipt, see the section above on how to get it.

In the deposit receipt you'll see a section that looks like this:

    <link rel="edit-media" href="http://pubrouter.jisc.ac.uk/sword/collections/<YOUR_ACCOUNT_ID>/<YOUR_NOTIFICATION_ID>/media"/>

This is the "Edit Media IRI" (EM-IRI), and you can do:

    GET EM-IRI
    
This will require you to provide your PubRouter credentials (email and password) via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i http://email:password@pubrouter.jisc.ac.uk/sword/collections/<YOUR_ACCOUNT_ID>/<YOUR_NOTIFICATION_ID>/media

This will return to you the binary content you originally deposited.
