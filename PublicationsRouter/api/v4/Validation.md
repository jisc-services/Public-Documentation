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
| Issue:<br> `«metadata.author.affiliations.org» may be overpopulated: "Faculty of Education, Manchester University" (contains "Faculty")` | Indicates that a supplied affiliation organistion name may contain more than just the name of the institution.  For example it may also contain a department name, or other address elements. See below for example of proper encoding. |
| Issue:<br> `In the «metadata.author» array, X of the Y affiliations (from Z authors) have questionable «org» values. Please ensure affiliation address elements, including department & organisation names, are tagged separately` | Indicates that a number of supplied affiliation organisation names may contain more than just the name of the institution (these issues are also logged separately - see previous row). <br><br>Within JATS XML, the organisation name is supplied in the affiliation ***\<institution\>*** element; department names may also be supplied in the <institution> element, BUT they should additionally be tagged with the ***content-type*** attribute with one of these values: **"dept", "department", "office" or "division"** (to differentiate it from the institution name).<br><br>For example "Faculty of Education, Manchester University" should be encoded as shown below. |
|………………………………………………………………………||

<br>
NOTE: If you cannot find the information you are looking for on this page please email Jisc's Router support team - [help@jisc.ac.uk](mailto:help@jisc.ac.uk?subject=Publications%20Router%20-%20) (mentioning Publications Router in the Subject line) and we will do our best to add it.
<br><br>

## Example correct JATS codings ##

Example coding of Department and Institution name ("Faculty of Education, Manchester University") in a JATS affiliation:
```xml
<aff>
  <institution-wrap>
    <institution content-type="dept">Faculty of Education</institution>,
    <institution>Manchester University</institution>
  </institution-wrap>
</aff>
```

Example coding of a full affiliation:
```xml
<aff id=" aff1">
	<sup>1</sup>
	<institution-wrap>
		<institution content-type="department">School of Chemistry</institution>
		<institution>University of Bristol</institution>
		<institution-id institution-id-type="grid">grid.5337.2</institution-id>
		<institution-id institution-id-type="ror">https://ror.org/0524sp257</institution-id>
	</institution-wrap>
	<addr-line>Cantock's Close</addr-line>
	<city>Bristol</city>
	<state>Avon</state>
	<postal-code>BS8 1TS</postal-code>
	<country country=”GB”>United Kingdom</country>
</aff>
```

Alternative example encoding of a full affiliation using *\<addr-line\>* with *content-type* attribute:
```xml
<aff id=" aff1">
	<label>1</label>
	<institution-wrap>
		<institution content-type="division">School of Chemistry</institution>
		<institution>University of Bristol</institution>
		<institution-id institution-id-type="grid">grid.5337.2</institution-id>
		<institution-id institution-id-type="ror">https://ror.org/0524sp257</institution-id>
	</institution-wrap>
	<addr-line>Cantock's Close</addr-line>
	<addr-line content-type="city">Bristol</addr-line>
	<addr-line content-type="state">Avon</addr-line>
	<addr-line content-type="postal-code">BS8 1TS</addr-line>
	<addr-line content-type="country">United Kingdom</addr-line>
</aff>
```
