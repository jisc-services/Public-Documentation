# Publications Router EPrints XML Description

This document describes the XML output by Publications Router for ingest into EPrints repositories via the SWORDv2 interface.

For more information about the schema see: 
* [EPrints XML](http://wiki.eprints.org/w/XML_Export_Format)

The following table lists:
* Column 1 - the XML elements output by Publications Router 
* Column 2 - Publications Router internal metadata JSON fields from which the output is derived
* Column 3 - the output XML element construction (format) - see note. 

**NOTE: XML Format column** - Field holders are shown in `[square brackets]`, in the output XML these field holders are replaced by data from the indicated JSON metadata fields.  For example, `[journal.title]` would be replaced by the actual title of the journal.  Conditional phrases are shown within `{curly brackets}` these will be omitted if there is no data to display; within such phrases choices in data to display are indicated by `|` character, the first non-blank data item is displayed. Any other text is output as it appears in the format.

| Eprint element terms | Publications Router Metadata | XML Format |
|-----------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| publication |  journal.title | `<publication>[journal.title]</publication>` |
| volume | journal.volume | `<volume>[journal.volume]</volume>` |
| number | journal.issue | `<number>[journal.issue]</volume>` |
| publisher | journal.publisher | `<publisher>[journal.publisher]</publisher>` |
| issn | journal.identifier.id | `<issn>[journal.identifier.id]</issn>` |
| title | article.title <br> article.subtitle | `<title>[article.title] - [article.subtitle 1] - [article.subtitle 2] -...</title>` |
| type | article.type | `<type>[article.type]</type>` |
| pagerange | article.page_range <br> article.article_e_num <br> | `<pagerange>[article.page_range]</pagerange>` <br><br>Or, if article.page_range is absent: <br> `<pagerange>[article.article_e_num]</pagerange>`  |
| abstract | article.abstract | `<abstract>[article.abstract]</abstract>` |
| id_number | article.identifier.id | `<id_number>[article.identifier.id]</id_number>` |
| keywords | article.subject | `<keywords>[article.subject 1], [article.subject 2], ...</keywords>` |
| creators.item | author.name.surname <br> author.name.firstname <br> author.identifier.type <br> author.identifier.id <br> author.email | `<creators>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<item>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<[author.identifier.type]> author.identifier.id </[author.identifier.type]>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<id>[author.email]</id>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<name>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `<family>[author.name.surname]</family>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<given>[author.name.firstname]</given>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `</name>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `</item>` <br> `</creators>` |
| contributors.item | contributor.name.surname <br> contributor.name.firstname <br> contributor.identifier.type <br> contributor.identiier.id <br> contributor.type <br> contributor.email | `<contributors>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<item>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<type>[contributor.type]</type>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<[contributor.identifier.type]>contributor.identifier.id</[contributor.identifier.type]>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<id>[contributor.email]</id>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<name>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `<family>[contributor.name.surname]</family>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `<given>[contributor.name.firstname]</given>` <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `</name>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `</item>` <br> `</contributors>` |
| date | publication_date <br> accepted_date | `<date>{[publication_date]\|[accepted_date]}</date>` <br><sub>Output date is publication date, otherwise accepted date.</sub> |
| date_type | publication_date <br> accepted_date | `<date_type>(published\|Accepted)</date_type>` <br><sub>Output corresponds to the date type appearing in `<date>` element.</sub> |
| funders.item | funding.grant_numbers <br> funding.name <br> funding.identifier | `<funders>` <br> &nbsp;&nbsp;&nbsp;&nbsp; `<item>** Funder: [funding.name]; [funding.identifier.type]: [funding.identifier.id]; Grant(s): [funding.grant_numbers]</item>` <br> `</funders>` |
| note | article.version | The article version appears in `<note>` element as: <br> `** Article version: [article.version]` |
| note | embargo.end | Embargo details appear in `<note>` element as: <br> `** Embargo end date: [embargo.start]` |
| note | provider.agent | Provider details appear in `<note>` element as: <br> `** From [provider.agent] via Jisc Publications Router` |
| note | history_date.date_type <br> history_date.date  | History dates appear in `<note>` element as: <br> `** History: [history_date.date_type]{; [history_date.date_type]…}` <br> <sub>Note: will include as many history dates as are available.</sub>   |
| note | license_ref.title <br> license_ref.type <br> license_ref.url <br> license_ref.start <br> article.version | License details appear in `<note>` element as: <br> `** License for{ [article.version] version of} this article{ starting on [license_ref.start]}: {[license_ref.url]\|[license_ref.title]\|[license_ref.type]}` <br> <sub>Note: The output string contains {conditional phrases} that are included only if the field they contain is not empty; the '\|' character separates alternative values, where the first non-empty value is used.</sub> |
| note | ack | Acknowledgements appear in `<note>` element as: <br> `** Acknowledgements: [ack]` |
| refereed | peer_reviewed | `<refereed>TRUE\|FALSE</refereed>` |

## Example XML Output

An example Atom Entry document containing the metadata listed above is shown here
```xml
<eprints xmlns="http://eprints.org/ep2/data/2.0">
	<eprint>
		<id_number>55.aa/base.1</id_number>
		<title>Test Article - Test Article SUBtitle</title>
		<abstract>Abstract of the work </abstract>
		<type>article</type>
		<refereed>TRUE</refereed>
		<creators>
			<item>
				<name>
					<family>Smith</family>
					<given>Richard</given>
				</name>
				<orcid>8888-0000-1111-9999</orcid>
				<id>richard@example.com, richard2@example.com</id>
			</item>
			<item>
				<name>
					<family>Jones</family>
					<given>Mark</given>
				</name>
				<orcid>6666-2222-3333-7777</orcid>
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
			<item>** Funder: BBSRC; ringold: bbsrcid; Grant(s): BB/34/juwef</item>
		</funders>
		<note>** Article version: VoR
** Embargo end date: 12-12-2016
** From Premier Publisher via Jisc Publications Router
** History: submitted 03-07-2014.
** Licence for VoR version of this article starting on 12-12-2016: https://testing.org/licenses/by/4.0/
** Acknowledgements: ...Some acknowledgement text...</note>
	</eprint>
</eprints>
```
