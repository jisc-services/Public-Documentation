# Packaging

Packages, in the context of Publications Router, are zip files which contain one or more other files/folders which conform to a known/documented specification.  For example packages may contain meta-data in JATS format together with related files such as article PDF and or images.

## Identification of packages

Each package format has a URI which unambiguously identifies it, and we use this to unpack and interpret the content that appears in the zip.  (Note that the URI does not have to resolve to a resource on the web, so any links shown below may not actually go to a web page).  Publications Router works with the following package formats:

| Format | Description |
|--------|-------------|
| [https://pubrouter.jisc.ac.uk/FilesAndJATS](#httpspubrouterjiscacukfilesandjats) | A zipped flat file structure that includes a JATS XML file |
| [http://purl.org/net/sword/package/SimpleZip](#httppurlorgnetswordpackagesimplezip) | A zipped, flat file structure of unspecified files |

When sending or retrieving packages from Publications Router, the format needs to be specified.

If you are a publisher sending a package via the Publications Router API, you MUST specify the URI of the package format in the
JSON `content.packaging_format` field, thus:
```json
{
    "content" : {
        "packaging_format" : "https://pubrouter.jisc.ac.uk/FilesAndJATS"
    }
}
```

If you are retrieving packaged content via the Publications Router API, you will obtain the URL for the package from the notification
JSON in the **links** section, for example:
```json
"links" : [
    {
        "type" : "package",
        "format" : "application/zip",
        "url" : "https://pubrouter.jisc.ac.uk/api/v4/notification/123456789/content",
        "packaging" : "https://pubrouter.jisc.ac.uk/FilesAndJATS"
    },
    {
        "type" : "package",
        "format" : "application/zip",
        "url" : "https://pubrouter.jisc.ac.uk/api/v4/notification/123456789/content/SimpleZip.zip",
        "packaging" : "http://purl.org/net/sword/package/SimpleZip"
    }
]
```

The `links.packaging` element which tells you the format of the zip file you will receive if you GET the **url**.

## What package formats are supported?

Publications Router accepts and disseminates packages in the following formats:

| Format | Accepts from publishers| Disseminates to repositories |
|----| :---: | :---: |
| [https://pubrouter.jisc.ac.uk/FilesAndJATS](#httpspubrouterjiscacukfilesandjats) | Yes | Yes |
| [http://purl.org/net/sword/package/SimpleZip](#httppurlorgnetswordpackagesimplezip) | No | Yes |

If a package format can be accepted, publishers may use it to deposit binary content associated with a notification into Publications Router.

If a package format can be disseminated, it will either be sent to your repository via SWORD or will be available to you for download (you must ensure your repository account in Publications Router specifies the package format you are interested in). Note that SimpleZip is the most widely supported format.


## A guide to the formats

### https://pubrouter.jisc.ac.uk/FilesAndJATS 

(Note: this URI does not link anywhere).

This is Publications Router's native package format.  It conforms to the following specification:

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

This is [SWORDv2](http://swordapp.github.io/SWORDv2-Profile/SWORDProfile.html#iris)'s native package format.  It is essentially a standard zip file format, restricted to a flat structure. It conforms to the following specification:

1. Only contains files in a flat structure, does not contain folders
2. May contain an arbitrary number of files of any format

That is, it is the simplest possible packaging format, and it is left to the consumer to decide what to do with the content.
