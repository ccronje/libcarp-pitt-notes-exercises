---
title: "Advanced OpenRefine functions"
teaching: 20
exercises: 10
questions:
- "How do I fetch data from an Application Programming Interface (API) to be used in OpenRefine?"
- "How do I reconcile my data by comparing it to authoritative datasets"
- "How do I install extensions for OpenRefine"
objectives:
- "Introduce how to use URLs to fetch data from the web based on what's in an OpenRefine project"
- "Introduce how to parse JSON data returned by web services"
- "Introduce how to use Reconciliation services"
- "Introduce OpenRefine extensions"
keypoints:
- "OpenRefine can look up custom URLs to fetch data based on what's in an OpenRefine project"
- "Such API calls can be custom built, or one can use existing Reconciliation services to enrich data"
- "OpenRefine can be further enhanced by installing extensions"
---

## Looking up data from an URL

* OF can retrieve data from the web
* As an example, you can look up names against the Virtual International Authority File (VIAF), and retrieve additional information such as dates of birth/death and identifiers.
* Typically this is a two step process - firstly a step to retrieve data from a remote service, and secondly to extract the relevant information from the data you have retrieved.
* To retrieve data from an external source, from the drop down menu at a column heading use the option `Edit column->Add column by fetching URLs`.

---

This will prompt you for a GREL expression to create a URL. Usually this would be a URL that uses existing values in your data to build a query. When the query runs OpenRefine will request each URL (for each line) and retrieve whatever data is returned (this may often be structured data, but could be simply HTML).

The data retrieved will be stored in a cell in the new column that has been added to the project. You can then use OpenRefine transformations to extract relevant information from the data that has been retrieved. Two specific OpenRefine functions used for this are:

* `parseHtml()`
* `parseJson()`

The 'parseHtml()' function can also be used to extract data from XML.

The next exercise demonstrates this two stage process in full.

---

>## Exercise 11a: Retrieving journal details from CrossRef via ISSN
>Because retrieving data from external URLs takes time, this exercise targets a single line in the data. In reality you would want to run this over many rows (and probably go and do something else while it ran)
>
>* Select a single row from the data set which contains an ISSN by:
>    * Clicking the star icon for the relevant row in the first column
>    * Facet by Star
>    * Choose the single row
>* In the ISSN column use the dropdown menu to choose 'Edit column->Add column by fetching URLs'
>* Give the column a name e.g. "Journal details"
>* In the expression box you need to write some GREL where the output of the expression is a URL which can be used to retrieve data (the format of the data could be HTML, XML, JSON, or some other text format)
>
>In this case we are going to use the CrossRef api (read more about the CrossRef service at >[http://www.crossref.org](http://www.crossref.org), read more about the API we are going to use at >[https://github.com/CrossRef/rest-api-doc/blob/master/rest_api.md)](https://github.com/CrossRef/rest-api-doc/blob/master/rest_api.md))
>
>The syntax for requesting journal information from CrossRef is ```http://api.crossref.org/journals/{ISSN}``` where {ISSN} is replaced with the ISSN of the journal
>
>* In the expression box type the GREL `"http://api.crossref.org/journals/"+value`
>* Click `OK`
>
>You should see a message at the top on the OpenRefine screen indicating it is fetching some data, and how far it has >got. Wait for this to complete. Fetching data for a single row should take only ten seconds or so, but fetching data for >all rows will take longer. You can speed this up by modifying the "Throttle Delay" setting in the 'Add column by >fetching URLs' dialog which controls the delay between each URL request made by OpenRefine. This is defaulted to a >rather large 5000 milliseconds (5 seconds).
>
{: .challenge}


>## Exercise 11b: Parsing the results from crossref
>At this point you should have a new cell containing a long text string in a format called 'JSON' (this stands for  JavaScript Object Notation, although very rarely spelt out in full).
>
>OpenRefine has a function for extracting data from JSON (sometimes referred to as 'parsing' the JSON). The 'parseJson' >function is explained in more detail at >[https://github.com/OpenRefine/OpenRefine/wiki/GREL-Other-Functions](https://github.com/OpenRefine/OpenRefine/wiki/GREL-Other-Functions).
>
>1. In the new column you've just added use the dropdown menu to access 'Edit column->Add column based on this column'
>2. Add a name for the new column e.g. "Journal Title"
>3. In the Expression box type the GREL `value.parseJson().message.title`
>4. You should see in the Preview the Journal title displays
>
>The reason for using `Add column based on this column` is simply that this allows you to retain the full JSON and >extract further data from it if you need to. If you only wanted the title and did not need any other information from >the JSON you could use `Edit cells->Transform...` with the same GREL expression.
{: .challenge}

---

## Reconciliation services

* Allow you to lookup terms from your data in OpenRefine against external services, and use values from the external services in your data.
* Reconciliation services can be more sophisticated and often quicker than using the method described above to retrieve data from a URL
* There are a few services where you can find an OpenRefine Reconciliation option available. For example WikiData has a (fledgling) reconciliation service at [https://tools.wmflabs.org/wikidata-reconcile/](https://tools.wmflabs.org/wikidata-reconcile/).
    * As of OF 2.7 this is a default option

---

* However, to use the ‘Reconciliation’ function in OpenRefine requires the external resource to support the necessary service for OpenRefine to work with, which means unless the service you wish to use supports such a service you cannot use the ‘Reconciliation’ approach.
* People have built reconciliation services
* One of the most common ways of using the reconciliation option in OpenRefine is with an extension (see below for more on extensions to OpenRefine) can use linked data sources for reconciliation. The extension is called "RDF Refine" and can be downloaded from [http://refine.deri.ie](http://refine.deri.ie).

---

## Reconciliation services

More info:
* For more information on using Reconciliation services see [https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation-Service-API](https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation-Service-API)
* There also exist extensions to do reconciliation against local data such as csv files (see [http://okfnlabs.org/reconcile-csv/](http://okfnlabs.org/reconcile-csv/)) and maintained lists of values (see [http://okfnlabs.org/projects/nomenklatura/index.html](http://okfnlabs.org/projects/nomenklatura/index.html)).

---

>## Exercise 12: Reconcile Publisher names with VIAF IDs
>In this exercise you are going to use the VIAF Reconciliation service written by [Jeff Chiu](https://twitter.com/absolutelyjeff). Jeff offers two ways of using the reconciliation service - either via a public service he runs at [http://refine.codefork.com/](http://refine.codefork.com/), or by installing and running the service locally using the instructions at [https://github.com/codeforkjeff/refine_viaf](https://github.com/codeforkjeff/refine_viaf).
>
>If you are going to do a lot of reconciliation, please install and run your own local reconciliation service - the >instructions at [https://github.com/codeforkjeff/refine_viaf](https://github.com/codeforkjeff/refine_viaf) make this reasonably straightforward.
>
>Once you have chosen which service you are going to use:
>
>1. In the Publisher column use the dropdown menu to choose `Reconcile->Start Reconciling`
>2. If this is the first time you've used this particular reconciliation service, you'll need to add the details of the service now:
>    * Click `Add Standard Service...` and in the dialogue that appears enter:
>        * <http://refine.codefork.com/reconcile/viaf> for Jeff's public service
>        * <http://localhost:8080/reconcile/viaf> if you are running the service locally
>3. You should now see a heading in the list on the left hand side of the Reconciliation dialogue called **VIAF Reconciliation Service**
>4. Click on this to choose to use this reconciliation service
>5. In the middle box in the reconciliation dialogue you may get asked what type of 'entity' you want to reconcile to - that is, what type of thing are you looking for. The list will vary depending on what reconciliation service you are using.
>    * In this case choose "Corporate Name" (it seems like the VIAF Reconciliation Service is slightly intelligent about this and will only offer options that are relevant)
>6. In the box on the righthand side of the reconciliation dialogue you can choose if other columns are used to help the reconciliation service make a match - however it is sometimes hard to tell what use (if any) the reconciliation service makes of these additional columns
>7. At the bottom of the reconciliation dialogue there is the option to "Auto-match candidates with high confidence". This can be a time saver, but in this case you are going to uncheck it, so you can see the results before a match is made
>8. Now click 'Start Reconciling'
>    
>    Reconciliation is an operation that can take a little time if you have many values to look up. However, in this case there are only 6 publishers to check, so it should work quite quickly.
>
>    Once the reconciliation has completed two Facets should be created automatically:
>        * `Publisher: Judgement`
>        * `Publisher: best candidate's score`
>
>    These are two of several specific reconciliation facets and actions that you can get from the 'Reconcile' menu (from the column drop down menu).
>    
>9. Close the `Publisher: best candidate's score` facet, but leave the `Publisher: Judgement` facet open
>
>    If you look at the Publisher column, you should see some cells have found one or more matches - the potential matches are shown in a list in each cell. Next to each potential match there is a 'tick' and a 'double tick'. To accept a reconciliation match you can use the 'tick' options in cells. The 'tick' accepts the match for the single cell, the 'double tick' accepts the match for all identical cells.
>    
>10. Create a text facet on the Publisher column
>11. Choose 'International Union of Crystallography'
>    * In the Publisher column you should be able to see the various potential matches.
>    * Clicking on a match will take you to the VIAF page for that entity.  
>12. Click a 'double tick' in one of the Publisher column cells for the option "International Union of Crystallography"
>2. This will accept this as a match for all cells - you should see the other options all disappear
>3. Check the 'Publisher: Judgment' facet. This should now show that 858 items are 'matched' (if this does not update, try refreshing the facets)
>    
>    We could do these one by one, but if we are confident with matches, there is an option to accept all:
>    
>4. Remove all filters/facets from the project so all rows display
>5. In the Publisher column use the dropdown menu to choose `Reconcile->Actions->Match each cell to its best candidate`
>    
>    There are two things that reconciliation can do for you. Firstly it gets a standard form of the name or label for the entity. Secondly it gets an ID for the entity - in this case a VIAF id. This is hidden in the default view, but can be extracted:
>    
>6. In the Publisher column use the dropdown menu to choose `Edit column->Add column based on this column...``
>7. Give the column the name 'VIAF ID'
>8. In the GREL expression box type `cell.recon.match.id`
>9. This will create a new column that contains the VIAF ID for the matched entity
{: .challenge}

## Extensions
The functionality in OpenRefine can be enhanced by ‘extensions’ which can be downloaded and installed to add functionality to your OpenRefine installation.

A list of Extensions (not necessarily complete) is given on the OpenRefine downloads page at [http://openrefine.org/download.html](http://openrefine.org/download.html)

One of these extensions tries to work around the limitation of Reconciliation services described above, by making it possible to use a reconciliation service against ‘linked data’ sources which have SPARQL endpoints. For more information on this see the ‘RDF Extension’ at [http://refine.deri.ie](http://refine.deri.ie). An example of how this works is given in more detail at [http://refine.deri.ie/showcases](http://refine.deri.ie/showcases).

## Using the ‘cross’ function to lookup data in other OpenRefine projects
As well as looking up data in external systems using the methods described above, it is also possible to look up data in other OpenRefine projects on the same computer. This is done using the ‘cross’ function.

The ‘cross’ function takes a value from the OpenRefine project you are working on, and looks for that value in a column in another OpenRefine project. If it finds one or more matching rows in the second OpenRefine project, it returns an array containing the rows that it has matched.

As it returns the whole row for each match, you can use a transformation to extract the values from any of the columns in the second project.

You can use this function to compare the contents of two OpenRefine projects, or to use data between the two projects.

The [VIB-Bits extension](https://www.bits.vib.be/index.php/software-overview/openrefine) adds a number of very useful functions to OpenRefine including a way of using the 'cross' function with simply point-and-click functionality which makes looking up data from other projects significantly simpler.
