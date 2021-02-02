# API for retrieving/updating matching configuration

The current version of the API is v3, and it can be accessed at

    https://pubrouter.jisc.ac.uk/api/v3

All URL paths provided in this document will extend from this base url.

If you have an Institution account (i.e. you are populating a repository or CRIS) you have access to the endpoint `/config` which allows you to retrieve and/or replace your current matching configuration. This endpoint requires authentication using an api key.

Also see the **[API Swagger documentation](https://jisc-services.github.io/Public-Documentation/)** (which also enables you to try out the API).

## Retrieving Matching Configuration

To retrieve your current matching configuration from this endpoint, simply make a GET request against the endpoint with your API key.

    GET /config?api_key=<api_key>

If authentication was successful then you will receive a 200 (OK) and a JSON document containing your current repository configuration:

    HTTP 1.1  200 OK
    Content-Type: application/json

    {
        "matching_configuration": ["values"],
        ...
    }

Otherwise, if authentication was unsucessful you will receive a 401 (Unauthorized) 


    HTTP 1.1  401 Unauthorized


For information regarding the JSON data structure returned, see [matching config structure](./MatchingConfig.md).

## Submitting Matching Configuration

To submit new matching configuration you make a POST request to this endpoint. You may submit the changes either as a JSON data structure or as a CSV file.

Changes made using this request will *replace* existing configuration with the new configuration posted.

### JSON Submission

To submit JSON, make a POST request against the endpoint, with your api key and content-type of `application/JSON`, with valid JSON in the body (see [matching config structure](./MatchingConfig.md#retrieved-structure)).

    POST /config?api_key=<api_key>
    Header:
        Content-Type: application/json
    Body:
        { "Valid JSON matching config structure" }

Possible response codes:
* 204 - successfully changed matching parameters (no content is returned)
* 400 - invalid content (an error string will be returned)
* 401 - unauthenticated (nothing returned)

### CSV Submission

To submit CSV, make a POST request against the endpoint, with your api key and content-type of `multipart/form-data`, with a form submission having key value of `'file'` with the csv filename as the value. For valid csv format see [here for a csv template](http://pubrouter.jisc.ac.uk/static/csvtemplate.csv) or [here for an excel template](https://pubrouter.jisc.ac.uk/static/csvtemplate_router_matching_params_XLS_FORMAT.xlsx).

    POST /config?api_key=<api_key>

    Header:
        Content-Type: multipart/form-data; boundary=FulltextBoundary
    Body:
        --FulltextBoundary

        Content-Disposition: form-data; name="file"; filename="match_config.csv"
        Content-Type: text/csv

If submission was successful you will receive a 204 (No Content). Else, a 400 will be raised upon invalid content submitted, or a 404 if unauthenticated.
