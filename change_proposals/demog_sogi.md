Motivation
==========
This proposal is intended to cure two defects in the current _Demographics_ specification:

1. The current spec combines the distinct concepts of biological sex and gender identity in a single variable.
2. The current spec does not accomodate patient sexual orientation information available (or soon to be available) at a large number of implementing HCSRN sites.

Proposed Changes
================
Existing Spec
-------------
The current spec sets out a single field called 'gender' which may hold _either_ gender (subjective gender identity) _or_ sex (biological sex), depending on which data is available at the site.  The spec currently reads as follows:

|Variable Name|Definition|Type(Len)|Values|Implementation Guidelines|
|-------------|----------|---------|------|-------------------------|
|gender|The person's gender and/or sex;  if both gender and sex are known, this variable should hold gender|char(1)|M = Male<br>F = Female<br>O = Other including transgendered<br>U = Unknown| |

While we expect that the vast majority of values are in fact sex assigned at birth, users have no way of knowing whether any given value is sex or gender.

Changes
-----------
We propose to:

  1. Deprecate/remove the existing **gender** variable.
  2. Add a new variable to hold *only* gender identity information (as of last ascertainment)
  2. add a new fields to hold:
    1. sex at birth
    2. sexual orientation (as of last ascertainment)

|Variable Name|Definition|Type(Len)|Values|Implementation Guidelines|
|-------------|----------|---------|------|-------------------------|
|sex_at_birth|The person's sex as assigned at birth.|char(1)|F = Female<br>M = Male<br>A = Ambiguous<br>N = Not Applicable<br>O = Other<br>U = Unknown|This is <a href="https://phinvads.cdc.gov/vads/ViewValueSet.action?id=06D34BBC-617F-DD11-B38D-00188B398520">PHVS_AdministrativeSex_HL7_2x</a>. Values of 'intersex' should be coded as Ambiguous. Values of 'unsure' should be coded as Other.|
|gender_identity|The person's gender identity as subjectively experienced, on last ascertainment.|char(2)|FF = Female<br>MM = Male<br>FM = Female to Male transsexual<br>MF = Male to Female transsexual<br>GQ = Genderqueer/non-conforming/non-binary<br>OT = Other<br>ND = Chose not to disclose<br>UK = Unknown|Compatible with <a href='https://phinvads.cdc.gov/vads/ViewValueSet.action?id=660779DA-64E9-E611-A856-0017A477041A'>PHVS_GenderIdentity_CDC</a>. Values of 'unsure/questioning' should be coded as Other.|
|sexual_orientation|The person's sexual identity in relation to the gender to which they are attracted, on last ascertainment.|char(1)|H = Heterosexual<br>L = Lesbian or gay<br>B = Bisexual<br>O = Other<br>N = Choose not to disclose<br>D = Do not know|Compatible with <a href = "https://phinvads.cdc.gov/vads/ViewValueSet.action?id=E6EDE311-66E9-E611-A856-0017A477041A">PHVS_SexualOrientation_CDC</a>. Values of 'asexual' should be coded as Other.|

Notes
=====
The concepts of gender identity and sexual orientation are not static, but change over time.  This proposal does not seek to accomodate full information regarding what changes ocurred when, but rather just represent the current, best-known information at the time the table is created or updated.  This should be sufficient to serve researcher's needs to identify populations of interest, without unduly burdening users who rely on Demographics' one-record-per-person structure.

It will be possible to back-translate values from the proposed variables to the old (existing) **gender** scheme.  The workgroup will provide SAS code for doing so (possibly in a macro) once the spec for the new vars is finalized.

Anticipated Impacts
===================

On Users
--------
It would be hard to overstate the degree to which the existing **gender** variable is used.  There are probably far more applications that access **gender** than those that do not, and so we don't take removing it lightly.  That said, we consider the proposal a distinct improvement in that under the current spec, users don't actually know whether they are getting the preferred concept gender, or rather biological sex. If the change is approved, users will have much more control over their applications.

We expect that there will be significant amounts of older code that will need to be revised (including 4 standard macros) for cases where either sex is really what was desired, or where the existing combination-concept scheme is preferable to a pure gender_identity field.  But the revisions will by and large be easy to make, and we judge that the short-term pain is worth the longer-term gain of conceptual clarity and additional information.  One advantage to removing the old **gender** field is that old code that is not rewritten will cause errors, which will call attention to the issue.

On Implementers
---------------
It will require work to identify sources for the new variables and incorporate them into existing ETL code for the Demographics file, as well as possibly disentangling gender from sex.  We anticipate that at most sites this will be a low-to-medium effort process (do please comment to let us know if this is the case).


On the Workgroup
----------------
We will need to revise the QA program to warn (and eventually fail) implementations that still feature the old **gender** field, and add checks and descriptives for the new fields.

Request For Feedback
====================

For Scientific Users
--------------------

1. How do you expect these changes to affect your current program of research?
2. How valuable is this information to you?
3. Are the new variables proposed:
    1. conceptually coherent?
    2. clearly defined?
    3. likely to be useful?

For Implementers
--------------------

1. Can you obtain the information needed for the new variables?
1. Do you have other useful, gender-identity/sexual-orientation-relevant information in your source data that would not be well accomodated by the proposed variables?
3. Are the new variables proposed
    1. conceptually coherent?
    1. clearly defined?
    1. likely to be useful?
