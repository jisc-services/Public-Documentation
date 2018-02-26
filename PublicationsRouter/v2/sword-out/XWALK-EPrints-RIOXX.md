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

The following table lists first the Dublin Core fields which the xwalk populates. It then lists the RIOXX fields which the xwalk populates. 

| Terms | PubRouter Metadata fields | Description | Example
|:-----------------------------:|:---------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------:|
| pr:source | journal.title, journal.abbrevTitle,  journal.volume,  journal.issue| | <pr:source volume=[journal.volume] issue=[journal.issue]> [journal.title] + [journal.abbrevTitle] </pr:source> |
| dcterms:publisher  | journal.publisher | | \<dcterms:publisher\> [journal.publisher] \</dcterms:publisher\> |
| pr:source_id | journal.identifier.id, journal.identifier.type | | <pr:source_id type=[journal.identifier.type]> [journal.identifier.id] \</pr:source_id>
| dcterms:title | article.title | A name given to the resource. | \<dcterms:title> [article.title] </dcterms:title>
| dcterms:type | article.type | The nature or genre of the resource. | \<dcterms:type> [article.type] </dcterms:type>
| rioxxterms:type | article.type | The nature or genre of the resource. | \<rioxxterms:type> [article.type] </rioxxterms:type>
| pr:start_page | article.start_page |  | \<pr:start_page> [article.start_page] \</pr:start_page>
| pr:end_page | article.end_page |  | \<pr:end_page> [article.end_page] \</pr:end_page>
| pr:page_range | article.page_range |  | \<pr:page_range> [article.page_range] \</pr:page_range>
| pr:num_pages | article.num_pages |  | \<pr:num_pages> [article.num_pages] \</pr:num_pages>
| article.language | dcterms :language |  |
| article.abstract | dcterms :abstract |  |
| article.identifier.type | pr :identifier  | <pr:identifier> type: id </pr:identifier>  <rioxxterms:version_of_record> doi </rioxxterms:version_of_record> |
| article.identifier.id | pr :identifier  | ditto |
| article.subject | dcterms :subject |  |
| author.type | pr: author | <pr: author>  <pr:type> type </pr:type>  </pr:author> |
| author.name | pr: author | <pr: author>  <pr:surname> </pr:surname>  <pr:firstname> </pr:firstname>  <pr:suffix> </pr:suffix> </pr:author> |
| author.organisation_name | pr: author | <pr: author>  <pr:org_name> organisation_name </pr:org_name>  </pr:author> |
| author.identifier.orcid | pr: author | <pr: author>  <pr:id> orcid </pr:id>  </pr:author> |
| author.identifier.email | pr: author | <pr: author>  <pr:email> email </pr:email>  </pr:author> |
| author.affiliation | ---- |   |
| contributor.type | pr: contributor | <pr:contributor>  <pr:type> type </pr:type>  </pr:contributor> |
| contributor.name | pr: contributor | <pr: contributor>  <pr:surname>  </pr:surname>  <pr:firstname>  </pr:firstname>     <pr:suffix> </pr:suffix> </pr:contributor> |
| contributor.organisation_name | pr: contributor | <pr: contributor>  <pr:org_name> organisation_name </pr:org_name>  </pr:contributor> |
| contributor.identifier.orcid | pr: contributor | <pr: contributor>  <pr:id> orcid </pr:id>  </pr:contributor> |
| contributor.identifier.email | pr: contributor | <pr: contributor>  <pr:email> email </pr:email>  </pr:contributor> |
| contributor.affiliation | ---- |   |
| accepted_date | dcterms :dateAccepted |  |
| publication_date | rioxxterms: publication_date  dcterms: medium | Date of formal issuance (e.g., publication) of the resource.  <dcterms:medium>publication_format </dcterms:medium> |
| history_date.type | pr: history_date | <pr:history_date  type="type"> date </pr:history_date> |
| history_date.date * | pr: history_date | Eprints uses "submitted" when history_date.type is "received"   <pr:history_date  type="submitted"> date </pr:history_date> |
| publication_status | --- |  |
| project.name  | rioxxterms: project | <rioxxterms:project rioxxterms:funder_id="identifier" rioxxterms:funder_name="name"> grant_number </rioxxterms:project> |
| project.identifier | rioxxterms: project |  |
| project.grant_number | rioxxterms: project |  |
| embargo.start | pr: embargo | <pr:embargo start_date="..." end_date="..." /> |
| embargo.end | pr: embargo | ditto |
| embargo.duration | --- |  |
| license_ref.title | ali: free_to_read  pr:license | <ali:free_to_read ali:end_date="..." ali:start_date="..."/>  <pr:license start_date="..." url="..." version="..."> tittle + type </pr:license>  |
| license_ref.type | ali: free_to_read pr:license | ditto |
| license_ref.url | ali: free_to_read pr:license | ditto |
| license_ref.version | ali: free_to_read pr:license | ditto |
| license_ref.start | ali: free_to_read pr:license | ditto |
| provider_agent | pr: note | From  {provider_agent} via Jisc Publications Router.' |
| links | pr: relation  pr: download_link | <pr:relation url="..." format="..." packaging="..." />  <pr:download_link format="..." packaging="..." [ public=Bool ]"  primary=Bool filename="..." /> |
| formats_to_download | dcterms: format | element for any downloadable format |


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