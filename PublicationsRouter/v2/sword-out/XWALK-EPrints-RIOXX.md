# JPER Core Metadata to Dublin Core/RIOXX XML

This document describes the crosswalk from the notification metadata fields received via the JPER API to the
 XML format supplied to repositories via SWORDv2.
 
The approach to the crosswalk is:

* If there is a DC or DCTerms field to which the value can be written, do so
* If there is also a RIOXX field which is different, do that one too

This means that repositories which impelement one or other or both will have good changes of extracting useful metadata.

For information about the schemas see:

* [Dubin Core](http://dublincore.org/documents/dcmi-terms/)
* [RIOXX](http://rioxx.net/v2-0-final/)
* [PubRouter.xsd](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd)

The following table lists the various terms which the xwalk will populate, then the relevant metadata items which will populate these terms along with an example of how the xml would look. 

| Terms | PubRouter Metadata fields | Example |
|:-----------------------------|:-----------------------|:--------------------------------------------------------------------------------------------------------------|
| [pr:source](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L133) | journal.title<br> journal.abbrevTitle<br>  journal.volume<br>  journal.issue| \<pr:source volume=[journal.volume] issue=[journal.issue]> [journal.title] + [journal.abbrevTitle] \</pr:source> |
| [dcterms:publisher](http://dublincore.org/documents/dcmi-terms/#terms-publisher)  | journal.publisher | \<dcterms:publisher\> [journal.publisher] \</dcterms:publisher\> |
| [pr:source_id](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L151) | journal.identifier.id<br> journal.identifier.type | <pr:source_id type=[journal.identifier.type]> [journal.identifier.id] \</pr:source_id> |
| [dcterms:title](http://dublincore.org/documents/dcmi-terms/#terms-title) | article.title |  \<dcterms:title> [article.title] </dcterms:title> |
| [dcterms:type](http://dublincore.org/documents/dcmi-terms/#terms-type) | article.type |  \<dcterms:type> [article.type] </dcterms:type> |
| [rioxxterms:type](http://www.rioxx.net/profiles/v2-0-final/) | article.type |  \<rioxxterms:type> [article.type] </rioxxterms:type> |
| [rioxxterms:version](http://www.rioxx.net/profiles/v2-0-final/) | article.version |  \<rioxxterms:version> [article.version] \</rioxxterms:version>
| [pr:start_page](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L173) | article.start_page |  \<pr:start_page> [article.start_page] \</pr:start_page> |
| [pr:end_page](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L180) | article.end_page |  \<pr:end_page> [article.end_page] \</pr:end_page> |
| [pr:page_range](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L187) | article.page_range |   \<pr:page_range> [article.page_range] \</pr:page_range> |
| [pr:num_pages](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L194) | article.num_pages |  \<pr:num_pages> [article.num_pages] \</pr:num_pages> |
| [dcterms:language](http://dublincore.org/documents/dcmi-terms/#terms-language) | article.language | \<dcterms:language> [article.language] \</dcterms:language> |
| [dcterms:abstract](http://dublincore.org/documents/dcmi-terms/#terms-abstract) | article.abstract | \<dcterms:abstract> [article.abstract] \</dcterms:abstract> |
| [pr:identifier](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L203) | article.identifier.id<br> article.identifier.type | \<pr:identifier type=[article.identifier.type]> [article.identifier.id] \</pr:identifier> |
| [dcterms:subject](http://dublincore.org/documents/dcmi-terms/#terms-subject) | article.subject | \<dcterms:subject> [article.subject] \</dcterms:subject> |
| [pr:author](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L30) | author.name<br> author.organisation_name<br> author.identifier<br> author.email author.type | \<pr:author><br> &nbsp;&nbsp;&nbsp;&nbsp;  \<pr:type>[author.type]\</pr:type><br> &nbsp;&nbsp;&nbsp;&nbsp;  \<pr:id type=[author.identifier.type]>[author.identifier.id]\</pr:id><br>  &nbsp;&nbsp;&nbsp;&nbsp; \<pr:email>[author.email]\</pr:email><br> &nbsp;&nbsp;&nbsp;&nbsp;  \<pr:firstnames>[author.name.firstname]\</pr:firstnames><br> &nbsp;&nbsp;&nbsp;&nbsp;  \<pr:surname>[author.name.surname]\</pr:surname> <br> \</pr:author> |
| [pr:contributor](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L30) | contributor.type<br> contributor.name<br> contributor.organisation_name<br> contributor.identifier | \<pr:contributor><br> &nbsp;&nbsp;&nbsp;&nbsp; \<pr:type>[contributor.type]\</pr:type><br> &nbsp;&nbsp;&nbsp;&nbsp; \<pr:id type=[author.identifier.type]>[author.identifier.id]\</pr:id><br> &nbsp;&nbsp;&nbsp;&nbsp; \<pr:email>[contributor.email]\</pr:email> <br> &nbsp;&nbsp;&nbsp;&nbsp; \<pr:surname>[contributor.name.surname]\</pr:surname> <br> &nbsp;&nbsp;&nbsp;&nbsp; \<pr:firstnames>[contributor.name.firstname]\</pr:firstnames> <br> &nbsp;&nbsp;&nbsp;&nbsp; \<pr:org_name>[contributor.organisation_name]\</pr:org_name> <br> \</pr:contributor> |
| [dcterms:dateAccepted](http://dublincore.org/documents/dcmi-terms/#terms-dateAccepted) | accepted_date | \<dcterms:dateAccepted> [accepted_date] \</dcterms:dateAccepted> | 
| [rioxxterms:publication_date](http://www.rioxx.net/profiles/v2-0-final/) | publication_date | \<rioxxterms:publication_date> [publication_date] \</rioxxterms:publication_date> |
| [dcterms:medium](http://dublincore.org/documents/dcmi-terms/#terms-medium) | publication_date.publication_format | \<dcterms:medium> [publication_date.publication_format] \</dcterms:medium> | 
| [pr:history_date](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L224) | history_date.type<br> history_date.date | \<pr:history_date type=[history_date.type]> [history_date.date] \</pr:history_date> |
| [rioxxterms:project](http://www.rioxx.net/profiles/v2-0-final/) | project.name<br> project.identifier<br>project.grant_number | \<rioxxterms:project funder_id=[project.identifier] funder_name=[project.name]> [project.grant_number] \</rioxxterms:project> |
| [pr:embargo](https://github.com/jisc-services/Public-Documentation/blob/master/PublicationsRouter/v2/sword-out/pubrouter-xml/pubrouter.xsd#L275) | embargo.start<br> embargo.end | \<pr:embargo start_date=[embargo.start] end_date=[embargo.end]>\</pr:embargo> |



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
