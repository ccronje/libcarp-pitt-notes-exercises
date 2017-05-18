---
title: "Basic OpenRefine functions II:"
teaching: 20
exercises: 40
questions:
- "How do I edit my data based on filters and facets?"
- "How do I use transformations to programmatically edit my data?"
- "How do I use GREL, the General Refine Expression Language?"
- "How do I save and reuse a set of operations for use in subsequent projects?"
- "What are the data formats supported by OpenRefine and why should I care?"
objectives:
- "Introduce common transformations"
- "Introduce GREL, the General Refine Expression Language"
- "Explain how to write one's own transformations using GREL"
- "Explain how to use Undo and Redo to retrace ones' steps"
- "Explain how to use Extract and Apply to reuse a set of operations"
- "Introduce data formats"
- "Introduce Boolean values and arrays, and how to run transformations based on them"
keypoints:
- "You can alter data in OpenRefine based on specific instructions"
- "You can expand the data editing functions that are built-in into OpenRefine by building your own"
---

## Transforming data

---
1. Through facets, filters and clusters OpenRefine offers ways of getting an overview of your data, and making changes where you want to standardize terms used to a common set of values.
2. Sometimes there will be changes you want to make to the data that cannot be achieved in this way.
3. In these cases, OpenRefine supports 'Transformations' which are ways of manipulating data in columns.
    * Transformations are written in a language called 'GREL' (Google Refine Expression Language, General Refine Expression language).
    * Similar to Excel Formula, but they tend to focus on text manipulations rather than numeric functions.
---

## Uses (tidy data)

* Splitting variables that are in a single column into multiple columns (e.g. splitting an address into multiple parts)
* Standardizing the format of data in a column without changing the values (e.g. removing punctuation or standardizing a date format)
* Extracting a particular type of data from a longer text string (e.g. finding ISBNs in a bibliographic citation)

---
## GREL

* 'GREL' (Google Refine Expression Language).
* [https://github.com/OpenRefine/OpenRefine/wiki/Google-refine-expression-language](https://github.com/OpenRefine/OpenRefine/wiki/Google-refine-expression-language)
* This tutorial covers only a small subset of the commands available.
---

## Common Transformations

* Available through column drop downs

Common Transformation  | Action | GREL expression
--------------------| ------------- | -------------
To Uppercase| Converts the current value to uppercase | `value.toUppercase()`
To Lowercase| Converts the current value to lowercase | `value.toLowercase()`
To Titlecase| Converts the current value to titlecase (i.e. each word starts with an uppercase character and all other characters are converted to lowercase) | `value.toTitlecase()`
Trim leading and trailing whitespace | Removes any 'whitespace' characters (e.g. spaces, tabs) from the start or end of the current value | `value.trim()`

>## Exercise 7: Correct Publisher data
>* Create a text facet on the Publisher column
>* Note that in the values there are two that look identical - why does this value appear twice? (Hint: click edit on each value and see how they differ)
>* Now, correct the difference between the two values by filling in the blank:  
> `Edit cells->Common transforms->_______________`
>
>>## Solution
>>* On the publisher column use the dropdown menu to select `Edit cells->Common transforms->Trim leading and trailing whitespace`
>>* Look at the publisher facet now - has it changed? (if it hasn't changed try clicking the Refresh option to make sure it updates)
>{: .solution}
{: .challenge}

---
## Writing Transformations in GREL

* Select the column on which you wish to perform a transformation and choose `Edit cells->Transform…`
* Note the box where you write the expression
* Needs to be valid GREL (if you don't get a preview of the changed data it isn't GREL)

---
## GREL function syntax

GREL functions are written by giving a value of some kind (a text string, a date, a number etc.) to a GREL function. Some GREL functions take additional parameters or options which control how the function works. GREL supports two types of syntax:

* `value.function(options)`
* `function(value, options)`

---

* Either is valid, and which is used is completely down to personal preference. In these notes the first syntax is used.
* Next to the 'Preview' option are options to view:

* History - a list of transformations you've previously used with the option to reuse them immediately or to 'star' them for easy access
* Starred - a list of transformations you've 'starred' via the 'History' view
* Help - a list of all the GREL functions and brief information on how to use them

---

>## Exercise 8: Put titles into Title Case
>1. Facet by publisher
>2. Select "Akshantala Enterprises" and "Society of Pharmaceutical Technocrats"
>    * To select multiple values in the facet use the `Include` link that appears to the right of the facet
>3. See that the Titles for these are all in uppercase
>4. Click the dropdown menu on the Title column
>5. Choose 'Edit cells->Transform...'
>6. Now using the `toTitlecase()` function, transform those titles to title case. (Remember you can use a GREL function two ways: `value.function(options)` or `function(value, options)`)
>
>>## Solution
>>1. Facet by publisher
>>2. Select "Akshantala Enterprises" and "Society of Pharmaceutical Technocrats"
>>    * To select multiple values in the facet use the 'Include' link that appears to the right of the facet
>>3. See that the Titles for these are all in uppercase
>>4. Click the dropdown menu on the Title column
>>5. Choose 'Edit cells->Transform...'
>>6. In the Expression box type `value.toTitlecase()`
>>7. In the Preview note that you can see what the affect of running this will be
>>8. Click 'OK'
>{: .solution}
{: .challenge}

---

## Undo and Redo
* OpenRefine lets you undo, and redo, any number of steps you have taken in cleaning the data
* Feel free to try things out and undo/redo
* Open OF and see how you can unwind the transformations
* If you unwind to a point and then create new transformations from that point the 'grayed out' previous transformations will be lost
* if you 'undo' a set of steps and then start doing new transformations, the greyed out steps will disappear and you will no longer have the option to 'redo' these steps.

---

## Undo and Refine -- Getting the JSON

* Click extract JSON - save in file
* You can apply work from someone else or share your own with others
* Use the 'apply' button to do this

---
## Exporting data
* use the export button to export the data you've been working on.
* The export options are accessed through the 'Export' button at the top right of the OpenRefine interface
* Export formats support include HTML, Excel and comma- and tab-separated value (csv and tsv).
* You can also write a custom export, selecting to export specific fields, adding a header or footer and specifying the exact format.

## Data Types in OF

Every piece of data in OpenRefine has a **type**. The most common **type** is a **string** - that is a piece of text.

The data types supported are:

* String
* Number
* Date
* Boolean
* Array

---

* there are other data types available and transformations let you convert data from one type to another where appropriate
* So far we've been looking only at 'String' type data.
* Much of the time it is possible to treat numbers and dates as strings.
* For example in the Date column we have the date of publication represented as a String.
* However, some **operations and transformations** only work on 'number' or 'date' type operations.
* The simplest example is sorting values in numeric or date order. To carry out these functions we need to convert the values to a date or number first.

>## Exercise 9: Reformat the Date
>1. Make sure you remove all Facets and Filters
>2. On the Date column use the dropdown menu to select `Edit cells->Common transforms->To date`
>3. Note how the values are now displayed in green and follow a standard convention for their display format (ISO8601) - this indicates they are now stored as date data types in OpenRefine. We can now carry out functions that are specific to Dates
>4. On the Date column dropdown select `Edit column->Add column based on this column`. Using this function you can create a new column, while preserving the old column
>5. In the `New column name` type `Formatted Date`
>6. In the `Expression` box type the GREL expression `value.toString("dd MMMM yyyy")`
{: .challenge}

---

## Booleans
* Booleans and Arrays are data types that are more often used while manipulating data in a GREL expression than for actually storing in a cell (in fact, Arrays cannot be stored in a cell in OpenRefine).
* `Boolean` is a binary value that can either be 'true' or 'false':

~~~
value.contains("test")
~~~

Such tests can be combined with other GREL expressions to create more complex transformations. For example, to carry out a further transformation only if a test is successful. The GREL transformation `if(value.contains("test"),"Test data",value)` replaces a cell value with the words "Test data" only *if* the value in the cell contains the string "test" anywhere.

---

## Arrays
* An 'Array' is a collection of values, represented in Refine by the use of square brackets containing a list of values surrounded by quotes and separated by commas.

E.g.:
["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"]

* Can be sorted, de-duplicated, and manipulated in other ways in GREL expressions, but cannot appear directly in an OpenRefine cell.
* Arrays in OpenRefine are usually the result of a transformation. For example the 'split' function takes a string, and changes it into an array based on a 'separator'

---

## Arrays

For example if a cell has the value:

"Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday"

This can be transformed into an array using the 'split' function

```
value.split(",")
```

And would create;

["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"]

---

##Arrays

This can be combined with array operations like 'sort'. For example, assuming the cell contains the same value as above, then the function

```
value.split(",").sort()
```

would result in an array containing the days of the week sorted in alphabetical order:

["Friday","Monday","Saturday","Sunday","Thursday","Tuesday","Wednesday"]

---

To output a value from an array you can either select a specific value depending on its position in the list (with the first position treated as 'zero'). For example

```
value.split(",")[0]
```

would extract the first value

---

You can also join arrays together to make a 'String'. The GREL expression would look like

```
value.split(",").sort().join(",")
```

Taking the above example again, this would result in a string with the days of the week in alphabetical order, listed with commas between each day.

---

>## Exercise 10: Reverse author names
>In this exercise we are going to use both the Boolean and Array data types.
>If you look at the Authors column, you can see that most of the author names are written in the natural order. However, a few have been reversed to put the family name first.
>
>We can do a crude test for reversed author names by looking for those that contain a comma:
>
>* Make sure you have already split the author names into individual cells using `Edit cells->Split multi-valued cells` (you should have done this in exercise 5)
>* On the Authors column, use the dropdown menu and select `Facet->Custom text facet...`
>    * The Custom text facet function allows you to write GREL functions to create a facet
>* In the Expression box type `value.contains(",")`
>* Click `OK`
>* Since the 'contains' function outputs a Boolean value, you should see a facet that contains 'false' and 'true'. These represent the outcome of the expression, i.e. true = values containing a comma; false = values not containing a comma
>* In this facet select 'true' to narrow down to the author names that contain a comma
>
>Now we have narrowed down to the lines with a comma in a name, we can use the `match` function. The match function allows you to use regular expressions, and output the capture groups as an array, which you can then manipulate.
>
>* On the Authors column use the dropdown menu and select `Edit cells->Transform`
>* In the Expression box type `value.match(/(.*),(.*)/)`  The "/",  means you are using a regular expression inside a >GREL expression. The parentheses indicate you are going to match a group of characters. The ".*" expression will match >any character from 0, 1 or more times. So here we are matching any number of characters, a comma, and another set of any >number of characters.
>* See how this creates an array with two members in each row in the Preview column
>
>To get the author name in the natural order you can reverse the array and join it back together with a space to create the string you need:
>
>* In the Expression box, add to the existing expression until it reads `value.match(/(.*),(.*)/).reverse().join(" ")`
>* In the Preview view you should be able see this has reversed the array, and joined it back into a string
>* Click 'OK'
{: .challenge}
