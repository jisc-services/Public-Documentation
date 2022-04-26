# Jisc Publications Router API v4 (2022)
 
Jisc Publications Router exposes an REST API that may be used to submit and/or retrieve publication notifications. These pages document the API interfaces and data formats.

The current version of the API is v4, and it can be accessed at

    https://pubrouter.jisc.ac.uk/api/v4

All URL paths in this documentation will extend from this base url.

In all cases you will need an API key to access the API.  This can be obtained from your Publications Router account page.

**[Swagger documentation](https://jisc-services.github.io/Public-Documentation/)** (which also enables you to try out the API) is available.

## Contents

* [API for sending notifications](./Send.md) (Publishers)
* [API for retrieving notifications](./Retrieve.md) (CRIS and Repositories)
* [API for changing matching configuration](./Config.md) (CRIS and Repositories)

## Alternatives to REST API

These pages describe Publications Router's REST API, however there are other means by which Publishers may send information to Publications Router (SWORD2 or FTP); and by which Repositories may receive information (OAI-PMH and SWORD2).  More information on these is available on the  Publications Router website [introduction](https://pubrouter.jisc.ac.uk/about/) and [technical overview](https://pubrouter.jisc.ac.uk/about/resources/).

## API Release History

### v4 (2022)

1. Change to Incoming and Outgoing notification JSON structures: 
   * New metadata fields:
     * *created* - Created date (replaces *created_date*)
     * *metadata.article.e_num* - Electronic article number (aka e-location id), typically used in place of page numbers for online articles
     * *metadata.ack* - Acknowledgements text
     * *metadata.peer_reviewed* - Boolean indicating whether article has been peer reviewed or not
     * *metadata.author.affiliations* - Array of affiliation objects (replaces *metadata.author.affilation* string)
     * *metadata.contributor.affiliations* - Array of affiliation objects (replaces *metadata.contributor.affilation* string)

   * Deleted metadata fields:
     * *created_date* - replaced by *created*)
     * *metadata.author.affiliation* - replaced by *metadata.author.affiliations*
     * *metadata.contributor.affiliation* - replaced by *metadata.contributor.affiliations*

2. Additional (optional) parameter *since_id* for `GET /routed/<repo_id>` endpoint with associated change to returned JSON structure.  This provides for more convenient (and efficient) retrieval of all new notifications since the last notification retrieved.

### v3 (2017)
* Initial version