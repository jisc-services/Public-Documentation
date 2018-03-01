# PubRouter EPrints XML Schema Description
This document describes the XML output by PubRouter for ingestion into EPrints repositories via the SWORDv2 interface.

For more information about the schema see: 
* [EPrints XML](http://wiki.eprints.org/w/XML_Export_Format)

The following table lists the the output XML elements (left most column), PubRouter internal metadata JSON fields (central column) and the XML format which the terms take in the XML (right most column).

| Eprints.Eprint terms | PubRouter Metadata | XML Format |
|-----------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| publication |  journal.title | `<publication> [journal.title] </publication>` |
| volume | journal.volume | `<volume> [journal.volume] </volume>` |
| number | journal.issue | `<number> [journal.issue] </volume>` |
| publisher | journal.publisher | `<publisher> [journal.publisher] </publisher>` |
| issn | journal.identifier.id | `<issn> [journal.identifier.id] </issn>` |
| title | article.title <br> article.subtitle | `<title> [article.title] - [article.subtitle1] - [article.subtitle2] -...</title>` |
| type | article.type | `<type> [article.type] </type>` |
| pagerange | article.page_range | `<pagerange> [article.page_range] </pagerange>` |
| abstract | article.abstract | `<abstract> [article.abstract] </abstract>` |
| id_number | article.identifier.id | `<id_number> [article.identifier.id] </id_number>` |
|  keywords | article.subject | `<keywords> [article.subject1], [article.subject2], ... </keywords>`
| creators.item | author.name.surname <br> author.name.firstname <br> author.identifier.type <br> author.identifier.id <br> author.email | `<creators>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<item>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<[author.identifier.type]> author.identifier.id </[author.identifier.type]>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<id> [author.email] </id>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<name>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `<family> [author.name.surname] </family>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<given> [author.name.firstname] </given>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `</name>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `</item>` <br> `</creators>` |
| contributors.item | contributor.name.surname <br> contributor.name.firstname <br> contributor.identifier.type <br> contributor.identiier.id <br> contributor.type <br> contributor.email |  `<contributors>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<item>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<type> [contributor.type] </type>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<[contributor.identifier.type]> contributor.identifier.id </[contributor.identifier.type]>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<id> [contributor.email] </id>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<name>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `<family> [contributor.name.surname] </family>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<given> [contributor.name.firstname] </given>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `</name>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `</item>` <br> `</contributors>` |
| date | publication_date <br> accepted_date | Prioritises a publication_date <br> `<date> [publication_date OR accepted_date] </date>`
| date_type | publication_date <br> accepted_date | If publication date then published, if accepted date then Accepted <br> `<date_type> [published OR Accepted] </date_type>`
| funders.item | funding.grant_number <br> funding.name <br> funding.identifier | `<funders>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<item> ** Funder: [funding.name]; Grant num: [funding.grant_number]; [funding.identifier.type]: [funding.identifier.id] </item>` <br> `</funders>` |
| note |  embargo.end | Any embargo information will be written in `<note>` as "** Embargo End Date: [embargo.start]"
| note | license_ref <br> article.version | License information will be written in `<note>` as "** License for [article.version] version of this article starting on [license_ref.start]: [license_ref.url OR license_ref.type OR license_ref.title]"

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
