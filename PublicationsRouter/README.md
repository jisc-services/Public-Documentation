# Publications Router #

Jisc Publications Router is one of Jisc's Open Access services.  It is concerned with the automated distribution (routing) of Open Access articles and associated metadata to the repositories of UK institutions that have signed up to the service.  Some of these articles (either AM or Vor) come direct from publishers that have agreed to work with Publications Router, others are harvested from sources such as EPMC, Crossref, PubMed.

Publishers that send their articles and/or associated metadata directly to Publications Router do so by one of these mechanisms:
* submission of a zipfile package containing JATS metadata and article PDF (and possibly other files) to Router's FTP server via SFTP 
* using Router's API.

The pages available in this repository describe the services provided by Publications Router, including:

* [RESTful API](./api/README.md) - which enables publishers to submit articles and metadata, and institutions to retrieve the same
* [SWORD-IN](./sword-in/README.md) - which enables publishers to submit articles and metadata via Router's SWORDv2 interface
* [Router's use of JATS](./JATS/README.md) - provides information useful to Publishers (particularly those using Router's auto-test service)
* [XML structures used by Router to send data to Repositories via SWORD ](./sword-out)
* [Router release history](./Release_History.md)

Further information, including a technical overview, is available on the Publications Router website https://pubrouter.jisc.ac.uk/.
