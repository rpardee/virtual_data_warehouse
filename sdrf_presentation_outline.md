# Github and the HCSRN

## Synopsis

The HCSRN’s VDW Implementation Group has gone through several evolutions for managing its main intellectual property—namely, our data specifications and associated implementation guidelines. In this presentation Roy and Al will describe a possible next step in that evolution: moving it to GitHub.  We will describe what GitHub is, what makes it a good fit for the VIG, give a short primer on its use, and demonstrate what using it for our specifications might look like.

## Objectives

3. introduce github
2. describe our specifications
2. review markdown format
1. Illustrate (with demo) what our process(es) would be if we were to switch to markdown on github.com for our specifications
4. discuss pros and cons

## Introduction

1. Github is a web publishing platform

  * it is a medium for publishing and distributing text-based intellectual property with:
    * an emphasis on collaboration and remixing
    * tracking of history and individuals' contributions
    * robust security
    * and associated project management tools like:
      * issue tracking
      * documentation

   * it is currently the number one source for open-source software, supplanting sourceforge and everything that's come before it

   * So it's popular enough to draw a huge audience. Is literally "where the cool kids are".

   * while it is optimized for software projects, it actually has broad applicability outside that domain
      * [Municipal code](https://arstechnica.com/tech-policy/2018/11/how-i-changed-the-law-with-a-github-pull-request/)
      * [Crowdsourcing historical information on slave-holding congressmen](https://github.com/washingtonpost/data-congress-slaveowners)
      * [Covid-19 data](https://github.com/nytimes/covid-19-data)
      * [Covid POLICY data](https://github.com/OxCGRT/USA-covid-policy)
      * [Health services research code lists](https://github.com/MHResearchNetwork/MHRN-Central)
      * [neveragain.tech](https://neveragain.tech/)
      * [I'm using it to negotiate data requests on a project](https://github.com/kaufman-lab/act-ap-requests)--document its holdings.
         * for that, "issues" are data requests. We have a template prompting requestors to include all the relevant information
         * discussion attached to each one fleshes out exactly what the requestor wants/can have.

1. Why our specs are important
   * Because our data warehouse is Virtual, no single person can see/touch the whole thing at any given time.
   * There's not really anything enforcing the structure/content of our implementations
   * Even if you can find out everything's completely synced and legit today, that could change by tomorrow.
      * Bugs get fixed.
      * Data gets added
      * Source systems get changed out from under implementers
      * Bugs get introduced
   * The specs are our North Star in all this.

2. History of HCSRN Spec formats
   1. word documents in e-mail
   2. word documents on a web portal (uncoordinated)
   3. harmonized word documents on a web portal (or was it a wiki? I don't actually remember)
   4. excel spreadsheet

2. Why where we keep them is important
   * canonicality (Tina Belcher equestranauts example)
   * synchronizing the shared delusion of the VDW
   * accessibility

3. Why the web is an excellent choice for displaying our specs
   * anybody can get at it
   * easy to bookmark
   * no need to wonder if your colleague is seeing the same thing as you--you're reading the exact same files.
   * no waiting for a proprietary program like excel to launch.

4. But--HTML is kind of a pain to write & maintain
  ::screenshot of inspect view of say, https://datawranglr.com/d3_demo/enrollments.html::
  * so flexible, which is awesome and horrible
  * you can use any sort of editor (notepad!) but have to know the tags, make sure they're balanced, etc.
  * certainly not beyond the abilities of anybody on this call, but--it's a pain. Especially if you want it to look pretty.
  * Enter _MARKDOWN_ **markdown**

5. Markdown

Markdown is just a set of conventions for formatting a plain text document that makes it trivially simple to output as actual HTML.

* without having to worry about what the tags are called, or balancing them
* without having to use fancy or pricey software
* or pay any licensing fees
* or worry that your reader won't have the same version of the software as you
* which is understood the world over.

You can create markdown on any device running any operating system.

No horrible horrible freedom--you're limited to just a few common formatting options (headings, italics, bold, tables, lists).

6. Tools for working with markdown

  * [ghostwriter](https://wereturtle.github.io/ghostwriter/)
  * [dillinger.io](https://dillinger.io/)

6. You probably already know it

Markdown is one of those technologies whose simplicity and openness has caused people to integrate it into other technologies.  If you've ever used:
* reddit
* slack
* discord

> "The more constraints one imposes, the more one frees one's self of the chains that shackle the spirit."
- Igor Stravinsky

## The Demo

Create new repo virtual_data_warehouse under @kaiser-roy
Include only the md's--not the built HTML versions (add \*.html to .gitignore)
Replay some of the edits made to the specs

Show:
   navigation from spec to spec



3. Audiences
   1. implementers
   3. study programmer-users
   2. investigator-users
   4. funders?
   5. other external ppl?






