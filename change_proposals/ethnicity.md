# Motivation

There is currently detailed ethnicity data at the sites which may be useful for multi-site research that needs to target particular groups of people. Because this data is not currently being collated into a VDW table it is not easily used by multi-site projects.

## Existing Spec

Not applicable

## Proposed Changes

We propose adding a new table, very like the current `_vdw_language` table, to hold free-text descriptions of patient ethnicities.

### The New Table

The `_vdw_ethnicity` table will contain information on patients' ethnic backgrounds. There is one record per person per known ethnicity. People on whom you have no ethnicity information should not be included in the table.

| Variable Name | Definition                                                                                           | Type(Len) | Values                                                                                                                                  | Implementation Guidelines |
| ------------- | ---------------------------------------------------------------------------------------------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| mrn           | Medical record number is the unique patient identifier within a site and should never leave the site | char(\*)  | Unique to each patient at each site                                                                                                     |                           |
| ethnicity      | A free-text description of the ethnic background.                                                    | char(\*)  | Any non-null value| Misspellings should be corrected (e.g. 'Fijan' should be changed to 'Fijian')                          |

## Primary Key
mrn + ethnicity

## Foreign Keys

|Source Variable|Target Table|Target Variable<br>(Primary Key)|Orphans allowed?|
|---------------|------------|--------------------------------|----------------|
|mrn            |Demographics|mrn                             |No              |

## Anticipated Questions

Q: Can't the values be standardized?

A: Unfortunately we were unable to find any existing standard that would accomodate [the wide variety of values seen in our source data.](https://hcsrnvdw.sharepoint.com/:u:/r/sites/VIG/Shared%20Documents/Enrollment%20and%20Demographics/clarity_ethnicity_collate.html?csf=1&web=1&e=jrLbf7) In the absence of expert guidance on collecting this data we are opting to at least start out making the data available for multi-site projects as free-text. As the data get used and users advise us we may opt to standardize further.

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
