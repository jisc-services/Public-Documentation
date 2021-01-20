# Validation of notifications

The Publications Router validation service, available via [API Validation endpoints](./Send.md#Validation-endpoints) and  file submission via SFTP, analyses the submitted metadata using proprietary rules to determine whether it is adequate.

The result of validation will be:
* Success - zero errors were found, though there may be issues (warnings)
* Failure - one or more errors were found, and possibly issues (warnings).

In general, validation produces the following output:
* Status
* List (array) of error messages
* List (array) of issue messages.

Users of the API Validation service will get the validation results as a JSON object in the [response](./Send.md#possible-http-responses) to the API call.  

## Error and issue messages
Error and issue messages will frequently specify data fields using a dot notation, such as: `«metadata.journal.identifier»` which reference the [Incoming Notification JSON data structure](./IncomingNotification.md#json-data-structure).  

For interpreting validation messages in the context of SFTP submission of JATS metadata it may be useful to refer to our [JATS metadata](../../JATS/readme.md) page.

In the majority of cases the messages are self-explanatory, but there are some for which further explanation is provided in the table below.

### Message further information

| Message | Explanation |
|---------|-------------|
|Found no actionable routing metadata in notification...|Router works by attempting to match reference information supplied by institutions, such as:<br>- Institution name(s)<br>- Postcodes<br>- Researcher ORCIDs<br>- Domain names<br>- Grant numbers<br><br> to particular article metadata: <br>- Author affiliations<br>- Author ORCIDS<br>- Author email domains<br>- Project funding identifiers (grant numbers).<br><br>This error message occurs where none of this article metadata exists.|


