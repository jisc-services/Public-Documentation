# Release History

## Release 10.0 - January 2021
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
