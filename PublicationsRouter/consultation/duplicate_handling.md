# Duplicate handling consultation

Following Router's introduction of basic duplicate handling options (in June 2021) we are considering adding additional functionality:

1. Sending duplicate notifications by email (instead of into your repository) if you provide one (or more) email addresses for that purpose (there would be a new *Duplicates by Email* field on your Router account page)
2. More sophisticated duplicates filter mechanism which would provide additional options listed below.

### Current duplicate filter options:
1. No duplicates from secondary sources (Crossref, PubMed, EMPC)
2. Secondary source duplicates only if they contain additional metadata 
3. All duplicates)

(Currently duplicates direct from publishers will always be sent).

### Proposed duplicate filter options
0. No duplicates from any source (including publishers)
1. Duplicates direct from Publisher (none from secondary sources)
2. As (1) or where notification has an increased number of any of the following metadata (Authors, Author-ORCIDS, Funders, Funder-IDs, Grant-numbers, Licences) 
3. As (2) or with additional **high** ranked metadata fields or a PDF article (where none previously supplied)
4. As (3) or with additional **medium** ranked metadata fields
5. As (4) or with additional **low** ranked metadata fields
6. All duplicates.

**Notes:**

* Options from 1 onwards are all *additive*: they include the criteria of all preceding options in addition to those described 
* Options 2 to 6 effectively apply only to secondary sources, because they inherit option 1, which is passing ALL duplicates direct from a publisher. 
* For options 3 to 5, we only consider the presence of new metadata fields, we do NOT analyse for changes in existing metadata fields. E.g. if an Abstract had previously been supplied, and in a later notification it is revised, this would not be noticed.

### Comparison of current and proposed options
Proposed options 0, 2, 3 and 4 would be new.

Proposed options 1, 5 and 6 correspond to existing options: 
* Proposed option 1 <=> Current option 1
* Proposed option 5 <=> Current option 2
* Proposed option 6 <=> Current option 3.


## Ranking of metadata fields
The proposed options introduce the concept of **metadata rank**, which is intended to allow differentiation between metadata based on its "value" to your repository entries.

**This is a key area for consultation with you.**

Below is our *straw-man* draft which we would like you to comment on and propose changes.

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
We would welcome your comments on the following topics:
* Receiving duplicates by email (instead of into your repository)
* Proposed duplicate filter options - are they useful? what alternatives would you prefer?
* Proposed rank - Do you agree with the suggesed ranking? How would you rank the metadata fields differently?

We would prefer you to provide feedback by adding a new Issue or by commenting on or reacting to an existing issue on this page: **https://github.com/jisc-services/Public-Documentation/issues** as this will then be visible to all interested parties, who can also comment or "vote" by clicking a reaction icon.   You will need to create a GitHub account to do this.