# Release History

## Release 12.14.0 - 20th September 2023
Technology refresh release, including:
1. Upgrade third-party libraries to current versions
2. Upgrade Elasticsearch database to latest version on new dedicated server.

## Release 12.13.0 - 8th September 2023
Minor release, including:
1. Improve error handling of Sword-Out process.

## Release 12.12.0 - 9th August 2023
Minor release, including:
1. Accessibility improvements 
2. New admin bulk-email function
3. Bug fixes.

## Release 12.11.0 - 7th June 2023
Major release, including:
1. Remove upper limit on password length
2. Restrict number of failed login attempts (enforced pause after 10 failures) 
3. Capture `acceptance_date` from Crossref if it is supplied as a *history* date.

## Release 12.10.0 - 16th May 2023
Major release, including:
1. Add technical contact email addresses field to Account records
2. Revise wording for automated publisher error emails wrt "Important note: Atypon..." and send such emails to techncial contacts 
3. Modify JATS parsing to exclude capture of article-categories, only capture keywords
4. Fix Matching Parameters Organisation ID display error that affected Crossref (FundRef) IDs.
5. Remove markup tags from Abstract text received from EPMC
6. For DSpace - ensure output SWORD metadata includes non-FundRef funding identifiers (e.g. ROR IDs).
7. New admin screens to show Router batch schedule & batch metrics

## Release 12.9.2 - 11th May 2023
Emergency release:
1. Fix an occassional problem with SWORD deposits originating from EPMC which resulted in the wrong article PDF file being uploaded to a Repository. This did not affect Router API users).

## Release 12.9.1 - 27th April 2023
Emergency release:
1. Fix a problem with JATS XML parsing which occasionally resulted in authors with no affiliations, erroneously being assigned (many) cross-referenced affiliations. (This would have adversely impacted Router API users).

## Release 12.9.0 - 21st April 2023
Major release, including:
1. Fix "File is not a zip file" bug (which generated false errors of this type)
2. Capture additional history dates from Crossref
3. Add feature to enable easy population of Org Ids in Matching Params
4. Improve DOI search of Notification history (allow full URLs)
5. Capture 'corresp' author status in all circumstances 
6. Check for presence of PDF in publisher package & raise error if missing
7. Automatically email submission errors to Publishers (when a deposited article can't be processed)
8. Capture processing metrics

## Release 12.8.0 - 28th February 2023
Minor release, including:
1. Rationalise and reduce the publication date-types by replacing "pub-electronic" and "pub-print" by "epub" and "ppub" respectively.
2. Fix GUI display sort bug (admin only) "Pub test overview" screen
3. New (admin only) features for Harvester administration.
4. Publisher autotesting now reports missing 'city', 'street', 'country' & 'postcode' fields in author structured affiliations.
5. Groundwork for future analysis & reporting of deposit characteristics
6. Remove redundant "Acknowledgement" title if present.

## Release 12.7.1 - 23rd February 2023
Emergency release, including:
1. Bug fix to prevent erroneous deposits

## Release 12.7.0 - 7th February 2023
Minor release, including:
1. Update links on *Technical information* page to point to latest version (v4) of API documentation 
2. Further internal changes to resolve memory leak

## Release 12.6.0 - 30th January 2023
Minor release, including:
1. Fix password reset bug
2. Internal changes to address memory leak

## Release 12.5.0 - October 2022
Major release, including:
1. Upgrade underlying technology to latest version
2. More flexible matching parameters load: allow loading of a sub-set of parameters  (e.g. just ORCIDS)
3. Remove redundant "Abstract" heading text from abstracts
4. Add DOI search function to notification history page
5. Add content management capability for system administrators
6. Refine publisher article distribution report: No Test repositories; additional totals line
7. Add support for Ringgold Org IDs in matching params

## Release 12.4.0 - September 2022
Major release, including:
1. New XML fields (peer-reviewed, acknowledgements, eLocation-id) are included in metadata that is deposited in Eprints & Dspace repositories  
2. Addition of Organisation IDs to matching parameters, which are now used in the matching algorithm
3. New publisher report: article (identified by DOI) distribution to institutions
4. Bug fix - capture all email addresses from cross-referenced affiliations

## Release 12.3.2 - August 2022
Minor release, including:
1. Added 'American Chemical Society' to publishers
2. Fixed email format (font selection) proble 
3. Fix package creation build problem 

## Release 12.3.1 - July 2022
Minor release, including:
1. New version of publisher FTP Deposit Protocol PDF document

## Release 12.3 - June 2022
Major release, including:
1. Introduction of API v4, has some changes to notification structures
* structured affiliations
* e_num (elocation-number)
* ack (acknowledgements)
* peer_reviewed boolean indicator
2. Capture structured affiliations, e_num and ack from JATS packages
3. Mitigation for a memory leak
4. Improved account administration functionality (admins only).

## Release 12.2.3 - June 2022
Minor release, including:
* Revised diagrams on web pages
* Mention of Worktribe & Company of Biologists
* Stop error occurrence when processing an Atypon format zip file containing an Issue directory 

## Release 12.2 - March 2022
Minor release, including:
* Remedy for password reset problem experienced by some users (cause attributed to an Outlook plugin altering reset URL)
* Fix bug where a publisher's default open licence was not being applied if the default embargo duration was set to 0 (i.e. licence effective immediately)
* Supported a specific JATS XML affiliation encoding permutation that was previously ignored by Router
* Improve resilience to database connection timeout exceptions.

## Release 12.1 - February 2022
Minor release, including:
* Resolved issue with release 12.0

## Release 12.0 - February 2022
Major release, including:
* Migration to new relational database platform
* Fix bug in domain-name matching algorithm: now problems like "chester.ac.uk" matching "manchester.ac.uk" no longer occur
* Addition of new open licences: "https://creativecommons.org/publicdomain/zero/1.0/" and "https://www.nationalarchives.gov.uk/doc/open-government-licence"
* Fixed password reset problem where reset link supplied by email wasn't working.

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
