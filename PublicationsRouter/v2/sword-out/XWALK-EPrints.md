# PubRouter EPrints XML Schema Description
This document describes the crosswalk from the notification metadata fields received via the JPER API to the
 XML format supplied to repositories via SWORDv2.

 For more information about the schema see: 
* [EPrints XML](http://wiki.eprints.org/w/XML_Export_Format)

The following table lists the notification fields in the JSON, then the DC/RIOXX fields it will be transformed to, and
any notes regarding the transformation:

| PubRouter Metadata | Eprints.Eprint terms | Description |
|-----------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| journal.title | publication 
| journal.abbrev_title | ---- 
| journal.volume | volume 
| journal.issue | number
| journal.publisher | publisher
| journal.identifier.type | ---- | We list one identifier for a journal, if there are multiple identifiers we prioritise using the identifier with identifier.type value of eissn/essn, then issn and finally any other ssn value. 
| journal.identifier.id | issn | This will be the id value of the favoured ssn type.
| article.title | title | 
| article.sub_title | title | Any subtitles are appended to the title, like so: "Title - \<subtitle1\> - \<subtitle2\> - \<subtitle3\> ..."
| article.type | type | 
| article.version | note | Stored in Licensing information line of note, will see "** License for \<article_version\> version of this article... "
| article.start_page | pagerange | 
| article.end_page | pagerange | 
| article.page_range | pagerange | 
| article.num_pages | pagerange |
| article.language | ---- |
| article.abstract | abstract | 
| article.identifier.type | ---- | We list one identifier for an article, if there are multiple identifiers we prioritise a DOI, else use whichever other identifier is present. 
| article.identifier.id | id_number | 
| article.subject | keywords | Subjects are entered as one entry, separated by commas. For example "\<subject1>, \<subject2>, ..."
| author.type | ---- | 
| author.name.firstname | creators.item.name.given | 
| author.name.surname | creators.item.name.family | 
| author.name.fullname | creators.item.name | 
| author.name.suffix | ---- |
| author.organisation_name | ---- |
| author.identifier.type | creators.item | If type is email, this will be listed as "id", else an individual tag of the same name as author.identifier.type with text of author.identifier.id will be created. 
| author.identifier.id | creators.item.\<type\> | If the id is an email, and there are multiple emails associated with this author, these will be listed as an individual string as a comma separated list. 
| author.affiliation | ---- |
| contributor.type | contributors.item.type |
| contributor.name.firstname | contributors.item.name.given | 
| contributor.name.surname | contributors.item.name.family | 
| contributor.name.fullname | contributors.item.name |
| contributor.name.suffix | ---- | 
| contributor.organisation_name | ---- |
| contributor.identifier.type | ---- |  If id type is email, this will be listed as "id", else an individual tag of the same name as author.identifier.type with text of author.identifier.id will be created. 
| contributor.identifier.id | contributors.item.\<type\> | If the id is an email, and there are multiple emails associated with this contributor, these will be listed as an individual string as a comma separated list. 
| contributor.affiliation | ---- |
| accepted_date | date | If accepted_date is present, but publication_date is not, then date_type field will be 'Accepted' and ispublished field will be 'inpress' 
| publication_date | date | If publication_date is present, then date_type field will be 'published' and ispublished field will be 'pub'
| history_date | ---- | 
| publication_status | ---- |
| funding | funding.item | Full funding information for a funder will be found within an item tag under the funding tag. Information will be presented as so "** Funder: \<funding.name\>; Grant num: \<funding.grant_number\>; \<funding.identifier1.type\>: \<funding.identifier1.id\>; \<funding.identifier2.type\>: \<funding.identifier2.id\>, ..."
| embargo.start | ---- |
| embargo.end | note | Any embargo information will be written in note as "** Embargo End Date: \<embargo.start\>"
| embargo.duration | ---- |
| license_ref | note | License information will be written in note as "** License for \<article_version\> version of this article starting on \<license_ref.start\>: \<license_ref.url\>/\<license_ref.type\>/\<license_ref.title\>"

## Example XML Output

An example Atom Entry document containing the metadata listed above is shown here
```xml
<eprints xmlns="http://eprints.org/ep2/data/2.0">
	<eprint>
		<id_number>55.aa/base.1</id_number>
		<title>Test Article - Test Article SUBtitle</title>
		<abstract>Abstract of the work </abstract>
		<type>article</type>
		<creators>
			<item>
				<name>
					<family>Jones</family>
					<given>Richard</given>
				</name>
				<orcid>aaaa-0000-1111-bbbb</orcid>
				<id>richard@example.com, richard2@example.com</id>
			</item>
			<item>
				<name>
					<family>MacGillivray</family>
					<given>Mark</given>
				</name>
				<orcid>dddd-2222-3333-cccc</orcid>
				<id>mark@example.com</id>
			</item>
		</creators>
		<contributors>
			<item>
				<type>http://www.loc.gov/loc.terms/relators/EDT</type>
				<name>
					<family>Williams</family>
					<given>Manolo</given>
				</name>
				<id>manolo@example.com</id>
			</item>
		</contributors>
		<publisher>Premier Publisher</publisher>
		<publication>Journal of Important Things</publication>
		<volume>Number of a journal (or other document) within a series</volume>
		<number>Issue number of a journal, or in rare instances, a book</number>
		<pagerange>Text describing discontinuous pagination.</pagerange>
		<issn>1234-5678</issn>
		<date>2015-01-01</date>
		<date_type>published</date_type>
		<ispublished>pub</ispublished>
		<keywords>science, technology, arts, medicine</keywords>
		<related_url>
			<item>
				<url>http://testing.no.real.jisc.ac.uk/api/v1/notification/1234567890/content/2</url>
			</item>
			<item>
				<url>http://url</url>
			</item>
		</related_url>
		<funders>
			<item>** Funder: BBSRC; Grant num: BB/34/juwef; ringold: bbsrcid</item>
		</funders>
		<note>
** Embargo End Date: 01-01-2016
** History: submitted 03-07-2014.
** Licence for VoR version of this article starting on 12-11-2016: http://url</note>
	</eprint>
</eprints>
```
