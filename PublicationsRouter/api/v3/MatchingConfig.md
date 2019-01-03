# Matching Parameters
The main purpose of PubRouter is to forward relevant articles to institutional repositories or CRISs. Each institution defines a set of ***Matching Parameters*** which are used by PubRouter during its analysis of article metadata (author affiliations and grant information) to decide whether an article is relevant to a particular insitution.  

# Matching Parameter Structure
The JSON structures used with the GET and POST `/config` API endpoints are described below.

## JSON Structure for GET
The JSON document returned by a GET request will have the following format:
```
{
  "name_variants": ["list of name variants the institution is known by"],
  "postcodes" : ["list of postcodes where authors may list their affiliation address"],
  "domains": ["list of email or website domain names the institution owns or operates under"],
  "grants": ["list of grant numbers affiliated with the institution"],
  "author_ids": [ // list of identifier objects of format
		{
			'type': 'the type of identifier, either email or ORCID', 
			'id': 'the value, so either the email address or the ORCID'
		}
	],
  "id": "the id of this repository configuration object (used internally, not of use externally)",
  "repository": "id of the repository retrieved",
  "created_date": "date this repository configuration object was created",
  "last_updated": "date this repository was last updated"
}
```

Example: 
```JSON
{
	"name_variants": ["Oxford University", "University of Oxford"],
	"postcodes": ["BS1 1SB", "LS2 2SL"],
	"domains": ["ox.ac.uk"],
	"grants": ["yyybbb-123", "abcde-321"],
	"author_ids": [
		{
			"type": "email",
			"id": "some.one@gmail.com"
		},
		{
			"type": "orcid",
			"id": "0000-0001-2345-6789"
		}
	],
	"id": "123456789",
	"repository": "987654321",
    "created_date": "2016-11-23T14:57:00Z",
    "last_updated": "2018-06-04T13:04:23Z"
}
```
## JSON Structure for POST
This JSON structure must be used for submitting a new matching configuration using POST:
```
{
  "name_variants": ["list of name variants the institution is known by"],
  "postcodes" : ["list of postcodes where authors may list their affiliation address"],
  "domains": ["list of domain names the institution owns or operates under"],
  "grants": ["list of grant numbers affiliated with the institution"],
  "author_ids": [ // list of identifier objects of format
		{
			'type': 'the type of identifier, either email or ORCID', 
			'id': 'the value, so either the email address or the ORCID'
		}
	]
}
```  
See example of GET for an idea of how to populate the JSON for POST submissions. 

## CSV Structure
For more information regarding CSV structure see [here for a csv template](http://pubrouter.jisc.ac.uk/static/csvtemplate.csv) or [here for an excel template](https://pubrouter.jisc.ac.uk/static/csvtemplate_router_matching_params_XLS_FORMAT.xlsx).

