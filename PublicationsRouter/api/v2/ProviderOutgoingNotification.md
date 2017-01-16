# ProviderOutgoingNotification

The JSON structure of the model is as follows:

```json
{
    "analysis_date": "2015-12-01T17:26:40Z", 
    "content": {
        "packaging_format": "string"
    }, 
    "created_date": "2015-12-01T17:26:40Z", 
    "embargo": {
        "duration": 0, 
        "end": "2015-12-01T17:26:40Z", 
        "start": "2015-12-01T17:26:40Z"
    }, 
    "event": "string", 
    "id": "string", 
    "links": [
        {
            "format": "string", 
            "packaging": "string", 
            "type": "string", 
            "url": "string"
        }
    ], 
    "metadata": {
        "author": [
            {
                "affiliation": "string", 
                "identifier": [
                    {
                        "id": "string", 
                        "type": "string"
                    }
                ], 
                "name": "string"
            }
        ], 
        "date_accepted": "2015-12-01T17:26:40Z", 
        "date_submitted": "2015-12-01T17:26:40Z", 
        "identifier": [
            {
                "id": "string", 
                "type": "string"
            }
        ], 
        "language": "string", 
        "license_ref": {
            "title": "string", 
            "type": "string", 
            "url": "string", 
            "version": "string"
        }, 
        "project": [
            {
                "grant_number": "string", 
                "identifier": [
                    {
                        "id": "string", 
                        "type": "string"
                    }
                ], 
                "name": "string"
            }
        ], 
        "publication_date": "2015-12-01T17:26:40Z", 
        "publisher": "string", 
        "source": {
            "identifier": [
                {
                    "id": "string", 
                    "type": "string"
                }
            ], 
            "name": "string"
        }, 
        "subject": [
            "string"
        ], 
        "title": "string", 
        "type": "string", 
        "version": "string"
    }, 
    "provider": {
        "agent": "string", 
        "id": "string", 
        "ref": "string", 
        "route": "string"
    }
}
```

Each of the fields is defined as laid out in the table below.  All fields are optional unless otherwise specified:

| Field | Description | Datatype | Format | Allowed Values |
| ----- | ----------- | -------- | ------ | -------------- |
| analysis_date | Date the routing analysis took place | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| content.packaging_format | Package format identifier for the associated binary content | unicode |  |  |
| created_date | Date this record was created | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| embargo.duration | Duration (in months) of the embargo | int |  |  |
| embargo.end | End date for the embargo.  If this field is populated, this is the definitive information on the end-date of the embargo, and embargo.duration and embargo.start can be ignored. | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| embargo.start | Start date for the embargo | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| event | Keyword for this kind of notification - no restrictions on use in this version of the system | unicode |  |  |
| id | opaque, persistent system identifier for this record | unicode |  |  |
| links.format | mimetype of the resource available at the URL (e.g. text/html) | unicode |  |  |
| links.packaging | Package format identifier for the resource available at the URL | unicode |  |  |
| links.type | keyword for type of resource (e.g. splash, fulltext) - no restrictions on use in this version of the system | unicode |  |  |
| links.url | URL to the associated resource.  All URLs provided by publishers should be publicly accessible for a minimum of 3 months from notification; URLs provided by the Router will be accessible to authenticated users for the same period. | unicode | URL |  |
| metadata.author.affiliation | Author organisational affiliation | unicode |  |  |
| metadata.author.identifier.id | Author identifier (e.g. an ORCID) | unicode |  |  |
| metadata.author.identifier.type | Type of author identifier (e.g. "orcid") - no vocabulary for this field in this version of the system | unicode |  |  |
| metadata.author.name | Author's name in full | unicode |  |  |
| metadata.date_accepted | Date publication accepted for publication | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| metadata.date_submitted | Date article submitted for publication | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| metadata.identifier.id | Identifier for the article (e.g. DOI) | unicode |  |  |
| metadata.identifier.type | Identifier type (e.g. "doi") - no vocabulary for this field in this version of the system | unicode |  |  |
| metadata.language | Language of the article | unicode | 3 letter ISO language code |  |
| metadata.license_ref.title | Title or name of the licence applied to the article; free-text | unicode |  |  |
| metadata.license_ref.type | Type of licence (most likely the same as the title); free-text | unicode |  |  |
| metadata.license_ref.url | URL for information on the licence | unicode | URL |  |
| metadata.license_ref.version | Version of the licence | unicode |  |  |
| metadata.project.grant_number | Grant number for funding source behind this article | unicode |  |  |
| metadata.project.identifier.id | Funder identifier (e.g. Ringold ID) | unicode |  |  |
| metadata.project.identifier.type | Funder identifier type (e.g "ringold") - no vocabulary for this field in this version of the system | unicode |  |  |
| metadata.project.name | Funder name | unicode |  |  |
| metadata.publication_date | Date of publication | unicode | UTC ISO formatted date: YYYY-MM-DDTHH:MM:SSZ |  |
| metadata.publisher | Publisher of the article | unicode |  |  |
| metadata.source.identifier.id | Identifier for the source of the publication (the journal), (e.g. the ISSN) | unicode |  |  |
| metadata.source.identifier.type | Identifier type (e.g. "issn") - no vocabulary for this field in this version of the system | unicode |  |  |
| metadata.source.name | Name of the source (e.g. the Journal Name) | unicode |  |  |
| metadata.subject | Keywords | unicode |  |  |
| metadata.title | Title of the publication | unicode |  |  |
| metadata.type | Type of publication | unicode |  |  |
| metadata.version | Version of publication (e.g. AAM) | unicode |  |  |
| provider.agent | Free-text field for identifying the API client used to create the notification | unicode |  |  |
| provider.id | Identifier for the provider of the notification (account name) | unicode |  |  |
| provider.ref | Publisher's own identifier for the notification - free-text | unicode |  |  |
| provider.route |  | unicode |  |  |
