# Motivation

This proposal is in response to a recently released change in the US Office of Management & Budget's [directive on standards for the collection of race and ethnicity](https://www.federalregister.gov/documents/2024/03/29/2024-06469/revisions-to-ombs-statistical-policy-directive-no-15-standards-for-maintaining-collecting-and). Despite the newness of this directive (it was released on 28-mar-2024) several of our member organizations (including all the Kaiser regions) are already substantially adhering to this directive with respect to treating Hispanicity (if that's a word) as a type of race rather than an independent ethnicity. There have also been scattered efforts to divine Middle-Eastern/North African status from ethnicity and spoken language.

## Existing Spec
The current spec sets out a series of 5 `race` fields, along with a single `hispanic` field intended to capture that one type of ethnicity.

| Field Name         | Definition                                                                                                  | Type(Len) | Values                                                                                                                                                                                                                                                                                      | Implementation Guidelines                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------| ----------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| race1 - race5      | The person's race. Preference is for self-reported; please see comment 1 for recording multiple race values | char(2)   | HP = Native Hawaiian / Pacific Islander<br>IN = American Indian / Alaskan Native<br>AS = Asian<br>BA = Black or African American<br>WH = White<br>MU = Multiple races with particular unknown<br>OT = Other, values that do not fit well in any other value<br>UN = Unknown or Not Reported | Fill in the values in the same order as the races are listed in the valid values column. [Guidelines on mapping local race values to the permissible value set in the VDW](https://hcsrnvdw.sharepoint.com/:x:/r/sites/hcsrn-vdw/_layouts/15/Doc.aspx?sourcedoc=%7B8A279712-2456-5230-816F-CF9979A62876%7D&file=Appendix%20E.xlsx&action=default&mobileredirect=true)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| hispanic           | Whether the person is of Hispanic origin / ethnicity                                                        | char(1)   | Y = Yes<br>N = No<br>U = Unknown                                                                                                                                                                                                                                                            |

## Proposed Changes

The new thinking on these concepts--as embodied in [the new OMB directive](https://www.federalregister.gov/documents/2024/03/29/2024-06469/revisions-to-ombs-statistical-policy-directive-no-15-standards-for-maintaining-collecting-and) is that:

1. 'Hispanic' should actually be considered a type of `race` value.
2. People from the Middle-East or North Africa should have their own category of race.

Separate from that, looking at the ethnicity data as actually collected at KPWA (which I believe tracks the Epic foundation system pretty closely) I would add:

3. 'ethnicity' is a hodgepodge of national origin (either one's own or that of one's ancestors), language tradition (again, either one's own, or that of one's ancestors) and subculture. We will follow-up with an additional change request that accomodates the fine-grained ethnicity data available at HCSRN member sites.

Several HCSRN organizations are already characterizing their patients/enrollees in these terms, either because they are collecting this information by the new methods, or because they are inferring/imputing the new categories from language preferences (or both). With the advent of the new OMB directive, we anticipate all member organizations will begin primary collection of race/ethnicity data according to this directive, and so it makes sense to alter our VDW demographic specification to accomodate this additional information. Fortunately, there is no fundamental incompatibility between our existing spec and the new directive.

This change gives us occasion to reconsider the design of our race data, and rather than stick with the current, you-can-have-up-to-five + artificial hierarchy structure, the proposal here is to go to a suite of flags, one for each of the race categories in the OMB directive.

Specifically, the propsal is to:

1. Rename `hispanic` to r_hisp
2. Deprecate the existing `race1-5` fields.
3. Add an array of independent race fields, one for each race category.
4. Add a single field that summarizes the information in the flags.

### New Fields

|Field Name  |Definition|Type(len)|Values|Implementation Guidelines|
|------------|----------|---------|------|-------------------------|
|r_nhpi      |Whether the person on this record is of Native Hawaiian/Pacific Islander race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_indig     |Whether the person on this record is of American Indian/Alaskan Native race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_asian     |Whether the person on this record is of Asian race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_black     |Whether the person on this record is of Black or African American race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_hisp      |Whether the person on this record is of Hispanic race.|char(1)| Y = Yes<br>N = No<br>U = Unknown|(This is just the existing `hispanic` field renamed.)|
|r_mena      |Whether the person on this record is of Middle-Eastern or North African race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_white     |Whether the person on this record is of White race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|r_mult      |Whether the person on this record belongs to multiple racial groups, but the individual groups are unknown|char(1)| Y = Yes<br>N = No<br>U = Unknown|This is only to be used when the source data convey that the person is multi-race, but does not include information on what those multiple races are. It is ***not*** an indicator that e.g., more than one of the other `r_*` flags are set to 'Y'.|
|r_other     |Whether the person on this record is of some other race.|char(1)| Y = Yes<br>N = No<br>U = Unknown||
|summary_race|Summary of the information in the individual race flags--see the individual flags for the true picture|char(2)|AS = Asian<br>BA = Black or African American<br>HP = Native Hawaiian / Pacific Islander<br>HS = Hispanic or Latino/Latina<br>IN = American Indian / Alaskan Native<br>MN = Middle-Eastern or North African<br>WH = White<br>MU = Multiple races, ***whether or not*** the particular races are known<br>OT = Other, values that do not fit well in any other value<br>UN = Unknown or Not Reported<br>|Added in recognition that the vast majority of uses of race information require each person to be assigned to one and only one category. Implementation is according to the best information available at the site--this spec does not require any particular algorithm.<br><br>In particular, *if timing information is available at the site*, implementers may want to consider timing of responses to discern whether a value of 'other' in early data was a substitute for one of the new categories.<br><br>Note that because the available source data will vary across sites, the implementation of this variable will also vary. If absolute consistency is important to a project, users are advised to implement their own summary field from the source flags.|


In addition to being able to acommodate more different specific values of race than the prior array of 5 race variables, this structure also gets us out of the business of specifying a hierarchy giving the order that values should appear in race 1, 2, 3 etc. fields.

## Anticipated Questions

Q: How should I implement `summary_race`? I do not have information regarding the timing of responses.

A: Here is one possible way to implement:

1. If none of the `r_*` field flags are set to 'Y', use 'UN' for unknown.
2. If exactly 1 flag is 'Y', use the corresponding value from the spec.
3. If > 1 of the flags are 'Y' use the value 'MU' (multiple races).

|First 'Y' in|Summary Value|
|------------|------------|
|r_nhpi|HP|
|r_indig|IN|
|r_asian|AS|
|r_black|BA|
|r_hisp|HS|
|r_mena|MN|
|r_white|WH|
|r_mult|MU|
|r_other|OT|

So, something like:

```sas
data my_demog ;
  length all_race_flags $ 10 summary_race $ 2 ;
  set inset ;
  all_race_flags = upcase(cats(of r_:)) ;
  * How many race flags are set? ;
  select(countc(all_race_flags, 'Y')) ;
    when(0) summary_race = 'UN' ; * no flags set--unknown ;
    when(1) select('Y') ; * just one flag--which one? ;
      when(r_nhpi)    summary_race = 'HP' ;
      when(r_mena)    summary_race = 'MN' ;
      when(r_indig)   summary_race = 'IN' ;
      when(r_asian)   summary_race = 'AS' ;
      when(r_black)   summary_race = 'BA' ;
      when(r_hisp)    summary_race = 'HS' ;
      when(r_white)   summary_race = 'WH' ;
      when(r_mult)    summary_race = 'MU' ;
      when(r_other)   summary_race = 'OT' ;
      otherwise       summary_race = 'UN' ;
    end ;
    otherwise         summary_race = 'MU' ; * multiple flags are set ;
  end ;
  label summary_race = 'Summary of the information in the individual race flags--see the individual flags for the true picture' ;
  drop all_race_flags ;
run ;
```

# Request For Feedback

## For Scientific Users

1. How do you expect these changes to affect your current program of research?
2. How valuable is this information to you?
3. Are the new variables proposed:
    1. conceptually coherent?
    2. clearly defined?
    3. likely to be useful?

## For Implementers

1. Can you obtain the information needed for the new variables?
1. Do you have other useful, race-relevant information in your source data that would not be well accomodated by the proposed variables?
3. Are the new variables proposed
    1. conceptually coherent?
    1. clearly defined?
    1. likely to be useful?
3. Can you think of a better way of storing this data for research use?
