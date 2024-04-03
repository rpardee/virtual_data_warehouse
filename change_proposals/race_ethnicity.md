# Motivation

This proposal is in response to a recently released change in the US Office of Management & Budget's [directive on standards for the collection of race and ethnicity](https://www.federalregister.gov/documents/2024/03/29/2024-06469/revisions-to-ombs-statistical-policy-directive-no-15-standards-for-maintaining-collecting-and). Despite the newness of this directive (it was released on 28-mar-2024) several of our member organizations (including all the Kaiser regions) are already substantially adhering to this directive with respect to treating Hispanicity (if that's a word) as a type of race rather than an independent ethnicity. There have also been scattered efforts to divine Middle-Eastern/North African status from ethnicity and spoken language.

## Existing Spec
The current spec sets out a series of 5 `race` fields, along with a single `hispanic` field intended to capture that one type of ethnicity.

| Field Name      | Definition                                                                                                  | Type(Len) | Values                                                                                                                                                                                                                                                                                      | Implementation Guidelines                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------ | ----------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| race1 - race5      | The person's race. Preference is for self-reported; please see comment 1 for recording multiple race values | char(2)   | HP = Native Hawaiian / Pacific Islander<br>IN = American Indian / Alaskan Native<br>AS = Asian<br>BA = Black or African American<br>WH = White<br>MU = Multiple races with particular unknown<br>OT = Other, values that do not fit well in any other value<br>UN = Unknown or Not Reported | [Guidelines on mapping local race values to the permissible value set in the VDW](https://hcsrnvdw.sharepoint.com/:x:/r/sites/hcsrn-vdw/_layouts/15/Doc.aspx?sourcedoc=%7B8A279712-2456-5230-816F-CF9979A62876%7D&file=Appendix%20E.xlsx&action=default&mobileredirect=true)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| hispanic           | Whether the person is of Hispanic origin / ethnicity                                                        | char(1)   | Y = Yes<br>N = No<br>U = Unknown                                                                                                                                                                                                                                                            |

## Proposed Changes

The new thinking on these concepts--as embodied in [the new OMB directive](https://www.federalregister.gov/documents/2024/03/29/2024-06469/revisions-to-ombs-statistical-policy-directive-no-15-standards-for-maintaining-collecting-and) is that:

1. 'Hispanic' should actually be considered a type of `race` value.
2. People from the Middle-East or North Africa should have their own category of race.

Separate from that, looking at the ethnicity data as actually collected at KPWA (which I believe tracks the Epic foundation system pretty closely) I would add:

3. 'ethnicity' is a hodgepodge of national origin (either one's own or that of one's ancestors), language tradition (again, either one's own, or that of one's ancestors) and subculture.

Several HCSRN organizations are already characterizing their patients/enrollees in these terms, either because they are collecting this information by the new methods, or because they are inferring/imputing the new categories from language preferences (or both). With the advent of the new OMB directive, we anticipate all member organizations will begin primary collection of race/ethnicity data according to this directive, and so it makes sense to alter our VDW demographic specification to accomodate this additional information. Fortunately, there is no fundamental incompatibility between our existing spec and the new directive.

Specifically, the propsal is to:

1. Add two new valid values to the 5 `race` fields:
<dl>
  <dt>HS</dt><dd>Hispanic or Latino</dd>
  <dt>MN</dt><dd>Middle-Eastern or North African</dd>
</dl>

2. Deprecate the existing `hispanic` field
3. Add a new Field to capture detailed ethnicity, defined like so:

|Field Name|Definition|Type(len)|Values|Implementation Guidelines|
|----------|----------|---------|------|-------------------------|
|ethnicity |The person's ethnicity, if known.|char(40)|Any, including null||


## Full Race Value Hierarchy

Because we have a tradition of filling in the race fields in something like reverse prevalence order (thereby emphasizing cohort diversity in the first `race` field), the proposal is to use this for a hierarchy:

|Existing Rank|Proposed Rank|Code|Description|
|--|----|----|-----------|
|1 |1|HP|Native Hawaiian / Pacific Islander|
|  |2|MN|Middle-Eastern / North African|
|2 |3|IN|American Indian / Alaskan Native|
|3 |4|AS|Asian|
|4 |5|BA|Black or African American|
|  |6|HS|Hispanic or Latino|
|5 |7|WH|White|
|6 |8|MU|Multiple races with particulars unknown|
|7 |9|OT|Other, values that do not fit well in any other value|
|8 |10|UN|Unknown or Not Reported|

# Questions for VIG

1. Do we really need an `ethnicity` field? That's going to take up a lot of space.
    * Does anyone know of a coded standard for ethnicity? I have been unable to find one.
    * We could do a round of metadata survey/exploration & collate all the values in use at the sites and come up with our own coded standard. Is that worth doing?
    * another idea would be to just code up the modal (and perhaps especially interesting?) values accross sites & lump all the rest into an 'other' type value.
    * **or** we could turf this data off into yet another table (which would only have records for MRNs whose ethnicity we know something about).
2. Maybe rip the band-aid right off & actually remove the `hispanic` field?
3. Do we want to argue over the preference rankings?
