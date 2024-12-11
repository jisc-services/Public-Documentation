# Article Deposit Errors #

The purpose of this page is to help publishers understand some of Router's error messages which may occur from time to time, or which arise through automated testing of submitted article packages in Router's UAT (user acceptance testing) environment.

## General Errors ##

Errors which may arise when depositing article packages either during normal running or during automated testing in UAT.

| Error Text | Explanation |
|---------|----------|
| Failed to create notification from file - Problem with notification metadata: DataSchemaException('Cast function `coerce_to_ymd` failed - Unable to parse 0000 with any known format.')                                                                            | Indicates that a supplied date (publication date, accepted date or a history date) has value '0000' which is not recognised as a legitimate year.  |
| Filename: issue-files.zip – Failed to create notification from file - Problem reading file: */Incoming/ftptmp/ 757b2058f09645b0b9a6b5ecc2460b24/ 0cb3e501553148e49a86dd0b9dbe9d12/ a60ba32c64044c35a28f3108a091ff4b.zip*. Error: No JATS metadata found in package | This indicates that the supplied package does not contain an XML file with JATS metadata.  **However, in this particular case the error can be ignored as it is known that a zip file with a name (like) 'issues-files.zip' contains summary information about an Issue, rather than an article, so there is nothing for Router to process.** |
| Failed to create notification from file 'XXXXX' - Problem storing file: */Incoming/ftptmp/ blah999blah888blah777blah666blah/ blah555blah444blah333blah222blah/XXXXX.zip*. Error: Failed to store file 'ArticleFilesJATS.zip' in remote container 'YYYYYY'. Return code: 413. | Indicates that the supplied zip file, after repackaging by Publications Router, exceeded the permitted file size of 2GB. <br><br>This error is relatively rare. It occurs where the supplied zip file is just under Router's 2GB limit, but after repackaging by Router (which involves unzipping and re-zipping) it then exceeds the limit (because the compression level used by Router is presumably lower than that used for the originally zip file).<br><br>Such large zip files typically contain supplementary video clips or other large media or data files.<br><br>Router's Technical Support team can usually find the original file and examine the enclosed JATS XML. If it contains authors with UK affiliations, then it may be possible to remove certain items from the zip file - replacing them with a *removal notice README.txt* file, so that the resulting file can be successfully processed. |

## UAT Auto-testing Errors & Issues ##

Errors or Issues which may arise only during automated testing in UAT.

| Error/Issue Text | Explanation |
|---------|----------|
| «metadata.author.affiliations.org» may be overpopulated: *"Faculty of Education, Manchester University" (contains "Faculty")* | Indicates that a supplied affiliation organistion name may contain more than just the name of the institution.  For example it may also contain a department name, or other address elements. See below for example of proper encoding. |
| In the «metadata.author» array, *x* of the *y* affiliations (from *z* authors) have questionable «org» values. Please ensure affiliation address elements, including department & organisation names, are tagged separately | Indicates that a number of supplied affiliation organisation names may contain more than just the name of the institution (these issues are also logged separately - see previous row). <br><br>Within JATS XML, the organisation name is supplied in the affiliation ***\<institution\>*** element; department names may also be supplied in the <institution> element, BUT they should additionally be tagged with the ***content-type*** attribute with one of these values: **"dept", "department", "office" or "division"** (to differentiate it from the institution name).<br><br>For example "Faculty of Education, Manchester University" should be encoded as shown below. |

<br>
NOTE: If you cannot find the information you are looking for on this page please email Jisc's Router support team - help@jisc.ac.uk (mentioning Publications Router in the Subject line) and we will do our best to add it.
<br><br>

## Example JATS codings ##

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
