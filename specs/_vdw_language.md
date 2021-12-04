# PATIENT LANGUAGES
Version = 5.0.0  Date = 11/16/2021 StdVar = &\_vdw\_language

## Subject Area Description
The LANGUAGE table contains information on the languages that patients speak and write. There is one record per person per known language. People on whom you have no language information should not be included in the table.

| Variable Name | Definition                                                                                           | Type(Len) | Values                                                                                                                                  | Implementation Guidelines |
| ------------- | ---------------------------------------------------------------------------------------------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| MRN           | Medical record number is the unique patient identifier within a site and should never leave the site | char(\*)  | Unique to each patient at each site                                                                                                     |                           |
| LANG\_ISO     | A code signifying the language.                                                                      | char(3)   | As defined by [ISO-639-2](http://www.loc.gov/standards/iso639-2/langhome.html) or 'unk' for unknown<br>Note that value set is lowercase.|                           |
| LANG\_USAGE   | How the person uses this language.                                                                   | char(1)   | S = Spoken/signed<br>W = Written<br>B = Both spoken and written<br>U = Unknown                                                          |                           |
| LANG\_PRIMARY | For spoken languages, whether this is the person's primary spoken language.                          | char(1)   | Y = Yes<br>N = No<br>U = Unknown                                                                                                        |                           |

## Primary Key
MRN + LANG_ISO

## Foreign Keys

|Source Variable|Target Table|Target Variable<br>(Primary Key)|Orphans allowed?|
|---------------|------------|--------------------------------|----------------|
|MRN            |Demographics|MRN                             |No              |

## Comments

Code to pull ISO specification from the web
Note: This may or may not work depending on SAS set up and website changes.
```sas
* Create file ref pointing to the URL;
*filename langiso url ""http://www.loc.gov/standards/iso639-2/ISO-639-2_utf-8.txt"";
* In case anyone would need UTF-8;
filename langiso url ""http://www.loc.gov/standards/iso639-2/ISO-639-2_8859-1.txt"";

* Import data from web site ;
PROC IMPORT DATAFILE = ""langiso""
            OUT = lang_iso1
            DBMS = dlm
            REPLACE;
            delimiter='|';
            GETNAMES = no;
RUN;

* Create file to match to local names ;
data lang_iso (keep = iso639_2 name iso_name);
  set lang_iso1 (rename = (var1 = iso639_2 var4 = name));
  iso_name = name;
run;

```
