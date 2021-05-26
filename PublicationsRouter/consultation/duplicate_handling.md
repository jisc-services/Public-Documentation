# Duplicate handling consultation

Following Router's introduction of basic duplicate handling options (in May 2021) we are considering adding a more sophisticated filter mechanism which provides additional granularity.

### Current options:
1. No duplicates from secondary sources (Crossref, PubMed, EMPC)
2. Secondary source duplicates only if they contain additional metadata 
3. All duplicates)

(Currently duplicates direct from publishers will always be sent).

### Proposed options
0. No duplicates from any source (including publishers)
1. Duplicates direct from Publisher (none from secondary sources)
2. As (1) or where notification has an increased number of any of the following (Authors, Author-ORCIDS, Funders, Funder-IDs, Grant-numbers, Licences) 
3. As (2) or with additional **high** ranked metadata fields or a PDF article (where none previously supplied)
4. As (3) or with additional **medium** ranked metadata fields
5. As (4) or with additional **low** ranked metadata fields
6. All duplicates.

**Notes:**

* Options from 1 onwards are all *additive*: they include the criteria of all preceding options in addition to those described 
* Options 2 to 6 effectively apply only to secondary sources, because they inherit option 1, which is passing ALL duplicates direct from a publisher. 
* For options 3 to 5, we only consider presence of new metadata fields, we do NOT analyse for changes in existing metadata fields. E.g. if an Abstract had previously been supplied, and in a later notification it is revised, this would not be noticed.

### Comparison of current and proposed options
Current option 1 corresponds to proposed option 1.

Current option 2 corresponds to proposed option 5.

Current option 3 corresponds to proposed option 6.


## Ranking of metadata fields
The proposed options introduce the concept of **metadata rank**, which is intended to allow differentiation between metadata based on its "value" to your repository entries.

**This is a key area for consultation with you.**

Below is our "straw-man" draft which we would like you to comment on and propose changes.

| Metadata Field | Proposed Rank | Notes |
| ----- | ----------- | -------- |
| Journal title | High |  |
| Journal abbreviated title | Low |  |
| Journal volume | Medium |  |
| Journal issue | Medium |  |
| Publisher name | High |  |
| Journal identifier (e.g. ISSN) | Medium |  |
| Article title | High |  |
| Article subtitle | Low |  |
| Article type (e.g research, review) | Low |  |
| Article version (e.g. AM, VoR) | High |  |
| Article start-page | Medium |  |
| Article end-page | Medium |  |
| Article page-range | Medium |  |
| Article num-pages | Medium |  |
| Article language | Low | This is invariably 'En' - English |
| Article abstract | High |  |
| Article identifier (e.g. PubMed Id) | Low | Note that articles ALWAYS have a DOI |
| Article subject | Low |  |
| Author | High |  |
| Contributor (e.g. editor) | Low |  |
| Accepted date | High |  |
| Publication date | High |  |
| History date(s) | Low |  |
| Funding (including funder-IDs and grant-numbers | High |  |
| Embargo | High |  |
| Licence | High |  |

## Consultation Feedback
We would welcome any comments on the following topics:
* Proposed options - are they useful? what alternatives would you prefer?
* Proposed rank - how would you rank the metadata fields?

We would prefer you to provide feedback by adding a new Issue on this page: https://github.com/jisc-services/Public-Documentation/issues as this will then be visible to all interested parties.  Note that you will need to create a GitHub account to do this.