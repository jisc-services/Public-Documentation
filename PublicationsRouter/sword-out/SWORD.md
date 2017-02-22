# JPER SWORDv2 Protocol Operations

This document describes the process of carrying out SWORD deposits against repositories subscribed to the JPER
service.

Section numbers referenced here are as per the [SWORDv2 profile](http://swordapp.github.io/SWORDv2-Profile/SWORDProfile.html):

The following operations are utilised

* 6.3.3. Creating a Resource with an Atom Entry
* 6.5.1. Replacing the File Content of a Resource
* 6.7.1. Adding Content to the Media Resource
* 9.3. Completing a Previously Incomplete Deposit


## Metadata Deposit

This is the process which creates a new repository object containing the [notification metadata](https://github.com/JiscPER/jper-sword-out/blob/develop/docs/system/XWALK.md).

In line with section 6.3.3. of the SWORDv2 specification, it is as follows:

    POST /collection
    In-Progress: True|False
    
    [Atom Entry XML Document]

In-Progress is set to True if this is an EPrints repository OR there is binary content associated with the notification,
otherwise it is set to False.

This creates a new object in the repository, based around the metadata in the Atom Entry XML Document, and returns
a deposit receipt.

If the repository does not return a deposit receipt (which is allowed by the specification), this will be followed up with an explicit
request for it

    GET /Edit-IRI

If this is an EPrints instance, we also carry out a further operation to ensure that a full copy of the XML metadata
is stored (because EPrints does not do that by default).  We do this using the process to add content to the media resource
as per section 6.7.1 of the SWORDv2 specification.

    POST /EM-IRI
    
    [Atom Entry XML Document]
    
This has the effect of adding the XML document as a file to the eprint.

## Packaged Content Deposit

This is the process that adds the package of binary content associated with the notification to the repository item
created in the previous step.

In general, this request takes the form of section 6.5.1 of the SWORDv2 specification:

    PUT /EM-IRI
    
    [Binary Content]
    
This has the effect of replacing any existing content on the item (of which there is none, as we created it with a 
metadata-only deposit in the previous step) with the new packaged content.

If this is an EPrints instance, we already added content to the media resource in the form of the Atom Entry XML document,
so we cannot overwrite it using the above operation.  Instead we add content to the media resource again, as per section
6.7.1 of the SWORDv2 specification:

    POST /EM-IRI
    
    [Binary Content]
    
This has the effect of appending the file to the list of files in the eprint.  EPrints will also unpack the files from
the zip.

## "Complete" the deposit

This operation is not carried out on EPrints repositories, as they are not able to action the request.

    POST /SE-IRI
    In-Progress: False
    
This has the effect of telling the repository that no further content files are coming for this item, and it can
be injected into whatever deposit workflow is required.
