# Implementation Proposal

We propose that the sex & gender identity changes happen in 3 phases.

## Summary
|Phase|Task|Who?|Proposed Due Date|
|-----|----|----|-----------------|
|1|Add the 3 new columns|Sites|31-mar-2020|
|1|Switch std macros to using new columns|Roy|31-mar-2020|
|2|Edit E/D/L QA program to check for existence/absence of new/old columns|Roy|29-may-2020|
|2|Remove GENDER from implementations|Sites|29-may-2020|
|3|Return Enroll/Demog/Lang QA|Sites|30-jun-2020|

## 1. Adding the 3 new columns

Epic-using sites should recall that [we posted code to pull the new columns out of Clarity](https://www.hcsrn.org/share/page/site/VDW/document-details?nodeRef=workspace://SpacesStore/8dd70be6-ec56-4c0e-8e9c-96c11a9dddbe) and recode them into the spec-compliant values.  (This is the code that Roy is using at KPWA, so it will likely need some tweaking, but may be useful.)

This is the milder of the changes necessary, and so it should hopefully go down pretty easily.  We propose **Tuesday March 31st 2020** as the due date for this implementation.

As soon as a site has implemented these new columns, we recommend sending out communications to local users explicitly deprecating the old GENDER column, and encouraging everyone to immediately switch to using the new columns.

Roy will edit the VDW standard macros code to remove all references to GENDER, replacing them with SEX_ADMIN, and create a macro that takes SEX_AT_BIRTH and GENDER_IDENTITY as parameters and return a value compatible with the old GENDER specification, for applications that really need the old column.  We will deploy this version of the code on the same timeline as the sites implementation.

## 2. Removing GENDER

We propose **Friday May 29th 2020** as the deadline for removing gender.

We will integrate checks for the new columns & a check that GENDER no longer exists in [the official QA program](https://www.hcsrn.org/share/page/site/VDW/document-details?nodeRef=workspace://SpacesStore/13dc5a14-4caa-4058-ade6-305c40238afa).

## 3. Run the HCSRN Quality Assurance Package & return results
We are due for a run of this anyway. We propose a due date of **Tuesday June 30th** for returning these results.