# Requirements: Browse by topic

## Summary

Browsing a list of *topics* (i.e. statistical domains) is a common way for users to find the data they want in a data source. This section documents the SDMX artefacts needed to support this use case with SDMX-JSON, as well as the steps involved.

## Artefacts involved

The following SDMX artefacts are needed to support this use case:

- Category Scheme
- Dataflow
- ContentConstraint

One of the guiding principles of SDMX-JSON is to make the artefacts useful on their own (i.e. standalone). This means that the `ContentConstraint` will embed the codes and concepts used by the constraints, thereby alleviating the need to also retrieve the `DataStructureDefinition`, along with its `ConceptSchemes` and `Codelists`. This not only simplifies the work to be done on the client but also aims to minimise the amount of resources needed (for example, by limiting the size of the message to a minimum). The same principle explains why the SDMX `Categorisations` are not used in this use case. 

For additional information about these artefacts, please refer to the [SDMX information model](http://sdmx.org/wp-content/uploads/2011/08/SDMX_2-1-1_SECTION_2_InformationModel_201108.pdf).

For additional information about the query syntax used in the examples below, please refer to the [SDMX RESTful specification](https://github.com/sdmx-twg/sdmx-rest/tree/master/v2_1/ws/rest/docs).

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

![Treeview control](img/cs-treeview.png)

The screenshot below displays the category scheme as a list box, similar to the typical control displayed by mobile apps. The example is taken from the ECB statistical tablet app.

![List of statistical domains](img/cs-list.png)

The client typically needs to display the names of the categories. In addition, some clients might also want to display the descriptions of the categories and the name of the category schemes and possibly the name of its maintenance agency. Some clients might also want to display the number of dataflows attached to each category.

Once a user has selected a category, the client could display the list of dataflows attach to the category. The screenshot below shows an example of the type of user interface (a list box in this case) that can be built from the response. The example is taken from the ECB statistical tablet app.

![List of dataflows](img/df-list.png)

The client typically needs to display the names of the dataflows. In addition, some clients might also want to display the descriptions of the dataflows and the name of the selected category.

In order to perform the next query, clients will need the full references to the dataflows (id, agency id and version).

### Query 2: Find data in the selected dataflow, using concept filters

Required artefacts: Content Constraint

Once a user has selected a dataflow, the web service client will need to retrieve the `concepts` that are used to structure that dataflow. In addition, the list of allowed values for each of these concepts will be needed. The list of values for which data exist can be found in the `content constraints`, while the names of the values can be retrieved from the `codes` in the `codelists` referenced by the `data structure definition`. All these artefacts can be retrieved in just one dataflow query, again using the references' resolution mechanism offered by the SDMX RESTful API.

```
https://ws-entry-point/dataflow/agency-id/dataflow-id/dataflow-version?references=all
```

The response will contain the selected `dataflow`, the `data structure definition` that structure the data for the dataflow, as well as the `concepts` and `codelists` referenced by the data structure definition. In addition, it will contain the `content constraints` attached to the dataflow.

The screenshot below shows an example of the type of user interface (a list box for the list of concepts, and a collection of check boxes for the list of allowed values for each of the concepts in this example) that can be built from the response. The example is taken from the ECB statistical tablet app.

![Dimension filters](img/df-filters.png)

The client typically needs to display the name of the concepts (Frequency, Reference area, etc.) and of the allowed values for each concept (e.g.: Annual, Monthly, etc.). In addition, some clients might also want to display the descriptions of the concepts and codes, as well as the code ids.

In order to retrieve the data matching the user's filters, the web service clients will need the full references to the dataflow (id, agency id and version) and the code ids for the dimension values.

Once the filters' selection is finished, the client will typically provide a list of matching series like the one shown on the screen below, but this is typically the result of a data query:

![Matching series](img/series-list.png)
