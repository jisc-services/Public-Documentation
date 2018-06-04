# Matching Config Structure

The matching configuration of an institution determines whether or not a notification will be matched to that institution's repository. The matching configuration may be uploaded with either a JSON format or alternatively using a csv file attachment as part of the request. 

## JSON Structure
The structure of a JSON submission for matching configuration must match the following format: 
```JSON
{
  "name_variants": ["list of name variants the institution is known by"],
  "postcodes" : ["list of postcodes where authors may list their affiliation address"],
  "domains": ["list of domain names the institution owns or operates under"],
  "grants": ["list of grant numbers affiliated with the institution"],
  "author_ids": [
                  {
                     "type": "the type of identifier, either 'email' or 'ORCID'",
                     "id": "the value, so either the email address or the ORCID"
                  }
                ]
}
```

## CSV Structure
For more information regarding CSV structure see [here for a csv template](http://pubrouter.jisc.ac.uk/static/csvtemplate.csv) or [here for an excel template](https://pubrouter.jisc.ac.uk/static/csvtemplate_router_matching_params_XLS_FORMAT.xlsx).
  
