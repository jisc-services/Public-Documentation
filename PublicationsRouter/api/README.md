# Publications Router API #

Jisc Publications Router exposes an REST API that may be used to submit and/or retrieve publication notifications.


There are two active versions of the API available:

* [Version 4](./v4/README.md):  the latest version introduced in Q2 2022
* [Version 3](./v3/README.md):  the earlier version, maintained for backward compatibility. Target end-of-life is June 2023.


## API Release History

### v4 (2022)

1. Additional (optional) parameter ***since_id*** for `GET /routed/<repo_id>` endpoint with associated change to returned JSON structure.  This provides for more convenient (and efficient) retrieval of all new notifications since the last notification retrieved (when used instead of the *since* date parameter).


2. Change to **Incoming notification** JSON structure: 
   * **New** metadata fields:
     * *metadata.**ack*** - Acknowledgements text
     * *metadata.**peer_reviewed*** - Boolean indicating whether article has been peer reviewed or not
     * *metadata.article.**e_num*** - Electronic article number (aka e-location id), typically used in place of page numbers for online articles
     * *metadata.author.**affiliations*** - Array of affiliation objects (replaces *metadata.author.affilation* string)
     * *metadata.contributor.**affiliations*** - Array of affiliation objects (replaces *metadata.contributor.affilation* string)
     
   * **Deprecated** metadata fields:
     * *metadata.author.affiliation* string - has been replaced by *metadata.author.affiliations* array of objects
     * *metadata.contributor.affiliation* string - has been replaced by *metadata.contributor.affiliations* array of objects
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**See [Incoming Notification](./v4/IncomingNotification.md#incoming-notification) description for detailed information on its structure.**
&nbsp;

3. Change to **Outgoing notification** JSON structure: 
   * **Changed** metadata fields:
     * ***id*** - Now a Long integer datatype (not a String)

   * **New** metadata fields:
     * ***created*** - Created date (replaces *created_date*)
     * *metadata.**ack*** - Acknowledgements text
     * *metadata.**peer_reviewed*** - Boolean indicating whether article has been peer reviewed or not
     * *metadata.article.**e_num*** - Electronic article number (aka e-location id), typically used in place of page numbers for online articles
     * *metadata.author.**affiliations*** - Array of affiliation objects (replaces *metadata.author.affilation* string)
     * *metadata.contributor.**affiliations*** - Array of affiliation objects (replaces *metadata.contributor.affilation* string)

   * **Deprecated** metadata fields:
     * *created_date* - replaced by *created*
     * *metadata.author.affiliation* string - has been replaced by *metadata.author.affiliations* array of objects
     * *metadata.contributor.affiliation* string - has been replaced by *metadata.contributor.affiliations* array of objects
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**See [Outgoing Notification](./v4/OutgoingNotification.md#outgoing-notification) description for detailed information on its structure.**
&nbsp;


### v3 (2017)
* Initial version
