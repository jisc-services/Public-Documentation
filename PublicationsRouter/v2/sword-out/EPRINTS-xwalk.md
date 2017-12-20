# JPER Core Metadata to Dublin Core/RIOXX XML

This document describes the crosswalk from the notification metadata fields received via the JPER API to the
 XML format supplied to repositories via SWORDv2.
 
The approach to the crosswalk is:

* If there is a DC or DCTerms field to which the value can be written, do so
* If there is also a RIOXX field which is different, do that one too

This means that repositories which impelement one or other or both will have good changes of extracting useful
metadata.

For information about the schemas see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX](http://rioxx.net/v2-0-final/)

The following table lists the notification fields in the JSON, then the DC/RIOXX fields it will be transformed to, and
any notes regarding the transformation:

| JPER Field | DC/RIOXX Field | Notes |
| ---------- | -------------- | ----- |
| links.url | dc:identifier | As per the recommended use by RIOXX |
| embargo.end | dcterms:available and ali:license_ref@start_date | "Date (often a range) that the resource became or will become available." from DCMI terms |
| metadata.title | dc:title | |
| metadata.version | rioxxterms:version | May not conform to required use by RIOXX because of unknown nature of the values supplied by publishers |
| metadata.publisher | dc:publisher | |
| metadata.source.name | dc:source | May not conform to required use by RIOXX because multiple dc:source elements may be present |
| metadata.source.identifier | dc:source | prefixed with appropriate namespace |
| metadata.identifier | dc:identifier AND rioxxterms:version_of_record | Always populate dc:identifier, and populate rioxxterms:version_of_record if this is a DOI (as a URL) |
| metadata.type | dc:type | Ignoring the RIOXX recommendation here, as the DC field is present, and is not so restrictive |
| metadata.author | dc:creator AND rioxxterms:author | for dc, use separate fields for creator name and any identifiers; for rioxx use orcid as id (as URL), and add other attributes as required.  See RIOXX guidelines during implenmentation for details. |
| metadata.author.affiliation | dc:contributor | Since there's nowhere else obvious to put this useful bit of information |
| metadata.language | dc:langauage | |
| metadata.publication_date | rioxxterms:publication_date AND dc:date | covering RIOXX and the more likely existing defaults in repositories |
| metadata.date_accepted | dcterms:dateAccepted | |
| metadata.date_submitted | dcterms:dateSubmitted | |
| metadata.license_ref.url | ali:license_ref AND dc:rights | Note that the @start_date should be embargo.end, but may also be any other suitable date from the metadata |
| metadata.license_ref.title | dc:rights | if no metadata.license_ref.url is present |
| metadata.project.* | rioxxterms:project | @funder_name=metadata.project.name, @funder_id=metadata.project.identifier, text=metadata.project.grant_number.  See RIOXX documentation for details in implementation |
| metadata.subject | dc:subject | |

## Example XML Output

An example Atom Entry document containing the metadata listed above is shown here

```xml
    <?xml version="1.0"?>
    <entry xmlns="http://www.w3.org/2005/Atom"
            xmlns:dcterms="http://purl.org/dc/terms/"
            xmlns:atom="http://www.w3.org/2005/Atom"
            xmlns:ali="http://www.niso.org/schemas/ali/1.0/"
            xmlns:rioxxterms="http://www.rioxx.net/schema/v2.0/rioxx/"
            xmlns:dc="http://purl.org/dc/elements/">

        <!-- some atom standard elements -->
        <generator uri="http://bitbucket.org/beno/python-sword2" version="0.1"/>
        <atom:updated>2015-10-13T11:10:47.060613</atom:updated>

        <!-- links to content files -->
        <dc:identifier>http://pubsrouter.jisc.ac.uk/api/v1/notification/1234567890/content</dc:identifier>
        <dc:identifier>http://pubsrouter.jisc.ac.uk/api/v1/notification/1234567890/content/SimpleZip</dc:identifier>

        <!-- embargo information -->
        <dcterms:available>2016-01-01T00:00:00Z</dcterms:available>
        <ali:license_ref start_date="2016-01-01T00:00:00Z">http://creativecommons.org/cc-by</ali:license_ref>

        <!-- article title -->
        <dc:title>An important article about science</dc:title>
        <atom:title>An important article about science</atom:title>

        <!-- version of the article described here -->
        <rioxxterms:version>AAM</rioxxterms:version>

        <!-- publisher of the journal in which the article appears
        <dc:publisher>Premier Publisher</dc:publisher>

        <!-- the journal (name and issns) in which the article appears -->
        <dc:source>Journal of Science</dc:source>
        <atom:source>Journal of Science</atom:source>
        <dc:source>issn:1234-5678</dc:source>
        <dc:source>eissn:1234-5678</dc:source>
        <dc:source>pissn:9876-5432</dc:source>

        <!-- identifier for the item, including version_of_record if a DOI -->
        <dc:source>doi:10.pp/jit</dc:source>
        <dc:identifier>doi:10.pp/jit.1</dc:identifier>
        <rioxxterms:version_of_record>doi:10.pp/jit.1</rioxxterms:version_of_record>

        <!-- type of article -->
        <dc:type>article</dc:type>

        <!-- author of the article (may be repeated), with their various properties -->
        <dc:creator>Richard Jones</dc:creator>
        <atom:author>
            <atom:name>Richard Jones</atom:name>
        </atom:author>
        <dc:creator>orcid:aaaa-0000-1111-bbbb</dc:creator>
        <dc:creator>email:richard@example.com</dc:creator>
        <rioxxterms:author id="orcid:aaaa-0000-1111-bbbb email:richard@example.com">Richard Jones</rioxxterms:author>

        <!-- author affiliation -->
        <dc:contributor>Cottage Labs</dc:contributor>
        <atom:contributor>
            <atom:name>Cottage Labs</atom:name>
        </atom:contributor>

        <!-- language of the article -->
        <dc:language>eng</dc:language>

        <!-- publication date -->
        <rioxxterms:publication_date>2015-01-01T00:00:00Z</rioxxterms:publication_date>
        <dc:date>2015-01-01T00:00:00Z</dc:date>
        <atom:published>2015-01-01T00:00:00Z</atom:published>

        <!-- date accepted -->
        <dcterms:dateAccepted>2014-09-01T00:00:00Z</dcterms:dateAccepted>

        <!-- date submitted -->
        <dcterms:dateSubmitted>2014-07-03T00:00:00Z</dcterms:dateSubmitted>

        <!-- rights information (see also ali:license_ref above) -->
        <dc:rights>http://creativecommons.org/cc-by</dc:rights>
        <atom:rights>http://creativecommons.org/cc-by</atom:rights>

        <!-- funder information -->
        <rioxxterms:project funder_name="BBSRC" funder_id="ringold:bbsrcid">BB/34/juwef</rioxxterms:project>
        <atom:contributor>
            <atom:name>BBSRC</atom:name>
        </atom:contributor>

        <!-- subject/keywords -->
        <dc:subject>science</dc:subject>
        <dc:subject>technology</dc:subject>
        <dc:subject>arts</dc:subject>
        <dc:subject>medicine</dc:subject>
    </entry>
```
