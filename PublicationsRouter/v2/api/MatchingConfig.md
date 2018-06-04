# Matching Config

The matching configuration of an institution determines whether or not a notification will be matched to that institution's repository. The matching configuration may be uploaded with either a JSON format or alternatively using a csv file attachment as part of the request. 

## JSON Structure
The structure of a JSON submission for matching configuration must match the following format: 
```JSON
{
  "name_variants": ["list of name variants the institution is known by"],
  "postcodes" : ["list of postcodes where authors may list their affiliation address"],
  "domains": ["list of domain names the institution owns or operates under"],
  "grants": ["list of grant numbers affiliated with the institution"],
  "author_ids": [# list of author identifier objects of format
                  {
                     "type": "the type of identifier, either 'email' or 'ORCID'",
                     "id": "the value, so either the email address or the ORCID"
                  }
                ]
}
```
  
