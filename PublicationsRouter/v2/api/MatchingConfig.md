# Matching Config Structure
Document contains structure of matching configuration JSON for both submission and retrieval. Used in POST/GET requests respectively against endpoint `/config`.

## JSON Submitted Structure
The structure of a JSON submission for matching configuration must match the following format: 
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
## JSON Retrieved Structure
The structure of JSON returned after a GET request will match the following format: 
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
	],
  "id": "the id of this repository configuration object (used internally, not of use externally)",
  "repository": "id of the repository retrieved",
  "created_date": "date this repository configuration object was created",
  "last_updated": "date this repository was last updated"
}
```
  
## CSV Structure
For more information regarding CSV structure see [here for a csv template](http://pubrouter.jisc.ac.uk/static/csvtemplate.csv) or [here for an excel template](https://pubrouter.jisc.ac.uk/static/csvtemplate_router_matching_params_XLS_FORMAT.xlsx).
