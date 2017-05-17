---
title: "Notes: Introduction to OpenRefine"
teaching: 15
exercises: 0
questions:
- "What is OpenRefine? What can it do?"
objectives:
- "Explain what the OpenRefine software does"
- "Explain how the OpenRefine software can help work with data files"
keypoints:
- "OpenRefine is 'a tool for working with messy data'"
- "OpenRefine works best with data in a simple tabular format"
- "OpenRefine can help you split data up into more granular parts"
- "OpenRefine can help you match local data up to other data sets"
- "OpenRefine can help you enhance a data set with data from other sources"
---
## What is OpenRefine

> A tool for working with ‘messy’ data

???
* Most of this class will involve working through a series of exercises in open refine.
* You are encouraged to go along at your own pace.
* For some of you this will feel like a lot of material, for others it might not feel like enough. * Remember that the session is **introductory**.
* If you finish early, our end time is not a hard stop, so you may of course leave.
* You might want to use the time to search online for more information or advanced skills guides.
* You may even wish to deepen your own skills by staying around and helping someone else out: there is nothing better for really getting to know something than teaching it to someone else!
* If you don't finish, don't worry, there are no prerequisites between classes and if you have time, you can always work-on and practice Open Refine at home (or even better as part of our work).

---

Open Refine can help you:

* Get an overview of a data set
* Resolve inconsistencies in a data set
* Help you split data up into more granular parts
* Match local data up to other data sets
* Enhance a data set with data from other sources

???

Some common scenarios might be:

* Where you want to know how many times a particular value appears in a column in your data
* Where you want to know how values are distributed across your whole data set
* Where you have a list of dates which are formatted in different ways, and want to change all the dates in the list to a single common date format

---

For example:

| Data you have   | Desired data |
|-----------------|:-------------|
| 1st January 2014| 2014-01-01   |
| 01/01/2014      | 2014-01-01   |
| Jan 1 2014      | 2014-01-01   |
| 2014-01-01      | 2014-01-01   |

???
---
#### Where you have a list of names or terms that differ from each other but refer to the same people, places or concepts. For example:

| Data you have   | Desired data |
|-----------------|:-------------|
| London          | London       |
| London]         | London       |
| London,]        | London       |
| london          | London       |

---

#### Where you have several bits of data combined together in a single column, and you want to separate them out into individual bits of data with one column for each bit of the data.

> Address parts in a single field:

>'University of Wales, Llyfrgell Thomas Parry Library, Llanbadarn Fawr, ABERYSTWYTH, Ceredigion, SY23 3AS, United Kingdom'
>
>"University of Wales", "Llyfrgel Thomas Parry Library", "Llanbadarn Fawr", "ABERYSTWYTH", "Ceredigion",  "SY23 3AS", "United Kingdom"

---
#### Where you want to add to your data from an external data source:

| Data you have   | Date of Birth from VIAF (Virtual International Authority File) | Date of Death from VIAF (Virtual International Authority File) |
|-----------------|:-------------|:-------------|
| Braddon, M. E. (Mary Elizabeth) | 1835 | 1915 |
| Rossetti, William Michael       | 1829 | 1919 |
| Prest, Thomas Peckett           | 1810 | 1879 |

---
