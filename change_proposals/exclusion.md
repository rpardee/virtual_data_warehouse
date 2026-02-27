# Motivation

In the course of doing research our patient/members will often express their desire to put persistent limits on the uses to which their data can be put. Some never want to be contacted for recruitment, for example. Others don't want researchers to review their medical charts. This table is for documenting which people have expressed those wishes so that they can be excluded from multi-site research cohorts early in the research process.

## Existing Spec

While HCSRN has not had a VDW specification for this table, CESR has had one since at least 2017. The spec below is CESR's.

## Proposed Changes

N/A

### The New Table

The `_vdw_exclusion` table will have one record per person (mrn) with one or more exclusion conditions indicated. Only MRNs for people with exclusions should appear in this table.

| Variable Name | Definition                                                                                                  | Type(Len)  | Values                                                                                                                                  | Implementation Guidelines |
| ------------- | ----------------------------------------------------------------------------------------------------        | ---------  | ---------------------------------------------------------- | ------------------------- |
| mrn           | Medical record number is the unique person identifier within a site and should never leave the site.        | char(\*)   | Unique to each patient at each site                        | Should never leave the site |
|excl_from_pt_contact_studies  |Exclude patient from studies with patient contact (telephone, e-mail, text, in-person, etc).  | numeric(\*)| 1 if this exclusion applies to this person and 0 otherwise.| |
|excl_from_data_only_studies   |Exclude patient from "data only" studies.                                                     | numeric(\*)| 1 if this exclusion applies to this person and 0 otherwise. | |
|excl_from_chart_review_studies|Exclude patient from studies that plan to do chart reviews.                                   | numeric(\*)| 1 if this exclusion applies to this person and 0 otherwise. | |

## Primary Key
MRN

## Foreign Keys

|Source Variable|Target Table|Target Variable<br>(Primary Key)|Orphans allowed?|
|---------------|------------|--------------------------------|----------------|
|mrn            |Demographics|mrn                             |No              |

## Anticipated Questions

Q: What if I want to denote exclusions other than the 3 in the official fields?

A: For now at least, those should be signified in site-specific enhancement fields (like e.g., `kpwa_do_not_email`).

## Anticipated Impacts

### On Users

We hope/expect this table to enable & encourage the practice of excluding people from research cohorts as early in the process as possible.

### On Implementers

We expect that even outside the CESR (Kaiser) sites this sort of data is routinely kept and used to respect the wishes of implementing organizations' patients and members. We certainly hope implementing this table would be a low-effort endeavor.

### On The Workgroup

The workgroup will need to incorporate this new table into the QA package. This too should be a fairly low-effort operation.

# Request For Feedback
## For Scientific Users
1. How do you expect these changes to affect your current program of research?
2. How valuable is this information to you?
3. Are the new fields proposed:
    1. conceptually coherent?
    2. clearly defined?
    3. likely to be useful?

## For Implementers

1. Can you obtain the information needed for the new fields?
1. Do you have other useful, relevant information in your source data that would not be well accomodated by the proposed fields?
3. Are the new fields proposed
    1. conceptually coherent?
    1. clearly defined?
    1. likely to be useful?
3. Can you think of a better way of storing this data for research use?
