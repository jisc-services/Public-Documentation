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

For interpreting validation messages in the context of SFTP submission of JATS metadata it may be useful to refer to our [JATS metadata](../../JATS) page.

In the majority of cases the messages are self-explanatory, but there are some for which further explanation is provided in the table below.

### Explanation of particular messages

| Message | Explanation |
|---------|-------------|
|Atypon file has incorrect format|The zip file was deposited in your SFTP account's `atypon_xfer` directory, which is intended for files in "Atypon zip format", but was found not to have the exptected format.<br><br>See our [Publisher protocol for FTP deposits](https://pubrouter.jisc.ac.uk/static/docs/FTP_deposit_protocol_for_new_publishers.pdf) document, Section 3 *FTP Submission*, for further details of zip file structures.<br><br> |
|Unable to parse XML file '...' on 1st attempt, succeeded after 2nd. Syntax Error: ...|<br>There are problems with the JATS XML which resulted in initial processing failure. However, it was processed at a second attempt using more permissive rules, but this could have resulted in data loss. <br><br>You can (probably) gain further insight into the problem by using a (free) online XML validator, such as: https://www.liquid-technologies.com/online-xml-validator, to check your JATS XML.<br><br> |
|Found no actionable routing metadata in notification...|<br>Router works by comparing reference information supplied by institutions, such as:<br>- Institution name(s)<br>- Postcodes<br>- Researcher ORCIDs<br>- Domain names<br>- Grant numbers<br><br> to particular article metadata: <br>- Author affiliations<br>- Author ORCIDS<br>- Author email domains<br>- Project funding identifiers (grant numbers).<br><br>This error message occurs where none of this article metadata exists.<br><br>|
| ... Problem processing JATS metadata: Valid article version value '**XXX**', but no licences share this in their specific-use attribute.<br><br>*Where XXX is one of: AM, P, VOR, EVOR, CVOR, C/EVOR.*  | You have supplied at least one licence value within &lt;permissions&gt; which contains a *specific-use* value, but none of the licence specific-use values match the supplied article version value obtained from either &lt;article-version&gt; or specific-use attribute of root &lt;article&gt; element.  |
| Error:<br>`«metadata.author.affiliations» has missing fields: «org», «city», «country».` | At least one of the authors had an affiliation which did NOT contain Organisation, City and Country defined in an acceptable JATS struture. For more information see Section 5.9 - *Structured affiliations* in [Publisher protocol for FTP deposits](https://pubrouter.jisc.ac.uk/static/docs/FTP_deposit_protocol_for_new_publishers.pdf). |
| Issue:<br> `«metadata.author.affiliations» has missing desirable fields: «country_code», «dept», «postcode», «street», «identifier».`  | At least one of the authors had an affiliation which did NOT contain Department, Street, Postcode, Country-code or any Identifiers defined in an acceptable JATS struture. For more information see Section 5.9 - *Structured affiliations* in [Publisher protocol for FTP deposits](https://pubrouter.jisc.ac.uk/static/docs/FTP_deposit_protocol_for_new_publishers.pdf). |
|………………………………………………………………………||

