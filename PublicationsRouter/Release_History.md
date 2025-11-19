# Release History

## Release 13.16.3 - August 2025
Main changes:
1. Update python libraries to latest versions
2. Fix bug in display of INI organisation id (resulting from new ROR API)
3. Remove HTML snippets from harvested article metadata
4. New admin screen to display number of users configured for each organisation
5. New admin screen to display Router version information (including versions of libraries).

## Release 13.15.0 - August 2025
Main changes:
1. Fix accessibility issues
2. Upgrade Router to use ROR API v2 to retrieve Organisation identifiers
3. Fixed an issue with display of publishers' history report of articles matched to institutions
4. Fixed bug in email subject line for publisher monthly article distribution reports
5. missing article number for SWORD feeds to vanilla Eprints.

## Release 13.14.0 - July 2025
Minor release, including:
1. Amend Router web site to clarify Router only available to full Jisc HE members
2. Resolve problem of e-num not getting populated by Eprints-RIOXX plugin
3. Modify use of ROR API (v1) so it continues to work after July 28, 2025 (when v2 becomes the default).

## Release 13.13.4 - May 2025
Minor release, including:
1. Modify JATS parsing to capture collaborator details (including individual collaborators) provided in new elements in JATS v1.4
2. New admininistrator function to dequeue a failing (blocking) notification
3. Update FTP Deposit Protocol PDF to reflect JATS processing changes.

## Release 13.12.0 - April 2025
Minor release, including:
1. Limit FTP deposit zip file size to avoid out-of-memory error
2. Capture history dates from JATS v1.4 `<pub-history><event>` elements.

## Release 13.10.1 - March 2025
Minor release, including:
1. New version of FTP Protocol Document (PDF)
2. Improve error information for bad dates in JATS files.

## Release 13.10.0 - Feb 2025
Minor release, including:
1. Fix double colon problem in abstract subheadings
2. Fix "articles in period" report
3. New GUI screen to allow users to view JSON metadata of live deposits (from a particular publisher) that have been matched to HEIs
4. Software library refresh - update libraries to latest versions.

## Release 13.9.3 - Feb 2025
Minor release, including:
1. Admin functionality improvements.

## Release 13.9.1 - Feb 2025
Minor release, including:
1. Fix cookie deletion bug.

## Release 13.9.0 - Feb 2025
Minor release, including:
1. Resolve various accessibility issues to bring website into compliance
2. Fix cookie deletion bug, so that all tracking cookies are removed if user selects that option
3. Resolve minor Router admin issue, such that unnecessary INFO alerting emails to Jisc PubRouter admins are no longer generated when scheduled shutdown occurs.

## Release 13.8.3 - Jan 2025
Minor bug fix release, including:
1. Allow publishers to deposit notifications with _Article Version_ **CVoR**, **EVoR** or **C/EVoR** and _Licence Version_ of **VoR** (previously licence version had to exactly match article version, and so such notifications were being rejected)
2. Fix some moved (broken) links to Jisc web-pages.

## Release 13.8.2 - Jan 2025
Minor release, including:
1. Fix JATS affiliation parsing issue resulting in *raw* string being wrongly formatted in some circumstances, such as when punctuation occurs after \<insitution-wrap\> elements
2. Additional unit tests.

## Release 13.8.1 - Jan 2025
Minor release, including:
1. Fix 'publisher duplicate submissions' internal report issue
2. Additional unit tests.

## Release 13.8.0 - Dec 2024
Major release, including:
1. Publisher Auto-testing: new testing of author affiliation organisation elements
2. Publisher Auto-testing: change licence value test
3. New report on author affiliation organisation overpopulation (Jisc support only)
4. Updates to public documentation. 

## Release 13.7.0 - 21 Nov 2024
Major release, including:
1. Technology security upgrades.
2. Enhanced admin email capability to allow user email addresses to be selected
3. Replaced broken links to discontinued blogs
5. Added reporting metric for file deletion. 

## Release 13.6.0 - 15 Oct 2024
Minor release, including:
1. Resolve newly emerged PubMed harvesting problem
2. Standardization of Title case on GUI screens
3. Supportability enhancements (code refactoring).

## Release 13.5.0 - 17 Sept 2024
Major release, including:
1. New scheduling system, allowing background execution of long running reports; this also improve Router's throughput capacity and performance (reducing the average time taken for deposits to transit through Router into target repositories)
2. New admin dashboard to summarise all matching parameters, hilighting use of RegEx in name variants; plus new page to compare current with old (archived) versions of matching parameters and enable reversion to earlier version
3. Update displayed Eprints repository options display to make more intuitive
4. Improve test suite performance
5. Fix monthly report failure due to missing template file

## Release 13.4.0 - 17 June 2024
Minor release, including:
1. Several Jisc admin only reports:
  * List users
  * Duplicate publisher submissions
  * Analysis of structured affiliation usage by publisher

## Release 13.3.0 - 18 April 2024
Minor release, including:
1. Tech-refresh (updgrade to latest version of librarie)
2. New report of unique articles in period (Jisc admin only)
3. Bug fixes - use correct elocation-Id for PubMed articles, strip linefeeds from captured affiliations.

## Release 13.2.0 - 12 March 2024
Major release, including:
1. Report missing PDF from zip file as an issue (& process the submission) instead of an error (without processing)
2. Amendments to publisher auto-testing - don't check for _provider.ref_, report zero ORCIDs as an error, amend author contributor-type validation.

## Release 13.1.0 - 31 January 2024
Major release, including:
1. Fix login link issue (replace by login access code)
2. Improve FTP submission error messages (include filename)
3. Fix harvester bug introduced in prior release - wrong function name
4. Update Router documentation re. individual user accounts

## Release 13.0.0 - 29th January 2024
Major release, including:
1. Introduction of individual User Accounts, with two factor login (via emailed login link)
2. Substitution of Router *Privacy* notice by generic Jisc notice
3. Bug fix relating to recording of job statistics in the event of job failure.

## Release 12.15.0 - 5th October 2023
Minor release, including:
1. Updated version of Publishers FTP Protocol document (PDF)
2. Recognition of 2 alternate spellings of Acknowledgement/Acknowledgment (for purpose of removing superfluous title)
3. Amendment to Publisher auto-testing - stricter checks on author affiliations; recognition of *open-government-licence* so issue not raised; changes to wording of test result email.

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
