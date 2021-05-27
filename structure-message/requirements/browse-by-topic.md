# Requirements: Browse by topic

## Summary

Browsing a list of *topics* (i.e. statistical domains) is a common way for users to find the data they want in a data source. This section documents the SDMX artefacts needed to support this use case with SDMX-JSON, as well as the steps involved.

## Artefacts involved

The following SDMX artefacts are needed to support this use case:

- Category Scheme
- Dataflow
- Actual Data Constraint
- Data Structure Definition
- Concept Scheme
- Codelist

For detailed information about these artefacts, please refer to the [SDMX information model](http://sdmx.org/wp-content/uploads/2011/08/SDMX_2-1-1_SECTION_2_InformationModel_201108.pdf).

For detailed information about the query syntax used in the examples below, please refer to the [SDMX RESTful specification](https://github.com/sdmx-twg/sdmx-rest/tree/master/doc).

## Choreography

It is possible to support the requirement with 2 queries, one to get the list of statistical domains and the dataflows attached to them and one to build the dimension filters for the selected dataflow.

### Query 1: Get the list of statistical domains and the dataflows attached to them

Required artefacts: Category Scheme, Dataflow

The starting point is to retrieve the list of *topics* for which data are available. These are known as `category schemes` in SDMX. The `dataflows` attached to each category give information about the data that can be retrieved for each of the *topics*. Both the category schemes and the dataflows can be retrieved using a categoryscheme query, along with the `references` parameter:

```
https://ws-entry-point/categoryscheme?references=parentsandsiblings
```

Alternatively, if only the topics of a particular category scheme are needed, it is possible to only retrieve that particular category scheme (assuming we also know the maintenance agency for the category scheme):

```
https://ws-entry-point/categoryscheme/agency-id/category-scheme-id?references=parentsandsiblings
```

The response will contain the `category scheme(s)` and its `categories`, as well as the dataflows attached to them.

The screenshot below shows an example of the type of user interface (a treeview control in this case) that can be built from a category scheme, using the [ECB Statistical Data Warehouse](https://sdw.ecb.europa.eu) as an example.

![Treeview control](https://github.com/sdmx-twg/sdmx-json/raw/master/structure-message/requirements/img/cs-treeview.png)

The screenshot below displays the category scheme as a list box, similar to the typical control displayed by mobile apps. The example is taken from the ECB statistical tablet app.

![List of statistical domains](https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/structure-message/requirements/img/cs-list.png)

The client typically needs to display the names of the categories. In addition, some clients might also want to display the descriptions of the categories and the name of the category schemes and possibly the name of its maintenance agency. Some clients might also want to display the number of dataflows attached to each category.

Once a user has selected a category, the client could display the list of dataflows attach to the category. The screenshot below shows an example of the type of user interface (a list box in this case) that can be built from the response. The example is taken from the ECB statistical tablet app.

![List of dataflows](https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/structure-message/requirements/img/df-list.png)

The client typically needs to display the names of the dataflows. In addition, some clients might also want to display the descriptions of the dataflows and the name of the selected category.

In order to perform the next query, clients will need the full references to the dataflows (id, agency id and version).

### Query 2: Find data in the selected dataflow, using concept filters

Required artefacts: Actual Data Constraint, Data Structure Definition, Concept Scheme, Codelist

Once a user has selected a dataflow, the web service client will need to give users the possibility to retrieve data using *filters*. The list(s) of values (and their localised names) for which data exist can be found through a data availability query in the `actual data constraints`. 

```
https://ws-entry-point/availability/dataflow/{agency-id}/{dataflow-id}/{dataflow-version}?references=all
```

The response will contain the `actual data constraint` and related structures, such as data structure definition, (partial) concept scheme(s) and (partial) codelists, for the selected dataflow. The concept scheme(s) and codelists only contain the needed concepts and codes (for which data exists).

The screenshot below shows an example of the type of user interface (a list box for the list of concepts, and a collection of check boxes for the list of allowed values for each of the concepts in this example) that can be built from the response. The example is taken from the ECB statistical tablet app.

![Dimension filters](https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/structure-message/requirements/img/df-filters.png)

The client typically needs to display the name of the retrieved concepts (Frequency, Reference area, etc.) from the (partial) concept scheme(s), of the allowed values for each concept (e.g.: Annual, Monthly, etc.) from the actual data constraint and of their localised names from the (partial) codelists according to the data structure definition. In addition, some clients might also want to display the descriptions of the concepts and codes, as well as the code ids.

Optionally, user interfaces can opt for dynamically adjusting the list of values per concept (dimension component) according to the data availability for the currently selected values in the other concepts by using the more advanced query that takes the current data selection into account:

```
https://ws-entry-point/availability/dataflow/{agency-id}/{dataflow-id}/{dataflow-version}/{data-query}/{componentId}
```

In order to retrieve the data matching the user's filters, the web service clients will need the full references to the dataflow (id, agency id and version) and the code ids for the dimension values.

Once the filters' selection is finished, the client may provide a list of matching series like the one shown on the screen below, but this is typically the result of a data query:

![Matching series](https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/structure-message/requirements/img/series-list.png)

For detailed information on data availability queries, see [here](https://github.com/sdmx-twg/sdmx-rest/blob/master/doc/availability.md).
