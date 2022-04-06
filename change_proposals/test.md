# Can we make list-to-table work in sublime or github?

<div class="t">

- - **Variable Name**
  - **Definition**
  - **Type(Len)**
  - **Values**
  - **Implementation Guidelines**

- - sexual_orientation[1-3]
  - The person's response(s) to inquiry into their sexual
    orientation
  - char(1)
  - <dl>
    <dt>B</dt>
        <dd>Bisexual</dd>
    <dt>T</dt>
        <dd>Heterosexual</dd>
    <dt>M</dt>
        <dd>Homosexual</dd>
    <dt>A</dt>
        <dd>Asexual</dd>
    <dt>P</dt>
        <dd>Pansexual</dd>
    <dt>Q</dt>
        <dd>Queer</dd>
    <dt>O</dt>
        <dd>Other</dd>
    <dt>D</dt>
        <dd>Does not know</dd>
    <dt>N</dt>
        <dd>Choose not to disclose</dd>
    <dt>U</dt>
        <dd>Not asked/no information</dd>
    </dl>
  - Null values not allowed--use the `U` value to signify
    a lack of information.

    Position does not signify temporal ordering, preference,
       or anything else--it is entirely arbitrary.

    A superset of both PHINVADS
       [PHVS_SexualOrientation_CDC](https://phinvads.cdc.gov/vads/ViewValueSet.action?id=E0004707-45BB-E711-ACE2-0017A477041A)
       value set and LOINC code[76690-7]
       (https://loinc.org/76690-7/).

    Also exceeds [HRSA recommendations and UDS reporting standards](https://data.hrsa.gov/tools/data-reporting).

</div>

<!--- Applies to lists marked as class="t"      --->
<!--- https://pborenstein.com/posts/tablehacks/ --->
div[class~="t"] > ul
    {
    display: table;
    list-style: none;
    margin: 0;
    padding: 0;
    }

div[class~="t"] > ul > li
    {
    display: table-row-group;
    }

div[class~="t"] > ul > li > ul
    {
    display: table-row;
    }

div[class~="t"] > ul > li > ul > li
    {
    display: table-cell;
    padding: 0 .25em;
    border: 1px solid gray;
    border-collapse: collapse ;
    }

