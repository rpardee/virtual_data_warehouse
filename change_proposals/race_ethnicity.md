# Motivation

This proposal is in response to a recently released change in the US Office of Management & Budget's [directive on standards for the collection of race and ethnicity](https://www.federalregister.gov/documents/2024/03/29/2024-06469/revisions-to-ombs-statistical-policy-directive-no-15-standards-for-maintaining-collecting-and). Despite the newness of this directive (it was released on 28-mar-2024) several of our member organizations (including all the Kaiser regions) are already substantially adhering to this directive with respect to treating Hispanicity (if that's a word) as a type of race rather than an independent ethnicity. There have also been scattered efforts to divine Middle-Eastern/North African status from ethnicity and spoken language.

## Existing Spec
The current spec sets out a series of 5 `race` fields, along with a single `hispanic` field intended to capture that one type of ethnicity.

| Field Name      | Definition                                                                                                  | Type(Len) | Values                                                                                                                                                                                                                                                                                      | Implementation Guidelines                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------ | ----------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| race1 - race5      | The person's race. Preference is for self-reported; please see comment 1 for recording multiple race values | char(2)   | HP = Native Hawaiian / Pacific Islander<br>IN = American Indian / Alaskan Native<br>AS = Asian<br>BA = Black or African American<br>WH = White<br>MU = Multiple races with particular unknown<br>OT = Other, values that do not fit well in any other value<br>UN = Unknown or Not Reported | Fill in the values in the same order as the races are listed in the valid values column. [Guidelines on mapping local race values to the permissible value set in the VDW](https://hcsrnvdw.sharepoint.com/:x:/r/sites/hcsrn-vdw/_layouts/15/Doc.aspx?sourcedoc=%7B8A279712-2456-5230-816F-CF9979A62876%7D&file=Appendix%20E.xlsx&action=default&mobileredirect=true)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| hispanic           | Whether the person is of Hispanic origin / ethnicity                                                        | char(1)   | Y = Yes<br>N = No<br>U = Unknown                                                                                                                                                                                                                                                            |

## Proposed Changes

The new thinking on these concepts--as embodied in [the new OMB directive](https://www.federalregister.gov/documents/2024/03/29/2024-06469/revisions-to-ombs-statistical-policy-directive-no-15-standards-for-maintaining-collecting-and) is that:

1. 'Hispanic' should actually be considered a type of `race` value.
2. People from the Middle-East or North Africa should have their own category of race.

Separate from that, looking at the ethnicity data as actually collected at KPWA (which I believe tracks the Epic foundation system pretty closely) I would add:

3. 'ethnicity' is a hodgepodge of national origin (either one's own or that of one's ancestors), language tradition (again, either one's own, or that of one's ancestors) and subculture.

Several HCSRN organizations are already characterizing their patients/enrollees in these terms, either because they are collecting this information by the new methods, or because they are inferring/imputing the new categories from language preferences (or both). With the advent of the new OMB directive, we anticipate all member organizations will begin primary collection of race/ethnicity data according to this directive, and so it makes sense to alter our VDW demographic specification to accomodate this additional information. Fortunately, there is no fundamental incompatibility between our existing spec and the new directive.

This change gives us occasion to reconsider the design of our race data, and rather than stick with the current, you-can-have-up-to-five + artificial hierarchy structure, the proposal here is to go to a suite of flags, one for each of the race categories in the OMB directive.

Specifically, the propsal is to:

1. Rename `hispanic` to r_hisp
2. Deprecate the existing `race-5` fields.
3. Add an array of independent race fields, one for each race category. Specifically

|Field Name|Definition|Type(len)|Values|Implementation Guidelines|
|----------|----------|---------|------|-------------------------|
|r_hipac |Whether the person on this record is of Hawaiian or Pacific Islander race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_naan  |Whether the person on this record is of Native American or Alaskan Native race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_asian |Whether the person on this record is of Asian race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_black |Whether the person on this record is of Black or African American race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_white |Whether the person on this record is of White race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_hisp  |Whether the person on this record is of Hispanic race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_mena  |Whether the person on this record is of Middle-Eastern or North African race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_other |Whether the person on this record is of some other race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||

In addition to being able to accomodate more different specific values of race than the prior array of 5 race variables, this structure also gets us out of the business of specifying a hierarchy giving the order that values should appear in race 1, 2, 3 etc. fields.

It would also save us 3 bytes of storage per record.

## Anticipated Questions

Q: What do we do for the many many applications (including NIH-required enrollment reports) that require race information to be condensed down to a single value per person?

A: This is of course also an issue with our current array of 5 `race` fields. As part of the rollout of the proposed change, we will write a standard macro that will return a single value, according to the following rules:

1. If there is > 1 'Y' values among the 7 `r_*` fields, it will return the value 'MU', for multiple races.
2. Otherwise it will search the fields in the order listed, returning one of the following values (compatible with the current spec):

|First 'Y' in|Return Value|
|------------|------------|
|r_hipac|HP|
|r_mena|MN|
|r_naan|IN|
|r_asian|AS|
|r_black|BA|
|r_hisp|HS|
|r_white|WH|
|r_other|OT|

So, something like:

```sas
data outset ;
  length all_race_flags $ 10 summary_race $ 2 ;
  set inset ;
  all_race_flags = upcase(cats(of r_:)) ;
  if countc(all_race_flags, 'Y') > 1 then summary_race = 'MU' ;
  else select('Y') ;
    when(r_hipac) summary_race = 'HP' ;
    when(r_mena)  summary_race = 'MN' ;
    when(r_naan)  summary_race = 'IN' ;
    when(r_asian) summary_race = 'AS' ;
    when(r_black) summary_race = 'BA' ;
    when(r_hisp)  summary_race = 'HS' ;
    when(r_white) summary_race = 'WH' ;
    when(r_other) summary_race = 'OT' ;
    otherwise     summary_race = 'UN' ;
  end ;
  label summary_race = 'Summary of the information in the individual race flags--see the individual flags for the true picture' ;
  drop all_race_flags ;
run ;
```

# Questions for VIG

1. Does it make sense to make a place for the fine-grained ethnicity data many of our sites are collecting? That's going to take up a lot of space if we wanted to put it in demog.
    * Are there likely to be multi-site studies that would use this data?
    * Does anyone know of a coded standard for ethnicity? I have been unable to find one.
    * We could do a round of metadata survey/exploration & collate all the values in use at the sites and come up with our own coded standard. Is that worth doing?
    * another idea would be to just code up the modal (and perhaps especially interesting?) values accross sites & lump all the rest into an 'other' type value.
    * **or** we could turf this data off into yet another table (which would only have records for MRNs whose ethnicity we know something about).
