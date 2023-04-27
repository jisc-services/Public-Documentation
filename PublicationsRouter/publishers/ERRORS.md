# Article Deposit Errors #

The purpose of this page is to help publishers understand Router's automatically error messages which may occur from time to time.


| Error Text | Explanation |
|---------|----------|
| Failed to create notification from file - Problem with notification metadata: DataSchemaException('Cast function `coerce_to_ymd` failed - Unable to parse 0000 with any known format.')                                                                            | Indicates that a supplied date (publication date, accepted date or a history date) has value '0000' which is not recognised as a legitimate year.  |
| Filename: issue-files.zip – Failed to create notification from file - Problem reading file: */Incoming/ftptmp/ 757b2058f09645b0b9a6b5ecc2460b24/ 0cb3e501553148e49a86dd0b9dbe9d12/ a60ba32c64044c35a28f3108a091ff4b.zip*. Error: No JATS metadata found in package | This indicates that the supplied package does not contain an XML file with JATS metadata.  **However, in this particular case the error can be ignored as it is known that a zip file with a name (like) 'issues-files.zip' contains summary information about an Issue, rather than an article, so there is nothing for Router to process.** |
