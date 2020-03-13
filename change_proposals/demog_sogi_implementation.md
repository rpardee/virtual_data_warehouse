# Implementation Proposal

We propose that the sex & gender identity changes happen in 3 phases.

## Summary
|Phase|Task|Who?|Proposed Due Date|
|-----|----|----|-----------------|
|1|Produce an interim QA program to check implementations|Roy|28-feb-2020 ([done](https://www.hcsrn.org/share/page/site/VDW/document-details?nodeRef=workspace://SpacesStore/27f7d47d-dbe5-44c4-9842-05cecc7d7a2b))|
|1|Add the 3 new columns|Sites|30-apr-2020|
|1|Switch std macros to using new columns|Roy|30-apr-2020|
|2|Integrate interim QA checks into main E/D/L QA program + add check that GENDER is removed.|Roy|29-may-2020|
|2|Remove GENDER from implementations|Sites|26-jun-2020|
|3|Return Enroll/Demog/Lang QA|Sites|31-jul-2020|

## 1. Adding the 3 new columns

This is the milder of the changes necessary, and so it should hopefully go down pretty easily.  We propose **Thursday April 30th 2020** as the due date for this implementation.

Epic-using sites should recall that [we posted code to pull the new columns out of Clarity](https://www.hcsrn.org/share/page/site/VDW/document-details?nodeRef=workspace://SpacesStore/8dd70be6-ec56-4c0e-8e9c-96c11a9dddbe) and recode them into the spec-compliant values.  (This is the code that Roy is using at KPWA, so it will likely need some tweaking, but may be useful.)

In order to verify your implementation and gather statistics on e.g., just how often ```sex_admin``` agrees with ```gender``` we have written a small QA program which [can be found on the HCSRN portal at this link](https://www.hcsrn.org/share/page/site/VDW/document-details?nodeRef=workspace://SpacesStore/27f7d47d-dbe5-44c4-9842-05cecc7d7a2b).  Please return the results of the /share folder from that package to Roy when you are happy with your implementation.

As soon as a site has implemented these new columns, we recommend sending out communications to local users explicitly deprecating the old GENDER column, and encouraging everyone to immediately switch to using the new columns.

Roy will edit the VDW standard macros code to remove all references to ```gender```, replacing them with ```sex_admin```, and create a macro that takes ```sex_at_birth``` and ```gender_identity``` as parameters and return a value compatible with the old ```gender``` specification, for applications that really need the old column.  We will deploy this version of the code on the same timeline as the sites implementation.

## 2. Removing ```gender```

We propose **Friday June 26th 2020** as the deadline for removing ```gender```.

We will integrate checks for the new columns & a check that GENDER no longer exists in [the official QA program](https://www.hcsrn.org/share/page/site/VDW/document-details?nodeRef=workspace://SpacesStore/13dc5a14-4caa-4058-ade6-305c40238afa).

## 3. Run the HCSRN Quality Assurance Package & return results
We are due for a run of this anyway. We propose a due date of **Friday July 31st** for returning these results.
