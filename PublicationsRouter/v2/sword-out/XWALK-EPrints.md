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

| PubRouter Metadata | DC terms | Description |
|:-----------------------------:|:---------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------:|
| journal.title | pr: source | <pr:source volume="..." issue="..."> title + abbrev </pr:source> |
| journal.abbrevTitle | pr: source | ditto |
| journal.volume | pr: source | ditto |
| journal.issue | pr: source | ditto |
| journal.publisher | dcterms: publisher |  |
| journal.identifier.issn | pr: source_id | <pr:source_id type="..."> value </pr:source_id> |
| journal.identifier.eissn | pr: source_id | <pr: source_id> eissn: value </pr: source_id> |
| journal.identifier.pissn | pr: source_id | <pr: source_id> pissn: value </pr: source_id> |
| journal.identifier.doi | pr: source_id | <pr: source_id> doi: value </pr: source_id> |
| article.title | dcterms: title | A name given to the resource. |
| article.subtitle | dcterms: title | A name given to the resource. |
| article.type | dcterms: type rioxxterms: type | The nature or genre of the resource. |
| article.version | rioxxterms: version |  |
| article.start_page | pr: start_page |  |
| article.end_page | pr: end_page |  |
| article.page_range | pr: page_range |  |
| article.num_pages | pr: num_pages |   |
| article.language | dcterms :language |  |
| article.abstract | dcterms :abstract |  |
| article.identifier.type | pr :identifier  | <pr:identifier> type: id </pr:identifier>   <rioxxterms:version_of_record> doi </rioxxterms:version_of_record> |
| article.identifier.id | pr :identifier  | ditto |
| article.subject | dcterms :subject |  |
| author.type | pr: author | <pr: author>      <pr:type> type </pr:type> </pr:author> |
| author.name | pr: author | <pr: author>     <pr:surname> </pr:surname>     <pr:firstname> </pr:firstname>     <pr:suffix> </pr:suffix> </pr:author> |
| author.organisation_name | pr: author | <pr: author>     <pr:org_name> organisation_name </pr:org_name> </pr:author> |
| author.identifier.orcid | pr: author | <pr: author>      <pr:id> orcid </pr:id> </pr:author> |
| author.identifier.email | pr: author | <pr: author>     <pr:email> email </pr:email> </pr:author> |
| author.affiliation | ---- |   |
| contributor.type | pr: contributor | <pr:contributor>\n<pr:type> type </pr:type>\n</pr:contributor> |
| contributor.name | pr: contributor | <pr: contributor>     <pr:surname> </pr:surname>     <pr:firstname> </pr:firstname>     <pr:suffix> </pr:suffix> </pr:contributor> |
| contributor.organisation_name | pr: contributor | <pr: contributor>     <pr:org_name> organisation_name </pr:org_name> </pr:contributor> |
| contributor.identifier.orcid | pr: contributor | <pr: contributor>     <pr:id> orcid </pr:id> </pr:contributor> |
| contributor.identifier.email | pr: contributor | <pr: contributor>     <pr:email> email </pr:email> </pr:contributor> |
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
<entry xmlns:dcterms="http://purl.org/dc/terms/" xmlns:rioxxterms="http://www.rioxx.net/schema/v2.0/rioxx/" xmlns:ali="http://www.niso.org/schemas/ali/1.0/" xmlns:pr="http://pubrouter.jisc.ac.uk/rioxxplus/">
	<pr:note>** From FTP publisher via Jisc Publications Router.</pr:note>
	<pr:download_link url="http://pubrouter.jisc.ac.uk/api/v2/notification/c37bcc16c2474ca8be1e7028aa1ed53b/content/eprints-rioxx/non-pdf-files.zip" format="application/zip" filename="non-pdf-files.zip" packaging="http://purl.org/net/sword/package/SimpleZip" primary="false"/>
	<pr:download_link url="http://pubrouter.jisc.ac.uk/api/v2/notification/c37bcc16c2474ca8be1e7028aa1ed53b/content/eprints-rioxx/elife-0000.pdf" format="application/pdf" primary="true" filename="elife-0000.pdf"/>
	<dcterms:format>application/pdf</dcterms:format>
	<pr:source volume="6">eLife</pr:source>
	<pr:source_id type="eissn">2050-084X</pr:source_id>
	<dcterms:publisher>eLife Sciences Publications, Ltd</dcterms:publisher>
	<dcterms:title>Using the Volta phase plate with defocus for cryo-EM single particle analysis</dcterms:title>
	<rioxxterms:type>Journal Article/Review</rioxxterms:type>
	<dcterms:type>article</dcterms:type>
	<pr:page_range>e00001</pr:page_range>
	<dcterms:language>en</dcterms:language>
	<dcterms:abstract>Previously, we reported an in-focus data acquisition ...</dcterms:abstract>
	<pr:identifier type="publisher-id">00000</pr:identifier>
	<pr:identifier type="doi">10.7554/elife.00000</pr:identifier>
	<dcterms:subject>Research Advance</dcterms:subject>
	<dcterms:subject>Biophysics and Structural Biology</dcterms:subject>
	<dcterms:subject>phase plate</dcterms:subject>
	<dcterms:subject>cryo-EM</dcterms:subject>
	<dcterms:subject>proteasome</dcterms:subject>
	<dcterms:subject>None</dcterms:subject>
	<dcterms:dateAccepted>2017-01-20</dcterms:dateAccepted>
	<rioxxterms:publication_date>2017-01-21</rioxxterms:publication_date>
	<dcterms:medium>electronic</dcterms:medium>
	<pr:history_date type="received">2016-11-07</pr:history_date>
	<pr:history_date type="submitted">2016-11-07</pr:history_date>
	<pr:history_date type="collection">2017</pr:history_date>
	<pr:history_date type="accepted">2017-01-20</pr:history_date>
	<pr:history_date type="pub">2017-01-21</pr:history_date>
	<pr:license url="http://creativecommons.org/licenses/by/4.0/">This article is distributed under the terms of the Creative Commons Attribution License.</pr:license>
	<pr:author>
		<pr:type>http://www.loc.gov/loc.terms/relators/AUT</pr:type>
		<pr:id type="orcid">0000-0001-6406-0000</pr:id>
		<pr:email>danev@biochem.mpg.de</pr:email>
		<pr:surname>Danev</pr:surname>
		<pr:firstnames>Radostin</pr:firstnames>
	</pr:author>
	<pr:author>
		<pr:type>http://www.loc.gov/loc.terms/relators/AUT</pr:type>
		<pr:id type="orcid">0000-0001-7019-0000</pr:id>
		<pr:surname>Tegunov</pr:surname>
		<pr:firstnames>Dimitry</pr:firstnames>
	</pr:author>
	<pr:author>
		<pr:type>http://www.loc.gov/loc.terms/relators/AUT</pr:type>
		<pr:surname>Baumeister</pr:surname>
		<pr:firstnames>Wolfgang</pr:firstnames>
	</pr:author>
	<pr:contributor>
		<pr:type>http://www.loc.gov/loc.terms/relators/EDT</pr:type>
		<pr:surname>Scheres</pr:surname>
		<pr:firstnames>Sjors HW</pr:firstnames>
	</pr:contributor>
</entry>
```
