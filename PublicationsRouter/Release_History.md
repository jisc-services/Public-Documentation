# Release History

## Release 11.3 - October 2021
Minor enhancement, including:
* Inclusion of publisher PNAS (National Academy of Sciences).

## Release 11.2 - August 2021
Minor enhancements, including:
* Modify EPMC harvesting to accomodate changes to EPMC API performance
* Bug fix in JATS parsing so Author Names presented within &lt;name-alternatives&gt; elements are captured
* In publisher testing impove Creative Commons licence checker.

## Release 11.1 - July 2021
Minor enhancements, including:
* Publisher automated testing will no longer report an error if &lt;elocation-id&gt; is provided in place of article first page, last page or page-range values
* Rewording of error messages related to article-version
* New version of *Publisher protocol for FTP deposits* document.

## Release 11.0 - June 2021
Substantial new capability:
* Duplicate filtering
* Improved GUI for entering repository connection details (SWORD etc)

Minor enhancements, including:
* Capture Elsevier corresponding author attribute
* Modify Dspace wording on "How to start using service" page to mention 4Science
* Simplify storage of ORCID and email matching parameters

## Release 10.3 - May 2021
Substantial new capability:
* Strengthen Router API security - all endpoints now require API Key
* Support JATS v1.2, in particular capture article version when set in the new <article-version> element
* Publisher autotesting - check for presence of PDF file, validate supplied article-version values.

Minor enhancements, including:
* Remove free2read metadata field
* Minor GUI changes.

## Release 10.2 - 29 March 2021
Minor enhancements, including:
* Store and display Repository or CRIS type
* Minor GUI changes.

## Release 10.1 - 25 February 2021
Substantial new capability:
* Increase capability of automated testing: capture metadata for display
* Make Crossref harvester more resilient to failure of Crossref API feed.

Minor enhancements, including:
* Bug fix - correctly handle "corporate authors" (as opposed to individual authors)
* Bug fix - make API HTTP request responses consistent with pubic documentation.

## Release 10.0 - 15 January 2021
Substantial new capability:
* Automated testing (including metadata validation) of article submissions for new publishers, with immediate feedback
* Processing and database storage efficiency improvements.

Minor enhancements, including:
* Transmission of author/contributor name suffix (e.g. "Snr." or "Jr.") to Eprints and DSpace repositories
* New "Download CSV - All records" button on publisher deposit history page.

## Release 9.20 - November 2020
Bug fix and more informative error messages.

## Release 9.19 - October 2020
Minor GUI text changes and bug fixes:
* Mention Haplo (new partner)
* Fix reporting error
* Better handling of large zip files.

## Release 9.18 - September 2020
GUI enhancements to address accessibility issues (in order to comply with UK Government guidelines), including:
* Accessibility statement
* Addition of ARIA attributes
* Heading levels in correct sequence (no skipped levels).

## Release 9.17 - August 2020
Bug fixes and refactoring.

## Release 9.16 - May 2020
This release includes the following changes:
* Fix API issue whereby an inappropriate error was being raised when attempting to retrieve content for notifications which had none
* Flag corresponding author by setting `"type": "corresp"`
* Fix Dspace issue where filenames include white space by converting them to underscores
* Include intellectbooks.com OA licence as recognised open access licence
* Update GUI in line with latest Cookie legislation
* Update branding in line with latest Jisc guidelines (Roboto font)
* Improve efficiency of matching algorithm
* Fix issue of matching being done on contributors and authors not always being captured properly from JATS XML.
