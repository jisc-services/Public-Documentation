# JPER SWORDv2 Deposit Endpoint

This application provides a thin interface between the JPER notification API and a publisher's SWORDv2 deposit 
client, allowing the publisher to interact with JPER via SWORDv2 rather than its native API.

This document tells you all you need to know to get connected to JPER via this approach.

The API is built on the SWORDv2 specification, which you can [read about here](http://swordapp.github.io/SWORDv2-Profile/SWORDProfile.html)

## Getting Started: retrieving the service document

The Service Document tells you all you need to know about the SWORDv2 endpoint that you're going to be interacting with.

When you start working with the endpoint, you'll probably want to retrieve it to take a look at the server's capabilities,
but you won't need it much after that.

The Service Document lists the collections that you can send your notifications to, the mimetypes and the packaging formats
that are supported.

In this version of the system there are only two collections:

1. validate - you can send content here to check whether it's correctly formatted for sending to the router
2. notify - you can send content here when you're ready to notify us of a new publication

You'll be able to get the URLs for those collections by retrieving the service document, which you can do like this:

    GET /service-document

This will require you to provide your username and api key via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i http://username:api_key@pubsrouter.jisc.ac.uk/sword/service-document

(check with Jisc for the correct base url of the live service).

## Send a package for validation

Once you have constructed a Zip file of the appropriate packaging format (see the JPER packaging documentation for details),
you'll want to check that it works with the router.  You can do this by sending it to the "validate" collection before you
try sending it for real.

Get the URL of the collection from the Service Document (in the section above), and then do the following

    POST /collection/validate
    Content-Disposition: filename=filename.zip
    Content-Type: application/zip
    Packaging: https://pubsrouter.jisc.ac.uk/FilesAndJATS
    
    [binary content]

This will require you to provide your username and api key via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i --data-binary "@test.zip" -H "Content-Disposition: filename=test.zip" -H "Content-Type: application/zip" -H "Packaging: https://pubsrouter.jisc.ac.uk/FilesAndJATS" http://userame:api_key@pubsrouter.jisc.ac.uk/sword/collection/validate

In response to this you will either receive a SWORDv2 error document detailing any problems with the request, or a
successful 202 (Accepted) response.  If you get the latter, you are good to start sending your notifications to the "notify"
collection.

## Send a package as a new notification

Once you have constructed and validated (see above) a  Zip file of the appropriate packaging format (see the JPER packaging documentation for details),
you can send your notifications to the live "notify" collection for inclusion in the router.

Get the URL of the collection from the Service Document (see above), and then do the following

    POST /collection/notify
    Content-Disposition: filename=filename.zip
    Content-Type: application/zip
    Packaging: https://pubsrouter.jisc.ac.uk/FilesAndJATS
    
    [binary content]

This will require you to provide your username and api key via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i --data-binary "@article.zip" -H "Content-Disposition: filename=article.zip" -H "Content-Type: application/zip" -H "Packaging: https://pubsrouter.jisc.ac.uk/FilesAndJATS" http://userame:api_key@pubsrouter.jisc.ac.uk/sword/collection/notify

In response to this you will either receive a SWORDv2 error document detailing any problems with the request, or a
successful 201 (Created) response.  If you get the former, you should consider sending the package to the "validate"
endpoint for a proper check-over.

Along with the successful response you will receive an HTTP header "Location" which gives you the "Edit-IRI" for your
notification.  If you want to access your notification again, you'll need this.  You will also get an XML deposit receipt
document, which provides you with a variety of other URLs to access parts of your created notification (see below for more
details).


## Retrieving details of previously created notifications

Once you've sent something to the "notify" collection, you may want to retrieve it again to see what happened to it.

There are a number of options for ways you can retreive it:

1. You can request a copy of the original deposit receipt you got when you first sent the notification
2. You can request a copy of the binary content package you originally deposited
3. You can request a status report of the notification (a Statement), which will tell you whether it has been routed yet or not

When you deposit the notification for the first time, you will receive back a URL in the Location header of the response.
This contains the "Edit-IRI" of the notification, and as long as you still have this, you can request information about
your notification.

### Retrieving the original deposit receipt

This is obtained by making a request to the Edit-IRI directly, which will return the original deposit receipt:

    GET Edit-IRI
    
This will require you to provide your username and api key via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i http://username:api_key@pubsrouter.jisc.ac.uk/sword/entry/b581851855c7414eaef4d7fd3f49ed50

(Note that the long id at the end will be specific to your individual notifications)

This will give back the XML deposit receipt, which contains the identifiers that you need to access the binary
content and the staus report.

### Retreiving the binary content

To get the binary content, you need the "Edit Media IRI" from the deposit receipt.  If you don't have the deposit
receipt, see the section above on how to get it.

In the deposit receipt you'll see a section that looks like this:

    <link rel="edit-media" href="http://pubsrouter.jisc.ac.uk/sword/entry/b581851855c7414eaef4d7fd3f49ed50/content"/>

This is the "Edit Media IRI" (EM-IRI), and you can do:

    GET MR-IRI
    
This will require you to provide your username and api key via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i http://username:api_key@pubsrouter.jisc.ac.uk/sword/entry/b581851855c7414eaef4d7fd3f49ed50/content

This will return to you the binary content you originally deposited.


### Retrieving the Statement

To get the statement you need the "State-IRI" from the deposit receipt.  If you don't have the deposit
receipt, see the section above on how to get it.

In the deposit receipt you'll see a section that looks like this:

    <link rel="http://purl.org/net/sword/terms/statement" 
        type="application/atom+xml;type=feed" 
        href="http://pubsrouter.jisc.ac.uk/sword/entry/b581851855c7414eaef4d7fd3f49ed50/statement/atom"/>

(there may be more than one of these, as the statement is available in different formats - pick your favourite).

You can then do:

    GET State-IRI

This will require you to provide your username and api key via HTTP Basic Authentication.

For example, using curl, you might do the following:

    curl -i http://username:api_key@pubsrouter.jisc.ac.uk/entry/b581851855c7414eaef4d7fd3f49ed50/statement/atom

This will return to you an XML document (in this case as an Atom Feed) which describes the structure and state of
your item, as per the SWORDv2 specification documentation.

If you are feeling particularly adventurous, you may also content negotiate with the server for the format of the statement
via the Edit-IRI (where you get the deposit receipt from):

    curl -i -H "Accept: application/atom+xml;type=feed" http://username:api_key@pubsrouter.jisc.ac.uk/entry/b581851855c7414eaef4d7fd3f49ed50
    
    curl -i -H "Accept: application/rdf+xml" http://username:api_key@pubsrouter.jisc.ac.uk/entry/b581851855c7414eaef4d7fd3f49ed50

