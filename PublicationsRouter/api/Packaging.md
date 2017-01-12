# Packaging

Packages, in the context of the Router, are zip files which contain one or more other files/folders which conform to 
and known/documented specification.

We use these known package formats to allow us to reliably extract match data, and to allow repositories to correctly
interpret the content they are being given.

## Identification of packages

Each format has a URI which unambiguously identifies it, and we use this identifier to allow us to interpret the content
that appears in the zip.  (Note that the URI does not need to resolve to a resource on the web, so any links presented
here may not actuall go to a web page).

Router works with the following package formats

| Format | Description |
|--------|-------------|
| https://pubrouter.jisc.ac.uk/FilesAndJATS | A flat file structure with JATS XML embedded |
| http://purl.org/net/sword/package/SimpleZip | A flat file structure of unspecified files |

When sending or retrieving packages from the router, the format needs to be specified.

If you are a publisher sending a package via the native API, you MUST specify the URI of the package format in the
notification JSON, thus:

    {
        "content" : {
            "packaging_format" : "https://pubsrouter.jisc.ac.uk/FilesAndJATS"
        },
    }

If you are retrieving packaged content via the native API, you will obtain the URL for the package from the notification
JSON in the **links** section, for example:

    "links" : [
        {
            "type" : "package",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v1/notification/123456789/content",
            "packaging" : "https://pubsrouter.jisc.ac.uk/FilesAndJATS"
        },
        {
            "type" : "package",
            "format" : "application/zip",
            "url" : "https://pubrouter.jisc.ac.uk/api/v1/notification/123456789/content/SimpleZip",
            "packaging" : "http://purl.org/net/sword/package/SimpleZip"
        }
    ]

In each of the two links there is a **packaging** element which tells you the format of the zip file you will receive
if you GET the **url**.

## What package formats are supported?

The router accepts and disseminates packages in the following formats:

| Format | Accepts | Disseminates |
|--------|---------|--------------|
| https://pubsrouter.jisc.ac.uk/FilesAndJATS | yes | yes |
| http://purl.org/net/sword/package/SimpleZip | no | yes |

If a package can be accepted, publishers may use it to deposit binary content associated with a notification into Router

If a package can be disseminated, it will be available to you for download once the notification has been routed to your
repository (you must ensure your repository account in the Router specifies the package format you are interested in).


## A guide to the formats

### https://pubsrouter.jisc.ac.uk/FilesAndJATS

This is Router's native package format.  It conforms to the following specification:

1. Only contains files in a flat structure, does not contain folders
2. Contains at least one JATS XML file (no naming convention, but must end with ".xml")
3. May contain one EPMC-style XML metadata file (no naming convention, but must end with ".xml")
4. May contain an arbitrary number of other binary files (e.g. pdfs, images, etc)

The JATS XML file may be any version of JATS or the preceeding NLM Article DTD which contains/supports some or all of the following
xpath expressions:

    //article-meta/article-id[@pub-id-type='doi']
    //article-meta/article-id[@pub-id-type='pmcid']
    //article-meta/pub-date
    //article-meta/pub-date[@date-type='pub']
    //contrib-group/contrib
    //email
    //history/date[@date-type='accepted']
    //history/date[@date-type='received']
    //journal-meta/issn
    //license
    //publisher/publisher-name
    //title-group/article-title

### http://purl.org/net/sword/package/SimpleZip

This is [SWORDv2](http://swordapp.github.io/SWORDv2-Profile/SWORDProfile.html#iris)'s native package format.  It confirms to the following specification:

1. Only contains files in a flat structure, does not contain folders
2. May contain an arbitrary number of files of any format

That is, it is the simplest possible packaging format, and it is left to the consumer to decide what to do with the content.
