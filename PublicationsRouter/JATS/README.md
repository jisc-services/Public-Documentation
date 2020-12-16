# JATS Metadata #

Publishers that submit their articles directly to Publications Router using SFTP send article metadata as JATS Publishing XML https://jats.nlm.nih.gov/publishing/ and article PDFs (and possibly other files) all zipped into a single package.

Currently Router works specifically with JATS v1.1, but is flexible enough to work with older versions of JATS and also more recent versions of the standard.

Router analyses the the JATS XML and transforms it into its own [internal metadata representation](./api/v3/IncomingNotification.md) which is a JSON data structure.

Below are general rules which govern how JATS metadata is mapped to Router's internal representation.  Certain fields in Router's structure do NOT derive from JATS, these are briefly noted too.  

Router only extracts metadata from JATS `<article><front>` section, which contains within it the `<journal-meta>` and `<article-meta>` sections. (Router does NOT use either the `<article><body>` or `<article><back>` sections.)

In summary, Router will extract metadata from sub-elements within these top level JATS elements:
```
<article>
    <front>
        <journal-meta>
            … source of Journal metadata …
        </journal-meta>
        
        <article-meta>
            … source of Article metadata …
        </article-meta>
    </front>
    …
<\article>
```

NISO provide comprehensive documentation of the [Journal Publishing Tag Library](https://jats.nlm.nih.gov/publishing/tag-library/1.1/).

## Mapping to Router's data model from JATS ##

IMPORTANT: For brevity, in column 2 below, `<journal-meta>` is represented by `<J-M>` and `<article-meta>` by `<A-M>`
| Router Field | JATS Source | Notes |
| ----------- | --------------- | ------|
| event | - | Populated via API |
| provider.agent | - | Populated via API or internal processing |
| provider.ref | - | ditto |
| content.packaging_format | - | ditto |
| links.type | - | ditto |
| links.format | - | ditto |
| links.url | - | ditto |
| metadata.journal.title | `<J-M><journal-title-group><journal-title>` |  |
| metadata.journal.abbrev_title | `<J-M><journal-title-group><abbrev-journal-title>` |  |
| metadata.journal.volume | `<A-M><volume>` |  |
| metadata.journal.issue | `<A-M><issue>` |  |
| metadata.journal.publisher | `<J-M><publisher><pubisher-name>` |  |
| metadata.journal.identifier.type<br>metadata.journal.identifier.id | `<J-M><issn pubication-format="…">`<br>or<br>`<J-M><issn pub-type="…">`  | `type` will be set to one of "issn", "eissn", "pissn" depending on the value found in `publication-format` or `pub-type` attribute of `<issn>` element, which may be one of "electronic", "online-only", "print", "epub", "ppub". `id` is set to the value of the `<issn>` element. |
| metadata.article.title | `<A-M><title-group><article-title>` |  |
| metadata.article.subtitle | `<A-M><title-group><subtitle>` |  |
| metadata.article.type |  |  |
| metadata.article.version |  |  |
| metadata.article.start_page |  |  |
| metadata.article.end_page |  |  |
| metadata.article.page_range |  |  |
| metadata.article.num_pages |  |  |
| metadata.article.language |  |  |
| metadata.article.abstract | `<A-M><abstract>` | `<abstract>` elements may have an `abstract-type` attribute indicating its type, Router attempts to choose an abstract without a type, otherwise it uses the first of these types that it finds 'summary', 'web-summary', 'executive-summary' |
| metadata.article.identifier.type |  |  |
| metadata.article.identifier.id |  |  |
| metadata.article.subject |  |  |
| metadata.author.type |  |  |
| metadata.author.name.firstname |  |  |
| metadata.author.name.surname |  |  |
| metadata.author.name.fullname |  |  |
| metadata.author.name.suffix |  |  |
| metadata.author.organisation_name |  |  |
| metadata.author.identifier.type |  |  |
| metadata.author.identifier.id |  |  |
| metadata.author.affiliation |  |  |
| metadata.contributor.type |  |  |
| metadata.contributor.name.firstname |  |  |
| metadata.contributor.name.surname |  |  |
| metadata.contributor.name.fullname |  |  |
| metadata.contributor.name.suffix |  |  |
| metadata.contributor.organisation_name |  |  |
| metadata.contributor.identifier.type |  |  |
| metadata.contributor.identifier.id |  |  |
| metadata.contributor.affiliation |  |  |
| metadata.accepted_date |  |  |
| metadata.publication_date. publication_format |  |  |
| metadata.publication_date.date |  |  |
| metadata.publication_date.year |  |  |
| metadata.publication_date.month |  |  |
| metadata.publication_date.day |  |  |
| metadata.publication_date.season |  |  |
| metadata.history_date.date_type |  |  |
| metadata.history_date.date |  |  |
| metadata.publication_status |  |  |
| metadata.funding.name |  |  |
| metadata.funding.identifier.type |  |  |
| metadata.funding.identifier.id |  |  |
| metadata.funding.grant_numbers |  |  |
| metadata.embargo.start |  |  |
| metadata.embargo.end |  |  |
| metadata.embargo.duration |  |  |
| metadata.license_ref.title |  |  |
| metadata.license_ref.type |  |  |
| metadata.license_ref.url |  |  |
| metadata.license_ref.version |  |  |
| metadata.license_ref.start |  |  |
| | column.spacer.column.spacer.column.spacer.column | |
