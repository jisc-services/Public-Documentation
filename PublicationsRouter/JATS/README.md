# JATS Metadata #

Publishers that submit their articles directly to Publications Router using SFTP send article metadata as JATS Publishing XML https://jats.nlm.nih.gov/publishing/ and article PDFs (and possibly other files) all zipped into a single package.

Currently Router works specifically with JATS v1.2, but is flexible enough to work with older versions of JATS and also more recent versions of the standard.

Router analyses the the JATS XML and transforms it into its own [internal metadata representation](../api/v4/IncomingNotification.md) which is a JSON data structure.

Below are general rules which govern how JATS metadata is mapped to Router's internal representation.  Certain fields in Router's structure do NOT derive from JATS, these are briefly noted too.  

Router extracts metadata from JATS `<article><front>`, containing within it the `<journal-meta>` and `<article-meta>`,  and `<article><back>` sections. (It does NOT use the `<article><body>`section.)

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
    
    <body>… NOT USED …</body>
    
    <back>
        <ack>… Acknowledgements …</ack>
    </back>
<\article>
```

NISO provide comprehensive documentation of the [Journal Publishing Tag Library](https://jats.nlm.nih.gov/publishing/tag-library/1.2).

## Mapping to Router's data model from JATS ##

**IMPORTANT**: In column 2 below, for brevity:
* **`<J-M>`** means `<article><front><journal-meta>`
* **`<A-M>`** means `<article><front><article-meta>`.

| Router Field | JATS Source | Notes |
| ----------- | --------------- | ------|
| event | - | Populated via API. |
| provider.agent | - | Populated via API or internal processing. |
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
| metadata.journal.identifier.type<br><br>metadata.journal.identifier.id | `<J-M><issn pubication-format="…">`<br>or<br>`<J-M><issn pub-type="…">`  | `type` will be set to one of "issn", "eissn", "pissn" depending on the value found in `publication-format` or `pub-type` attribute of `<issn>` element, which may be one of "electronic", "online-only", "print", "epub", "ppub". `id` is set to the value of the `<issn>` element. |
| metadata.article.title | `<A-M><title-group><article-title>` |  |
| metadata.article.subtitle | `<A-M><title-group><subtitle>` |  |
| metadata.article.type | `<article article-type="…">` | The `article-type` attribute of the root `<article>` element is the source. |
| metadata.article.version | `<A-M><article-version>`<br>or<br>`<article specific-use="…">`<br>or<br>`<A-M><permissions><license specific-use="…">`<br>or<br>`<A-M><permissions><ali:license_ref specific-use="…">` | Article version is preferably obtained from the `<A-M><article-version>` element (available since JATS v1.2), or failing that from `article specific-use` attribute, or otherwise from the `specific-use` attribute of most appropriate licence found (usually open-licence).|
| metadata.article.start_page | `<A-M><fpage>` |  |
| metadata.article.end_page | `<A-M><lpage>` |  |
| metadata.article.page_range | `<A-M><page-range>` | If not found then it will be determined from start_page and end_page if they are provided. |
| metadata.article.e_num | `<A-M><elocation-id>` |  |
| metadata.article.num_pages | Calculated | Derived value: (end_page - start_page + 1). |
| metadata.article.language | `<article xml:lang="…">` | From the `<article>` element `xml:lang` attribute if present, otherwise defaults to 'en'. |
| metadata.article.abstract | `<A-M><abstract>` | `<abstract abstract-type="…">` elements may have an `abstract-type` attribute indicating its type, Router attempts to choose an abstract without a type, otherwise it uses the first of these types that it finds 'summary', 'web-summary', 'executive-summary'. |
| metadata.article.identifier.type<br><br>metadata.article.identifier.id | `<A-M><article-id pub-id-type="…">` | `type` is derived from `pub-id-type` attribute; `id` is set to the element value. |
| metadata.article.subject | `<A-M><article-categories><subj-group><subject>`<br>and<br>`<A-M><kwd-group><kwd>` | Both sources of data are used. |
| metadata.author.type | `<A-M><contrib-group><contrib contrib-type="…">` | This will be "author" unless a corresponding author is indicated via `<A-M><contrib-group><contrib><xref ref-type="corresp" rid="…x…">` in conjunction with `<A-M><author-notes><corresp id="…x…">` in which case it is set to "corresp".  |
| metadata.author.name.firstname | `<A-M><contrib-group><contrib><name><given-names>` | |
| metadata.author.name.surname | `<A-M><contrib-group><contrib><name><surname>` |  |
| metadata.author.name.fullname | Calculated | Derived from `firstname` and `surname`. |
| metadata.author.name.suffix | `<A-M><contrib-group><contrib><name><suffix>` |  |
| metadata.author.organisation_name | `<A-M><contrib-group><contrib><collab>` | This element will normally only be present if `<name>` is absent. |
| metadata.author.identifier.type<br><br>metadata.author.identifier.id | `<A-M><contrib-group><contrib><contrib-id contrib-id-type="…">` | `type` is set to the `contrib-id-type` attribute value, `id` is set to `<contrib-id>` element value. |
| metadata.author.affiliations | `<A-M><contrib-group><contrib><aff>`<br>or<br> `<A-M><contrib-group><aff>`<br>or via<br>`<A-M><contrib-group><contrib><xref ref-type="aff" rid="…x…">` in conjunction with `<A-M><contrib-group><aff id="…x…">` | Affiliations may be associated directly with an author with an `<aff>` element within the `<contrib>` element, or with a group of authors via an `<aff>` within the `<contrib-group>` element or indirectly via an `<xref ref-type="aff" rid="…">` where the `rid` value references an `id` of a particular affiliation `<aff id="…">` within the `<contrib-group>`. |
| metadata.contributor.type | `<A-M><contrib-group><contrib contrib-type="…">` | This will be set to value of `contrib-type` attribute.  |
| metadata.contributor.name.firstname | `<A-M><contrib-group><contrib><name><given-names>` | |
| metadata.contributor.name.surname | `<A-M><contrib-group><contrib><name><surname>` |  |
| metadata.contributor.name.fullname | Calculated | Derived from `firstname` and `surname`. |
| metadata.contributor.name.suffix | `<A-M><contrib-group><contrib><name><suffix>` |  |
| metadata.contributor.organisation_name | `<A-M><contrib-group><contrib><collab>` | This element will normally only be present if `<name>` is absent. |
| metadata.contributor.identifier.type<br><br>metadata.contributor.identifier.id | `<A-M><contrib-group><contrib><contrib-id contrib-id-type="…">` | `type` is set to the `contrib-id-type` attribute value, `id` is set to `<contrib-id>` element value. |
| metadata.contributor.affiliations | `<A-M><contrib-group><contrib><aff>`<br>or<br> `<A-M><contrib-group><aff>`<br>or via<br>`<A-M><contrib-group><contrib><xref ref-type="aff" rid="…x…">` in conjunction with `<A-M><contrib-group><aff id="…x…">` | Affiliations may be associated directly with an author with an `<aff>` element within the `<contrib>` element, or with a group of authors via an `<aff>` within the `<contrib-group>` element or indirectly via an `<xref ref-type="aff" rid="…">` where the `rid` value references an `id` of a particular affiliation `<aff id="…">` within the `<contrib-group>`. |
| metadata.contributor.affiliations.identifier | `…<aff><institution-wrap><institution-id institution-id-type="…">` | The identifier object is created from `<institution-id>` element. |
| metadata.contributor.affiliations.identifier.type | `…<aff><institution-wrap><institution-id institution-id-type="…">` | The `institution-id-type` attribute value converted to Upper-case is used as the identifier `type`. |
| metadata.contributor.affiliations.identifier.id | `…<aff><institution-wrap><institution-id institution-id-type="…">` | The `institution-id` element value is used as the identifier `id`. |
| metadata.contributor.affiliations.org | `…<aff><institution>` <br>or<br> `…<aff><institution-wrap><institution>` | |
| metadata.contributor.affiliations.dept | `…<aff><institution content-type="*">` <br>or<br> `…<aff><institution-wrap><institution content-type="*">` <br>* - where `content-type` contains one of: "dept", "department", "division", "office" (E.g. `content-type="org-division"` or `content-type="office"`)| |
| metadata.contributor.affiliations.street | `…<aff><addr-line>` | |
| metadata.contributor.affiliations.city | `…<aff><city>` <br>or<br> `…<aff><addr-line content-type="city">` | |
| metadata.contributor.affiliations.state | `…<aff><state>` <br>or<br> `…<aff><addr-line content-type="state">` | |
| metadata.contributor.affiliations.postcode | `…<aff><postal-code>` <br>or<br> `…<aff><addr-line content-type="postal-code">` <br>or<br> `…<aff><addr-line content-type="postcode">` | |
| metadata.contributor.affiliations.country | `…<aff><country>` <br>or<br> `…<aff><addr-line content-type="country">` | |
| metadata.contributor.affiliations.country_code | `…<aff><country country_code="XX">` | |
| metadata.contributor.affiliations.raw | `…<aff>` | This field will contain the text (excluding any subsidiary XML tags) found in the `<aff>` element |
| metadata.accepted_date | `<A-M><history><date date-type="accepted">`<br>or<br>`<A-M><history><date pub-type="…x…">` where `…x…` is either "am" or "aam" |  The second option is for support of older versions of JATS. |
| metadata.publication_date. publication_format | `<A-M><pub-date publication-format="…">` | The `publication-format` attribute value is used. |
| metadata.publication_date.date | Calculated | Derived from `year`, `month`, `day` values - see below. |
| metadata.publication_date.year | `<A-M><pub-date><year>` |  |
| metadata.publication_date.month | `<A-M><pub-date><month>` |  |
| metadata.publication_date.day | `<A-M><pub-date><day>` |  |
| metadata.publication_date.season | `<A-M><pub-date><season>` |  |
| metadata.history_date.date_type | `<A-M><history><date date-type="…">` | The `date-type` attribute value is used. |
| metadata.history_date.date | Derived from:<br>`<A-M><history><date><year>`<br>`<A-M><history><date><month>`<br>`<A-M><history><date><day>` | The values are concatenated into date with format `YYYY-MM-DD`. |
| metadata.publication_status | Calculated | Set to "Published" if a publication date exists, otherwise set to "Accepted" if acceptance date exists. |
| metadata.funding.name | `<A-M><funding-group><award-group><funding-source>`<br>or<br>`<A-M><funding-group><award-group><funding-source><named-content content-type="…name…">` | In the last case, the value of any `<named-content>` with "name" somewhere within its `content-type` attribute is used; e.g. if `content-type` was "funder-name".   |
| metadata.funding.identifier.type | `<A-M><funding-group><award-group><funding-source><institution-id institution-id-type="…">`<br>or derived from<br>`<A-M><funding-group><award-group><funding-source href="…">`<br>or derived from<br>`<A-M><funding-group><award-group><funding-source><named-content content-type="funder-id">` | In the first case the value of `institution-id-type` attribute is used, in the last 2 cases `type` is set to "doi" if "doi.org" is found within the identifier string otherwise it is set to "Id". |
| metadata.funding.identifier.id | `<A-M><funding-group><award-group><funding-source><institution-id>`<br>or<br>`<A-M><funding-group><award-group><funding-source href="…">`<br>or<br>`<A-M><funding-group><award-group><funding-source><named-content content-type="funder-id">` | First option is preferred. |
| metadata.funding.grant_numbers | `<A-M><funding-group><award-group><award-id>` | There can be multiple `<award-id>` within an `<award-group>`.  |
| metadata.embargo.start | - | May be set via publisher default setting - see footnote. |
| metadata.embargo.end | Calculated | If an open licence with a start-date has been provided then this is used as the embargo end-date; or may be set via publisher default setting - see footnote.  |
| metadata.embargo.duration | - | May be set via publisher default setting - see footnote. |
| metadata.license_ref.title | `<A-M><permissions><license><license-p>`<br>or<br>`<A-M><permissions><license><p>` | Optional / may be set via publisher default setting - see footnote. |
| metadata.license_ref.type | `<A-M><permissions><license><license-type>` | Optional / may be set via publisher default setting - see footnote. |
| metadata.license_ref.url | `<A-M><permissions><license><ali:license_ref>`<br>or<br>`<A-M><permissions><license href="…url…">`<br>or<br>`<A-M><permissions><license><license-p><ext-link href="…url…">` | Use of `<ali:license_ref>` is strongly preferred.<br>In the last case, if `<license-p>` exists without an `<ext-link>` element, then the paragraph text is searched for a URL which, if found, is assumed to be the licence URL.<br>Or may be set via publisher default setting - see footnote. |
| metadata.license_ref.version | - | May be populated via publisher default setting - see footnote. |
| metadata.license_ref.start | `<A-M><permissions><license><ali:license_ref start_date="…">` | The `start_date` attribute value is used or may be set via publisher default setting - see footnote. |
| metadata.ack | `<article><back><ack>` |  |
| | ………………………………………………………………………………… | |

### Footnote: Default post-embargo licence and embargo period
Publishers may set default values for Embargo duration and Licence via their Publications Router account which will be used where the JATS metadata contains no licence information.

#### Default licence
If present, the default licence information will be applied whenever the JATS metadata lacks any licensing metadata. (In other words, if an article already has a licence specified in its metadata then that is used; otherwise the default is applied.)  The licence start-date will be set to the embargo end-date where that is available (see below).

#### Default embargo duration
If specified, this will be used when no licence is specified in the article's metadata and the publisher default licence is applied and the article metadata includes a publication date.  In this case the publication date will be used as the embargo start-date, and the embargo end-date and duration will be set from the default duration.
