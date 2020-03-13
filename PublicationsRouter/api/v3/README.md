# Jisc Publications Router API (Publications Router API)

Jisc Publications Router exposes an REST API that may be used to submit and/or retrieve publication notifications. These pages document the API interfaces and data formats.

The current version of the API is v3, and it can be accessed at

    https://pubrouter.jisc.ac.uk/api/v3

All URL paths in this documentation will extend from this base url.

In many cases you will need an API key to access the API.  This can be obtained from your Publications Router account page.

**[Swagger documentation](https://jisc-services.github.io/Public-Documentation/)** (which also enables you to try out the API) is available.

### Contents ###

* [API for sending notifications](./Send.md) (Publishers)
* [API for retrieving notifications](./Retrieve.md) (CRIS and Repositories)
* [API for changing matching configuration](./Config.md) (CRIS and Repositories)

### Alternatives to REST API ###

These pages describe Publications Router's REST API, however there are other means by which Publishers may send information to Publications Router (SWORD2 or FTP); and by which Repositories may receive information (OAI-PMH and SWORD2).  More information on these is available on the  Publications Router website [introduction](https://pubrouter.jisc.ac.uk/about/) and [technical overview](https://pubrouter.jisc.ac.uk/about/resources/).

---
