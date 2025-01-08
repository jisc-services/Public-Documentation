# Matching Parameters
The main purpose of Publications Router is to forward relevant articles to institutional repositories or CRISs. Each institution defines a set of ***Matching Parameters*** which are used by Publications Router during its analysis of article metadata (author affiliations and grant information) to decide whether an article is relevant to a particular insitution.  

Two methods are provided:
* GET - to retrieve current matching parameters
* POST - to set new matching parameters from a supplied JSON data structure or a CSV file.

## GET `/config?api_key=...`

Your repostiory account API-KEY must be provided. 

### Responses to GET

The `GET /config` endpoint will return one of these responses.

* **200 - Success**: if the request is successful then a HTTP **200 OK** code is provided with the JSON response body shown:

```
HTTP 1.1  200 OK
Content-Type: application/json

{
    "name_variants": [ "list of name variants the institution is known by" ],
    "org_ids": [ "list of Organisation identifiers of form: 'TYPE:VALUE'" ],
    "postcodes" : [ "list of postcodes where authors may list their affiliation address" ],
    "domains": [ "list of email or website domain names the institution owns or operates under" ],
    "grants": [ "list of grant numbers affiliated with the institution" ],
    "orcids": [ "list of ORCIDs affiliated with the institution" ],
    "emails": [ "list of affiliated researchers private emails (not using institution domain)"],
    "id": "the id of this repository configuration object (used internally, not of use externally)",
    "repository": "id of the repository retrieved",
    "created_date": "date this repository configuration object was created",
    "last_updated": "date this repository was last updated"
}
```

Successful response example: 
```json
{
    "name_variants": ["Oxford University", "University of Oxford"],
    "org_ids": [ "ROR:052gg0110", "ISNI:0000000419368948", "GRID:grid.4991.5"],
    "postcodes": ["BS1 1SB", "LS2 2SL"],
    "domains": ["ox.ac.uk"],
    "grants": ["yyybbb-123", "abcde-321"],
    "orcids": ["0000-0001-2345-6789"],
    "emails": ["some.one@gmail.com"],
    "id": "123456789",
    "repository": "987654321",
    "created_date": "2016-11-23T14:57:00Z",
    "last_updated": "2018-06-04T13:04:23Z"
}
```

&nbsp;
* **401 - authentication failure**: For invalid api_key or other problems authenticating.

```
HTTP 1.1  401 Unauthorized
```
&nbsp;
* **400 - malformed request**: Where the request is malformed in some way:

```
HTTP 1.1  400 Bad Request
Content-Type: application/json
    {
        "error" : "Human readable error message."
    }
```
&nbsp;
* **403 - forbidden**: Where an authenticated user as an invalid role (for example is a publisher) or account is turned off:

```
HTTP 1.1  403 Forbidden
Content-Type: application/json
    {
        "error" : "Only an admin or repository user can access this endpoint."
    }
```
&nbsp;
&nbsp;
## POST `/config?api_key=...`

The matching parameters to set are sent using POST either as a JSON data package or as a CSV file.

When parameters are loaded surplus white-space is removed, as are duplicate entries and redundant parameters (e.g. where both a domain and its sub-domain are specified, then the sub-domain will be removed).

### JSON data structure

To set ALL matching parameters, a data structure like this must be supplied.  See more detail [here](Config.md#json-submission).
```json
{
    "name_variants": [ "list of name variants the institution is known by" ],
    "org_ids": [ "list of Organisation identifiers as 'TYPE: VALUE' strings or URLs.  The following types are accepted: 'ROR', 'GRID', 'ISNI' (or 'ISN'), 'CROSSREF', 'RINGGOLD' (or 'RIN').  Identifiers supplied in URL format are converted and stored as 'Type:Value' format." ],
    "postcodes": [ "list of postcodes where authors may list their affiliation address" ],
    "domains": [ "list of domain names the institution owns or operates under" ],
    "grants": [ "list of grant numbers affiliated with the institution"] ,
    "orcids": [ "list of ORCIDs affiliated with the institution" ],
    "emails": [ "list of affiliated researchers private emails (not using institution domain)"],
}
```  

However, you may set individual (or several) matching parameters by supplying just those of interest.  If you provide an empty array, then all matching parameters of the specified type will be removed.  

For example, POSTing the following JSON would:
* replace existing ORCIDs with the 3 specified
* replace 4 Organisation IDs with those specified (note that "https://ror.org/0524sp257" and "ROR: 0524sp257" are duplicates, so only one would be retained)
* and would remove all existing emails.
```json
{
    "orcids": ["0000-0002-8567-3333", "0000-0002-1841-4346", "0000-0002-9377-555X"],
    "org_ids": ["  ISN: 0001 0002 0003 0004", "https://ror.org/0524sp257", "ROR: 0524sp257", "GRID: grid.5337.2", "https://api.crossref.org/funders/501100000883"]
    "emails": []
}
```
Also, see the GET example above. 

When the above data is retrieve, the Organisation Identifiers will be presented as:
```json
{
    "org_ids": ["ISNI:0001000200030004", "ROR:0524sp257", "GRID:grid.5337.2", "CROSSREF:501100000883"]
}
```

### CSV File
A CSV file (with filename ending in `.csv`) must be provided with the POST request - see more detail [here](Config.md#csv-submission).

The file format must correspond to that shown in this [csv template](http://pubrouter.jisc.ac.uk/static/csvtemplate.csv), which is also shown in this [Excel template](https://pubrouter.jisc.ac.uk/static/csvtemplate_router_matching_params_XLS_FORMAT.xlsx) which incorporates explanatory notes.

### Responses to POST

The `POST /config` endpoint will return one of these responses.

* **204 - No Content**: The request was **successfully** processed.  (No content is returned).

&nbsp;
* **401 - authentication failure**: For invalid api_key or other problems authenticating.

```
HTTP 1.1  401 Unauthorized
```
&nbsp;
* **400 - malformed request**: Where the request is malformed in some way:

```
HTTP 1.1  400 Bad Request
Content-Type: application/json
    {
        "error" : "Human readable error message."
    }
```
&nbsp;
* **403 - forbidden**: Where an authenticated user as an invalid role (for example is a publisher):

```
HTTP 1.1  403 Forbidden
Content-Type: application/json
    {
        "error" : "Only an admin or repository user can access this endpoint."
    }
```

