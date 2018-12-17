Motivation
==========
This proposal is intended to cure two defects in the current _Demographics_ specification:

1. The current spec conflates the distinct concepts of biological sex and gender identity.
2. The current spec does not accomodate patient sexual orientation information available (or soon to be available) at implementing HCSRN sites.

Proposed Changes
================
Existing Spec
-------------
The current spec sets out a single field called 'gender' which may hold _either_ gender (subjective gender identity) _or_ sex (biological sex), depending on which data is available at the site.  The spec currently reads as follows:

|Variable Name|Definition|Type(Len)|Values|Implementation Guidelines|
|-------------|----------|---------|------|-------------------------|
|gender|The person's gender and/or sex;  if both gender and sex are known, this variable should hold gender|char(1)|M = Male<br>F = Female<br>O = Other including transgendered<br>U = Unknown| |

Changes
-----------
We propose to:

  1. change the definition of gender to hold *only* gender identity information (as of last ascertainment)
  2. add a new fields to hold:
    1. sex at birth
    2. sexual orientation (as of last ascertainment)

|Variable Name|Definition|Type(Len)|Values|Implementation Guidelines|
|-------------|----------|---------|------|-------------------------|
|natal_sex|The person's physical sex at birth|char(1)|M = Male<br>F = Female<br>U = Unknown<br>O = Other| |
|gender|The person's gender identity as subjectively experienced, on last ascertainment|char(1)|M = Male<br>F = Female<br>N = Non-binary<br>T = Transgender<br>O = Other<br>U = Unknown| |
|sexual_orientation|The person's person's sexual identity in relation to the gender to which they are attracted, on last ascertainment.|char(1)|T = Heterosexual<br>M = Homosexual<br>B = Bisexual<br>O = Other<br>U = Unknown||


Notes
=====
The concepts of gender identity and sexual orientation are not static, but change over time.  This proposal does not seek to accomodate full information regarding what changes ocurred when, but rather just represent the current, best-known information at the time the table is created or updated.  This should be sufficient to serve researcher's needs to identify populations of interest, without unduly burdening users who rely on Demographics' one-record-per-person structure.

Anticipated Impacts
===================

On Users
--------
It would be hard to overstate the degree to which the existing **gender** variable is used.  There are probably far more applications that access **gender** than those that do not, and so we don't take redefining it lightly.  That said, we consider the proposal a distinct improvement in that under the current spec, users don't actually know whether they are getting the preferred concept gender, or rather biological sex.

We expect that the changes as proposed will result in a higher number of people with 'Unknown' gender, as implementers move sex information to the new natal_sex field.  Thus, there will be significant amounts of older code that will need to be revised (including 4 standard macros) for cases where either sex is really what was desired, or where the existing combination-concept scheme is preferable to a pure gender field.  But the revisions will by and large be easy to make, and we judge that the short-term pain is worth the longer-term gain of conceptual clarity and additional information.

On Implementers
---------------
It will require work to identify sources for the new variables and incorporate them into existing ETL code for the Demographics file, as well as possibly disentangling gender from sex.  We anticipate that at most sites this will be a low-to-medium effort process (do please comment to let us know if this is the case).


On the Workgroup
----------------
We will need to revise the list of valid values for **gender** in the QA program, and add checks and descriptives for the new fields.

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
