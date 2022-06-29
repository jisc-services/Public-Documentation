# Duplicate Differences Bit Mask

The `dup_diffs` array present for all duplicate notifications will contain 1 or 2 objects like that shown below (see the [Outgoing Notification](./OutgoingNotification.md) documentation for full details):
```
"dup_diffs":[
    { 
        "old_date": …,
        "curr_bits": <Long integer: Bit mask which summarises analysis of current notification's metadata>,
        "old_bits": <Long integer: Bit mask which summarises analysis of comparison notification metadata>,
        "n_auth": …,
        "n_orcid": …,
        "n_fund": …,
        "n_fund_id": …,
        "n_grant": …,
        "n_lic": …,
    }
]
```

This object contains 2 **bit masks**:
* `curr_bits`
* `old_bits`.  

In each bit mask, a bit set ON (value 1) indicates that a particular item of metadata is present; while a bit set OFF (value 0) indicates it is absent.  The table below summarises the meaning of each bit.

## Bit descriptions
This table describes the bits that are used and their interpretation.

| Bit number | Bit value | Metadata field dot description | Meaning if bit set ON |
|----|-----|-----|-----|
| 1  | 2 | event | Publishing event metadata field is present |
| 2  | 4 | links - Fulltext | At least one full-text link is present (i.e. an article PDF is available) |
| 3  | 8 | metadata.journal.title | Journal title is present |
| 4  | 16 | metadata.journal.abbrev_title | Journal abbreviated title is present |
| 5  | 32 | metadata.journal.volume | Journal volume is present |
| 6  | 64 | metadata.journal.issue | Journal issue is present |
| 7  | 128 | metadata.journal.publisher | Publisher name is present |
| 8  | 256 | metadata.journal.identifier.id | Publisher identifier is present (1 or more) |
| 9  | 512 | metadata.article.title | Article title is present |
| 10 | 1024 | metadata.article.subtitle | Article subtitle is present |
| 11 | 2048 | metadata.article.type | Article type is present |
| 12 | 4096 | metadata.article.version - AM | Article version with value 'AM' is present |
| 13 | 8192 | metadata.article.version - VoR | Article version with value 'VoR' is present |
| 14 | 16384 | metadata.article.version - EVoR or CVoR | Article version with value 'EVoR' or 'CVoR'  is present |
| 15 | 32768 | metadata.article.start_page | Article page start is present |
| 16 | 65536 | metadata.article.end_page | Article page end is present |
| 17 | 131072 | metadata.article.page_range | Article page range is present |
| 18 | 262144 | metadata.article.num_pages | Article number of pages is present |
| 19 | 524288 | metadata.article.language | Article language is present |
| 20 | 1048576 | metadata.article.abstract | Article abstract is present |
| 21 | 2097152 | metadata.article.identifier - DOI | Article DOI is present (this will always be true) |
| 22 | 4194304 | metadata.article.identifier - Other | Non-DOI article identifier is present |
| 23 | 8388608 | metadata.article.subject | Subject keywords are present (1 or more) |
| 24 | 16777216 | metadata.author - Author | Article authors are present (1 or more) |
| 25 | 33554432 | metadata.author - Corresp | At least 1 corresponding author is present |
| 26 | 67108864 | metadata.author | At least 1 Author ORCID is present |
| 27 | 134217728 | metadata.contributor | Contributors are present (1 or more) |
| 28 | 268435456 | metadata.accepted_date | Accepted date is present |
| 29 | 536870912 | metadata.publication_date.date - Full | Full publication date (YYYY-MM-DD) is present |
| 30 | 1073741824 | metadata.publication_date.date - Partial | Partial publication date (YYYY-MM or YYYY) is present |
| 31 | 2147483648 | metadata.publication_status | Publication status is present |
| 32 | 4294967296 | metadata.history_date | History dates are present (1 or more) |
| 33 | 8589934592 | metadata.funding | Funding information is present (1 or more Funders) |
| 34 | 17179869184 | metadata.funding - Identifier | At least 1 funder has an Identifier |
| 35 | 34359738368 | metadata.funding - Grants | At least 1 funder has Grant numbers |
| 36 | 68719476736 | metadata.embargo | Embargo date is present |
| 37 | 137438953472 | metadata.license_ref - Open | At least 1 Open licence is present. **Including a publisher's proprietary "open licence"** |
| 38 | 274877906944 | metadata.license_ref - Other | At least 1 Other (proprietary non-open) licence is present |
| 39 | 549755813888 | metadata.peer_reviewed | Peer-reviewed boolean indicator is present |
| 40 | 1099511627776 | metadata.ack | An acknowledgements string is present |
| 41 | 2199023255552 | metadata.article.e_num | An electronic article number (e-location id) is present |
