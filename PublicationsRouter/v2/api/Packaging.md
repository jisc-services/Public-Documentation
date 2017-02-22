# Packaging

Packages, in the context of PubRouter, are zip files which contain one or more other files/folders which conform to a known/documented specification.  For example packages may contain meta-data in JATS format together with related files such as article PDF and or images.

## Identification of packages

Each package format has a URI which unambiguously identifies it, and we use this to unpack and interpret the content that appears in the zip.  (Note that the URI does not have to resolve to a resource on the web, so any links shown below may not actually go to a web page).  PubRouter works with the following package formats:

| Format | Description |
|--------|-------------|
| [https://pubrouter.jisc.ac.uk/FilesAndJATS](https://github.com/sherpaservices/Public-Documentation/blob/master/PublicationsRouter/api/v2/Packaging.md#httpspubsrouterjiscacukfilesandjats) | A flat file structure with JATS XML embedded |
| [http://purl.org/net/sword/package/SimpleZip](https://github.com/sherpaservices/Public-Documentation/blob/master/PublicationsRouter/api/v2/Packaging.md#httppurlorgnetswordpackagesimplezip) | A zipped, flat file structure of unspecified files |

When sending or retrieving packages from PubRouter, the format needs to be specified.

If you are a publisher sending a package via the PubRouter API, you MUST specify the URI of the package format in the
JSON `content.packaging_format` field, thus:

    {
        "content" : {
            "packaging_format" : "https://pubsrouter.jisc.ac.uk/FilesAndJATS"
        },
    }

If you are retrieving packaged content via the PubRouter API, you will obtain the URL for the package from the notification
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

The `links.packaging` element which tells you the format of the zip file you will receive if you GET the **url**.

## What package formats are supported?

PubRouter accepts and disseminates packages in the following formats:

| Format | Accepts | Disseminates |
|--------|---------|--------------|
| [https://pubrouter.jisc.ac.uk/FilesAndJATS](https://github.com/sherpaservices/Public-Documentation/blob/master/PublicationsRouter/api/v2/Packaging.md#httpspubsrouterjiscacukfilesandjats) | yes | yes |
| [http://purl.org/net/sword/package/SimpleZip](https://github.com/sherpaservices/Public-Documentation/blob/master/PublicationsRouter/api/v2/Packaging.md#httppurlorgnetswordpackagesimplezip) | no | yes |

If a package can be accepted, publishers may use it to deposit binary content associated with a notification into PubRouter.

If a package can be disseminated, it will be available to you for download once the notification has been routed to your
repository (you must ensure your repository account in PubRouter specifies the package format you are interested in).


## A guide to the formats

### https://pubsrouter.jisc.ac.uk/FilesAndJATS 

(Note: this URI does not link anywhere).

This is PubRouter's native package format.  It conforms to the following specification:

1. Only contains files in a flat structure, does not contain folders
2. Contains a JATS XML file (no naming convention, but must end with ".xml")
3. May contain an arbitrary number of other binary files (e.g. pdfs, images, etc)

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
