# Introduction

Let's first start with a brief introduction of the SDMX information model.

In order to make sense of some statistical data, we need to know the concepts
associated with them. For example, on its own the figure 1.2953 is pretty meaningless,
but if we know that this is an exchange rate for the US dollar against the euro on
23 November 2006, it starts making more sense.

There are two types of concepts: dimensions and attributes. Dimensions, when combined,
allow to uniquely identifying statistical data. Attributes on the other hand do not help
identifying statistical data, but they add useful information (like the unit of measure
or the number of decimals). Dimensions and attributes are known as "components".

The measurement of some phenomenon (e.g. the figure 1.2953 mentioned above) is known as an
"observation" in SDMX. Observations are grouped together into a "data set". However, there
can also be an intermediate grouping. For example, all exchange rates for the US dollar
against the euro can be measured on a daily basis and these measures can then be
grouped together, in a so-called "time series". Similarly, you can group a collection of
observations made at the same point in time, in a "cross-section" (for example,
the values of the US dollar, the Japanese yen and the Swiss franc against the euro at a
particular date). Of course, these intermediate groupings are entirely optional and you
may simply decide to have a flat list of observations in your data set.

The SDMX information model is much richer than this limited introduction;
however the above should be sufficient to understand the sdmx-json format. For
additional information, please refer to the [SDMX documentation](http://sdmx.org/?page_id=10).

Samples, tools and other SDMX-JSON resources are available in the public Github
repository <https://github.com/sdmx-twg/sdmx-json>.

Before we start, let's clarify a few more things about this guide:

-  New fields may be introduced in later versions. Therefore
consuming applications should tolerate the addition of new fields with ease.
- The ordering of fields in objects is undefined. The fields may appear in any order
and consuming applications should not rely on any specific ordering. It is safe to consider a
nulled field and the absence of a field as the same thing.
- Not all fields appear in all contexts. For example response with error messages
may not contain fields for data, dimensions and attributes.

# Field Guide to SDMX-JSON Objects

## message

Message is the top level object and it contains the data as well 
as the metadata needed to interpret those data.

* header - *Object* *nullable*. *[Header](#header)* contains basic technical information about the message, such as when it was prepared and who has sent it.
* structure - *Object* *nullable*. *[Structure](#structure)* contains the information needed to interpret the data available in the message, such as the list of concepts used.
* dataSets - *Array* *nullable*. *DataSets* field is an array of *[dataSet](#dataset)* objects. That's where the data (i.e.: the observations) will be.
* errors - *Array* *nullable*. *Errors* field is an array of *[error](#error)* objects. When appropriate provides a list of error messages in addition to RESTful web services HTTP error status codes.

Example:

    {
      "header": {
          # header object #
      },
      "structure": {
          # structure object #
      },
      "dataSets": [
        {
          # dataSet object #
        }
      ],
      "errors": [
        {
          # error object #
        }
      ]
    }

## header

*Object* *nullable*. Header contains basic technical information 
about the message, such as when it was prepared and who has sent it.

* id - *String*. *String*. Unique string that identifies the message for further references.
* test - *Boolean* *nullable*. Indicates whether the message is for test purposes or not. False for normal messages.
* prepared - *String*. A timestamp indicating when the message was prepared. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* sender - *Object*. *[Sender](#sender)* contains information about the party that is transmitting the message.
* receiver - *Object* *nullable*. *[Receiver](#receiver)* contains information about the party that is receiving the message. This can be useful if the WS requires authentication.
* links - *Array* *nullable*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional external resources for the header.

Example:

    "header": {
      "id": "b1804c51-1ee3-45a9-bb75-795cd4e06489",
      "prepared": "2013-01-03T12:54:12",
      "test": false,
      "sender: {
        # sender object #
      },
      "receiver": {
        # receiver object #
      },
      "links": [
        {
          # link object #
        }
      ]
    }

### sender

*Object*. Information about the party that is transmitting the message. 
Sender contains the following fields:

* id - *String*. A unique identifier of the party.
* name - *String* *nullable*. A human-readable name of the sender.
* contacts - *Array* *nullable*. A collection of *[contacts](#contact)*.

Example:

    "sender": {
      "id": "ECB",
      "name": "European Central Bank",
      "contacts": [
        {
          # contact objects #
        }
      ]
    }

#### contact

*Object*. A collection of contact details. Each object in the collection 
may contain the following field:

* name - *String*. The contact's name.
* department - *String* *nullable*. The organisational structure for the contact.
* role - *String* *nullable*. The responsibility of the contact.
* telephone - *Array* *nullable*. An array of telephone numbers for the contact.
* fax - *Array* *nullable*. An array of fax numbers for the contact person.
* uri - *Array* *nullable*. An array of uris. Each uri holds an information URL for the contact.
* email - *Array* *nullable*. An array of email addresses for the contact person.

Example:

    {
      "name": "Statistics hotline",
      "department": "Statistics hotline",
      "role": "Statistics hotline",
      "telephone": [ "+00 0 00 00 00 00" ],
      "fax": [ "+00 0 00 00 00 01" ],
      "uri": [ "www.xyz.org" ],
      "email": [ "statistics@xyz.org" ]
    }

### receiver

*Object* *nullable*. Information about the party that is receiving the message. 
This can be useful if the WS requires authentication. Receiver contains the 
same fields as [sender](#sender).

### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

## structure

*Object* *nullable*. Provides the structural metadata necessary to interpret the data 
contained in the message. It tells you which are the components (`dimensions` and `attributes`) 
used in the message and also describes to which level in the hierarchy (`dataSet`, `series`, 
`observations`) these components are attached.

* links - *Array* *nullable*. *Links* field is an array of *[link](#link)* objects. A collection of links to structural metadata or to additional information regarding the structure. **Providing links allowing accessing the underlying SDMX Data Structure Definition, Dataflow and/or Provision Agreements is recommended.**
* dimensions - *Object*. Describes the *[dimensions](#dimensions-attributes)* used in the message as well as the levels in the hierarchy (`dataSet`, `series`, `observations`) to which these `dimensions` are attached.
* attributes - *Object*. Describes the *[attributes](#dimensions-attributes)* used in the message as well as the levels in the hierarchy (`dataSet`, `series`, `observations`) to which these `attributes` are attached.
* annotations - *Array* *nullable*. *Annotations* field is an array of *[annotation](#annotation)* objects. If appropriate, provides a list of `annotations` that can be referenced by `structure`, `component`, `component value`, `dataSets`, `series` and `observations`.

Example:

    "structure": {
        "links": [
          {
            # link object #
          }
        ],
        "dimensions": {
            # dimensions object #
        },
        "attributes": {
            # attributes object #
        },
        "annotations": [
          {
            # annotation object #
          }
        ]
    }

### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.
Providing links allowing accessing the underlying SDMX Data Structure Definition, Dataflow
and/or Provision Agreements is recommended.

### dimensions, attributes

*Object*. Describes the dimensions/attributes used in the message 
as well as the levels in the hierarchy (`dataSet`, `series`, `observations`) 
to which these dimensions/attributes are attached. 

* dataSet - *Array* *nullable*. *DataSet* field is an array of *[component](#component)* objects. Optional array to be provided if components (dimensions or attributes) are presented at the `dataSet` level. It is highly recommended to present all dimensions and attributes at the `dataSet` level for which the message contains only 1 single value.
* series - *Array* *nullable*. *Series* field is an array of *[component](#component)* objects. Optional array to be provided if components (dimensions or attributes) are presented at the `series` level.
* observation - *Array* *nullable*. *Observation* field is an array of *[component](#component)* objects. Optional array to be provided if components (dimensions or attributes) are presented at the `observation` level. When using the SDMX API, then the dimension(s) specified in the parameter "dimensionAtObservation" would be presented at `observation` level. If "dimensionAtObservation=AllDimensions" then all dimensions, except those with only one value possibly presented at the `dataSet` level, would be presented at `observation` level.

Example:

    "dimensions": {
        "dataSet": [
          {
            # component object #
          }
        ],
        "series": [
          {
            # component object #
          }
        ],
        "observation": [
          {
            # component object #
          }
        ]
    }
    "attributes": {
        "dataSet": [
          {
            # component object #
          }
        ],
        "series": [
          {
            # component object #
          }
        ],
        "observation": [
          {
            # component object #
          }
        ]
    }

#### component

*Object* *nullable*. A component represents a `dimension` or an `attribute` used in the message. 
It contains basic information about the component (such as its `name` and `id`) 
as well as the list of `values` used in the message for this particular component.
Each of the components may contain the following fields:

* id - *String*. Identifier for the component. 
* name - *String*. Name provides a human-readable name for the component.
* description - *String* *nullable*. Provides a description for the component.
* keyPosition - *Number* *nullable*. Indicates the position of the `dimension` in the key (e.g. used in the SDMX API call), starting at 0. This field should not be supplied for `attributes` and it may also be omitted for `dimensions`. This field could be used to build the "key" parameter string (i.e. D.USD.EUR.SP00.A) for data queries, whenever the order of the `dimensions` cannot easily be derived from the structural metadata information available in the data message. 
* role - *String* *nullable*. Defines the component role(s), if any. Roles are represented by the id of a concept defined as [SDMX cross-domain concept](https://sdmx.org/wp-content/uploads/01_sdmx_cog_annex_1_cdc_2009.pdf). Several of the concepts defined as SDMX cross-domain concepts are useful for data visualisation, such as for example, the series title ("TITLE"), the unit of measure ("UNIT_MEASURE"), the number of decimals to be displayed ("DECIMALS"), the  country or geographic reference area ("REF_AREA", e.g. when using maps), the period of time to which the measured observation refers ("REF_PERIOD"), the time interval at which observations occur over a given time period ("FREQ"), etc. It is strongly recommended to identify any component that can be useful for data visualisation purposes by using the appropriate SDMX cross-domain concept as role.
* default - *String* or *Number* *nullable*. Defines a default `value` for the component (valid for `attributes` only!). If no value is provided in the data part of the message then this value applies.
* links - *Array* *nullable*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional information regarding the component.
* annotations - *Array* *nullable*. *[Annotations](#annotation)* is a collection of indices of the corresponding *annotations* for the component. Indices refer back to the array of *annotations* in the structure field.
* values - *Array*. *Values* field is an array of *[component value](#component-value)* objects. Note that `dimensions` and `attributes` presented at `dataSet` level can only have one single component value.

Example:

    {
      "id": "FREQ",
      "name": "Frequency",
      "description": "The time interval at which observations occur over a given time period.",
      "keyPosition": 0,
      "role": "FREQ",
      "default": "A",
      "links": [
        {
          # link object #
        }
      ],
      "annotations": [ 2, 35 ],
      "values": [
        {
          # component value object #
        }
      ]
    }

##### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

##### component value

*Object* *nullable*. A particular value for a component in a message. 

* id - *String*. Unique identifier for a component value.
* name - *String*. Name provides a human-readable name for the component value.
* description - *String* *nullable*. Description provides a human-readable description of the value. The description is typically longer than the text provided for the name field.
* start, end - *String* *nullable*. Start and end are instances of time that define the actual Gregorian calendar period covered by the values for the time dimension. The algorithm for computing start and end fields for any supported reporting period is defined in the SDMX Technical Notes. These fields should be used only when the component value represents one of the values for the time dimension. Values are considered as inclusive both for the start field and the end field. Values must follow the ISO 8601 syntax for combined dates and times, including time zone. These fields are useful for visualisation tools, when selecting the appropriate point in time for the time axis. Statistical data, can be collected, for example, at the beginning, the middle or the end of the period, or can represent the average of observations through the period. Based on this information and using the start and end fields, it is easy to get or calculate the desired point in time to be used for the time axis.
* links - *Array* *nullable*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional information regarding the component value.
* annotations - *Array* *nullable*. *[Annotations](#annotation)* is a collection of indices of the corresponding *annotations* for the component value. Indices refer back to the array of *annotations* in the structure field.

Example:

    {
        "id": "2010",
        "name": "2010",
        "description": "Description for 2010.",
        "start": "2010-01-01T00:00Z",
        "end": "2010-12-31T23:59:59Z",
        "links": [
          {
            # link object #
          }
        ],
        "annotations": [ 5, 49 ]
    }

###### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

### annotation

*Object* *nullable*. An `annotation` object can be referenced through its `annotations` array index by `structure`, `component`, `component value`, `dataSets`, `series` and `observations`. It contains the following optional information:

* title - *String* *nullable*. Provides a title for the annotation.
* type - *String* *nullable*. Type is used to distinguish between annotations designed to support various uses. The types are not enumerated, and these can be freely specified by the creator of the annotations. The definitions and use of annotation types should be documented by their creator.
* text - *String* *nullable*. Contains the text of the annotation.
* id - *String* *nullable*. ID provides a non-standard identification of an annotation. It can be used to disambiguate annotations.
* links - *Array* *nullable*. *Links* field is an array of *[link](#link)* objects. If appropriate, a link to an additional external resource which may contain or supplement the annotation.

Example:

    {
      "title": "Sample annotation",
      "type": "reference",
      "text": "Sample annotation text",
      "id": "74747",
      "links": [
        {
          # link object #
        }
      ]
    }

#### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

## dataSet

*Object*. That's where the data (i.e.: the `observations`) will be.

In typical cases, the file will contain only one `dataSet`. 
However, in some cases, such as when retrieving, from an SDMX 2.1 web service, 
what has changed in the data source since in particular point in time, the web
service might return more than one `dataSet`.

There are between 2 and 3 levels in a `dataSet` object, depending on the way 
the data in the message is organized.

A `dataSet` may contain a flat list of `observations`. In this scenario, 
we have 2 levels in the data part of the message: 
the `dataSet` level and the `observation` level.

A `dataSet` may also organize observations in logical groups called `series`. 
These groups can represent time series or cross-sections. In this scenario, 
we have 3 levels in the data part of the message: 
the `dataSet` level, the `series` level and the `observation` level.

`Dimensions` and `attributes` may be specified at any of these 3 levels.

In case the `dataSet` is a flat list of `observations`, `observations` will be found 
directly under a `dataSet` object. In case the `dataSet` represents time series 
or cross sections, the `observations` will be found under the `series` elements.

The `dataSet` properties are:

* action - *String* *nullable*. Action provides a list of actions, describing the intention of the data transmission from the sender's side.
- `Append` - this is an incremental update for an existing `dataSet` or the provision of new data or documentation (attribute values) formerly absent. If any of the supplied data or metadata is already present, it will not replace these data.
- `Replace` - data are to be replaced, and may also include additional data to be appended.
- `Delete` - data are to be deleted.
- `Information` (default) - data are being exchanged for informational purposes only, and not meant to update a system.
* reportingBegin - *String* *nullable*. The start of the time period covered by the message.
* reportingEnd - *String* *nullable*. The end of the time period covered by the message.
* validFrom - *String* *nullable*. The validFrom indicates the inclusive start time indicating the validity of the information in the data.
* validTo - *String* *nullable*. The validTo indicates the inclusive end time indicating the validity of the information in the data.
* publicationYear - *String* *nullable*. The publicationYear holds the ISO 8601 four-digit year.
* publicationPeriod - *String* *nullable*. The publicationPeriod specifies the period of publication of the data in terms of whatever provisioning agreements might be in force (i.e., "2005-Q1" if that is the time of publication for a `dataSet` published on a quarterly basis).
* links - *Array* *nullable*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional information regarding the dataSet.
* annotations - *Array* *nullable*. *[Annotations](#annotation)* is a collection of indices of the corresponding *annotations* for the dataSet. Indices refer back to the array of *annotations* in the structure field.
* attributes - *Array* *nullable*. Collection of indices of the corresponding *values* of all attributes presented at the dataSet level. Each value is an index in the `values` array of the respective *component* object within the `structure.attributes.dataSet` array. This is typically the case for `attributes` that always have the same value for all the `observations` available in the `dataSet`. In order to avoid repetition, that value can simply be presented at the `dataSet` level.
* series - *Object* *nullable*. A collection of *[series](#series)* objects, to be used when the `observations` contained in the `dataSet` are presented in logical groups (time series or cross-sections), e.g. when using the SDMX API with the parameter "dimensionAtObservation=TIME_PERIOD" (default option) or with the "dimensionAtObservation" parameter with an ID of any other specific `dimension`. This element must **not** be used in case the `dataSet` presents a flat view of `observations`.
* observations - *Object* *nullable*. Collection of *[observations](#observations)* used in case when a `dataSet` is presented as a flat view of `observations`, e.g. when using the SDMX API with the parameter "dimensionAtObservation=AllDimensions". All `dimensions`, except those with only one `value` possibly presented at the `dataSet` level, would be presented at `observation` level. Alternatively, in case the `observations` are to be presented in logical groups (time series or cross-sections), use the *[series](#series)* element instead.

For information on how to handle the indices for `annotations`, `attributes` or 
`observations` see the section dedicated to [handling component values](#handling-component-values).

Examples:

    {
      "action": "Information",
      "reportingBegin": "2012-05-04",
      "reportingEnd": "2012-06-01",
      "validFrom": "2012-01-01T10:00:00Z",
      "validTo": "2013-01-01T10:00:00Z",
      "publicationYear": "2005",
      "publicationPeriod": "2005-Q1",
      "links": [
        # links array #
      ],
      "annotations": [ 3, 42 ],
      "attributes": [ 0, null, 0 ],
      "series": {
        # series object #
      }
    }

    {
      "action": "Information",
      "reportingBegin": "2012-05-04",
      "reportingEnd": "2012-06-01",
      "validFrom": "2012-01-01T10:00:00Z",
      "validTo": "2013-01-01T10:00:00Z",
      "publicationYear": "2005",
      "publicationPeriod": "2005-Q1",
      "links": [
        # links array #
      ],
      "annotations": [ 3, 42 ],
      "attributes": [ 0, null, 0 ],
      "observations": {
        # observations object #
      }
    }

### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

### series

*Object* *nullable*. Collection of series, when the `observations` contained in the `dataSet`
are used into logical groups (time series or cross-sections). Each underlying series 
is represented as a name/value pair in the `series` object.

A series is uniquely identified through the content of the *name* in the name/value pair, 
which is the indices of the corresponding `values` of all `dimensions` presented at `series` 
level (indices in the `values` array of the respective *component* object within the 
*structure.dimensions.series* array) separated by a colon (":"). 

The *value* in the name/value pair is an object containing:

* annotations - *Array* *nullable*. *[Annotations](#annotation)* is a collection of indices of the corresponding *annotations* for the series. Indices refer back to the array of `annotations` in the structure field.
* attributes - *Array* *nullable*. Collection of indices of the corresponding `values` of all `attributes` presented at the `series` level. Each value is an index in the `values` array of the respective `component` object within the `structure.attributes.series` array. This is typically the case for `attributes` that always have the same value for all the `observations` available in the series. In order to avoid repetition, that value can simply be presented at the `series` level. 
* observations - *Object* *nullable*. Collection of [observations](#observations) used in case when the `observations` are presented in logical groups (time series or cross-sections), e.g. when using the SDMX API with the parameter "dimensionAtObservation=TIME_PERIOD" (default option) or with the "dimensionAtObservation" parameter with an ID of any other specific `dimension`. Only (this) one `dimension` would be presented at `observation` level for each series.

Example:

    /*
    For this example, to ease understanding, let's consider a CSV format with 
    horizontal time series (with header row):
 
    DIM1,DIM2,Value for 2016,Value for 2017,ATTR1,ATTR2,ANNOT,ATTR3 for 2016,ATTR3 for 2017
    DIM1_VALUE_1,DIM2_VALUE_1,1.5931,1.5925,ATTR1_VALUE_2,ATTR2_VALUE_1,,ATTR3_VALUE_1,
    DIM1_VALUE_1,DIM2_VALUE_2,40.3426,40.3000,ATTR1_VALUE_1,,ANNOT_VALUE1,ATTR3_VALUE_1,ATTR3_VALUE_1
    
    In SDMX-JSON, using "dimensionAtObservation=TIME_PERIOD" (default) the observations 
    are presented in a similar way, grouped by time series (with the TIME_PERIOD dimension 
    at observation level), but dimension and attribute values are replaced by their indices:
    */
    
    "series": {
      "0:0": {
        "attributes": [1, 0],
        "observations": {
          "0": [1.5931, 0],
          "1": [1.5925]
        }
      },
      "0:1": {
        "annotations": [0],
        "observations": {
          "0": [40.3426, 0],
          "1": [40.3000, 0]
        }
      }
    }
    
    /*
    Series 1: "0:0" corresponds to the 2 indices for "DIM1":"DIM1_VALUE_1", "DIM2":"DIM2_VALUE_1"
              The attributes for this series are: "ATTR1":"ATTR1_VALUE_2", "ATTR2":"ATTR2_VALUE_1"
              This series has 2 observations:
              Observation 1: "0" corresponds to the index for "TIME_PERIOD":"2016"
                             The value for this observation is: 1.5931
                             The attribute for this observation is: "ATTR3":"ATTR3_VALUE_1"
              Observation 2: "1" corresponds to the index for "TIME_PERIOD":"2017"
                             The value for this observation is: 1.5925
                             The attribute for this observation is: "ATTR3":"ATTR3_VALUE_2" 
                             (because this is the default value)
    Series 2: "0:1" corresponds to the 2 indices for "DIM1":"DIM1_VALUE_1", "DIM2":"DIM2_VALUE_2"
              The annotation for this series is: "ANNOT":"ANNOT_VALUE1"
              The attributes for this series are: "ATTR1":"ATTR1_VALUE_1" (because this is the default value)
              This series has 2 observations:
              Observation 1: "0" corresponds to the index for "TIME_PERIOD":"2016"
                             The value for this observation is: 40.3426
                             The attribute for this observation is: "ATTR3":"ATTR3_VALUE_1"
              Observation 2: "1" corresponds to the index for "TIME_PERIOD":"2017"
                             The value for this observation is: 40.3000
                             The attribute for this observation is: "ATTR3":"ATTR3_VALUE_1"
    */
    
    "dimensions": {
      "dataSet": [],
      "series": [
        {
          "id": "DIM1",
          "name": "Dimension 1",
          "values": [
            {
              "id": "DIM1_VALUE_1",
              "name": "Dimension 1 - Value 1 with index 0"
            }
          ]
        },
        {
          "id": "DIM2",
          "name": "Dimension 2",
          "values": [
            {
              "id": "DIM2_VALUE_1",
              "name": "Dimension 2 - Value 1 with index 0"
            },
            {
              "id": "DIM2_VALUE_2",
              "name": "Dimension 2 - Value 2 with index 1"
            }
          ]
        }
      ],
      "observation": [
        {
          "id": "TIME_PERIOD",
          "name": "Time Period",
          "values": [
            {
              "id": "2016",
              "name": "2016"
            },
            {
              "id": "2017",
              "name": "2017"
            }
          ]
        }
      ]
    },
    "attributes": {
      "dataSet": [],
      "series": [
        {
          "id": "ATTR1",
          "name": "Attribute 1",
          "default": "ATTR1_VALUE_1",
          "values": [
            {
              "id": "ATTR1_VALUE_1",
              "name": "Attribute 1 - Value 1 with index 0"
            },
            {
              "id": "ATTR1_VALUE_2",
              "name": "Attribute 1 - Value 2 with index 1"
            }
          ]
        },
        {
          "id": "ATTR2",
          "name": "Attribute 2",
          "values": [
            {
              "id": "ATTR2_VALUE_1",
              "name": "Attribute 2 - Value 1 with index 0"
            }
          ]
        }
      ],
      "observation": [
        {
          "id": "ATTR3",
          "name": "Attribute 3",
          "default": "ATTR3_VALUE_2",
          "values": [
            {
              "id": "ATTR3_VALUE_1",
              "name": "Attribute 3 - Value 1 with index 0"
            },
            {
              "id": "ATTR3_VALUE_2",
              "name": "Attribute 3 - Value 2 with index 1"
            }
          ]
        }
      ]
    },
    "annotations": [
        {
          "title": "Annotation 1 - with index 0",
          "type": "example",
          "text": "Sample annotation text",
          "id": "ANNOT_VALUE1"
        }
    ]


For information on how to handle the indices for `annotations`, `attributes` or 
`observations` see the section dedicated to [handling component values](#handling-component-values).

### observations

*Object* *nullable*. Collection of observations. Each observation is represented as a 
name/value pair in the `observations` object.

An observation is uniquely identified through the content of the *name* in the name/value pair, 
which is the indices of the corresponding values of all dimensions presented at observation 
level (indices in the `values` array of the respective `component` object within the 
`structure.dimensions.observation` array) separated by a colon (":"). 
It's one single index for time series and cross-sections representations, but there will be 
more than one when the data are represented as a flat view of observations.

The *value* in the name/value pair is an array containing the observation value (first position),
the indices of the corresponding values of `attributes` presented at `observation` level
(any following position up to the number of `attributes` defined at  `observation` level), and
the indices of the corresponding values of `annotations` of that observation (any following 
position). Therefore, elements after the observation value are for the `observation` level 
`attributes` and for `annotations` of that observation. Elements for `annotations` are only 
included if there are `annotations` for that observation. **If `annotations` are present for an 
observation, then all `attributes` defined at `observation` level must be included.** Otherwise, 
if the observation has no `annotations`, then beginning from the end of the array, `observation` 
level `attributes` can be omitted if: 
- the `attribute` is not set for this observation (possible for optional attributes) or
- the `attribute` value for this observation corresponds to the default value.

The data type for observation value is *Number*. The data type for a reported missing 
observation value is a *null*. The index for an `attribute` is the corresponding index in the 
`values` array of the respective `component` object within the `structure.attributes.series` array.
It is *null*ed for unused optional `attributes` when the attribute index needs to be included. 
The index for an `annotation` is the index in the array of `annotations` in the structure field.

Example:

    /*
    For this example, to ease understanding, let's consider data in a 
    flat CSV format (with header row):
 
    DIM1,DIM2,Observation Value,ATTR1,ATTR2,ANNOT
    DIM1_VALUE_1,DIM2_VALUE_1,105.6,ATTR1_VALUE_1,,ANNOT_VALUE1
    DIM1_VALUE_1,DIM2_VALUE_2,105.9,ATTR1_VALUE_2,,
    
    In SDMX-JSON, the observations are presented in a similar flattened way, 
    but dimension and attribute values are replaced by their indices:
    */
    
    "observations": {
      "0:0": [105.6, 0, null, 0],
      "0:1": [105.9, 1]
    }
    
    /*
    Observation 1: "0:0" corresponds to the 2 indices for "DIM1":"DIM1_VALUE_1", "DIM2":"DIM2_VALUE_1"
                   The value for this observation is: 105.6
                   The attributes for this observation are: 
                     "ATTR1":"ATTR1_VALUE_1"
                     "ATTR2":"ATTR2_VALUE_1" (because this is the default value)
                   The annotation for this observation is: "ANNOT":"ANNOT_VALUE1"
    Observation 2: "0:1" corresponds to the 2 indices for "DIM1":"DIM1_VALUE_1", "DIM2":"DIM2_VALUE_2"
                   The value for this observation is: 105.9
                   The attributes for this observation are: 
                     "ATTR1":"ATTR1_VALUE_1"
                     "ATTR2":"ATTR2_VALUE_1" (because this is the default value)
    */
    
    "dimensions": {
      "dataSet": [],
      "series": [],
      "observation": [
        {
          "id": "DIM1",
          "name": "Dimension 1",
          "values": [
            {
              "id": "DIM1_VALUE_1",
              "name": "Dimension 1 - Value 1 with index 0"
            }
          ]
        },
        {
          "id": "DIM2",
          "name": "Dimension 2",
          "values": [
            {
              "id": "DIM2_VALUE_1",
              "name": "Dimension 2 - Value 1 with index 0"
            },
            {
              "id": "DIM2_VALUE_2",
              "name": "Dimension 2 - Value 2 with index 1"
            }
          ]
        }
      ]
    },
    "attributes": {
      "dataSet": [],
      "series": [],
      "observation": [
        {
          "id": "ATTR1",
          "name": "Attribute 1",
          "values": [
            {
              "id": "ATTR1_VALUE_1",
              "name": "Attribute 1 - Value 1 with index 0"
            },
            {
              "id": "ATTR1_VALUE_2",
              "name": "Attribute 1 - Value 2 with index 1"
            }
          ]
        },
        {
          "id": "ATTR2",
          "name": "Attribute 2",
          "default": "ATTR2_VALUE_1",
          "values": [
            {
              "id": "ATTR2_VALUE_1",
              "name": "Attribute 2 - Value 1 with index 0"
            }
          ]
        }
      ]
    }
    "annotations": [
        {
          "title": "Annotation 1 - with index 0",
          "type": "example",
          "text": "Sample annotation text",
          "id": "ANNOT_VALUE1"
        }
    ]

For information on how to handle the indices for `observations` 
see the section dedicated to [handling component values](#handling-component-values).

## error

*Object* *nullable*. Used to provide a error message in addition 
to RESTful web services HTTP error status codes.
The following pieces of information are to be provided:

* code - *number*. Provides a code number for the error message. Code numbers are defined in the SDMX 2.1 Web Services Guidelines.
* message - *string*. Provides the error message. Error messages are fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the error.

Example:

    {
      "code": 150,
      "message": "Invalid number of dimensions in the key parameter"
    }

# Linking mechanism

## link

*Object* *nullable*. A link to an external resource.

* href - *String*. Absolute or relative URL of the external resource.
* rel - *String*. Relationship of the object to the external resource.
* title - *String* *nullable*. A human-friendly description of the target link.
* type - *String* *nullable*. A hint about the type of representation returned by the link.
* hreflang - *String* *nullable*. The natural language of the external link, the same as used in the HTTP Accept-Language request header.

Examples:

    {
      "href": "https://registry.sdmx.org/ws/rest/dataflow/ECB/EXR",
      "rel": "dataflow"
    }
    
    {
      "href": "https://registry.sdmx.org/ws/rest/datastructure/ECB/ECB_EXR1",
      "rel": "datastructure"
    }
    
    {
       "href": "https://registry.sdmx.org/FusionRegistry/ws/rest/provisionagreement/ESTAT/PA_NAMAIN_IDC_N",
       "rel": "provisionagreement"
    }

    {
      "href": "https://registry.sdmx.org/help.html",
      "rel": "help",
      "title": "Documentation about the SDMX Global Registry",
      "type": "text/html",
      "hreflang": "en"
    }

Collections of links can be attached to various elements in SDMX-JSON.

Similarly with standards such as HTML5 and Atom, link elements in SDMX-JSON 
*must* define a *URL* (the `href` attribute) and a *semantic* (the `rel` attribute). 
This allows clients to follow the links they care about and ignore the ones whose 
semantic they are not interested in. In addition, links in SDMX-JSON *may* define 
a `title` (a human-friendly description of the target link) and a `type` (a hint 
about the type of representation returned by the link). Please refer to the 
[list of Media Types and Subtypes](http://www.iana.org/assignments/media-types/media-types.xhtml) assigned and listed by 
the IANA for additional information about expected values for the `type` attribute.

SDMX-JSON offers a list of predefined semantics, but implementers are free to extend it. 
The list of predefined semantics comes from the list of SDMX artefacts that can be 
returned by SDMX RESTful web services, semantics defined in [RFC5988](https://tools.ietf.org/rfc/rfc5988.txt) 
and additional items deemed to be useful in the context of statistical data dissemination. 
These semantics are:

  - SDMX artefacts: datastructure, metadatastructure, categoryscheme, conceptscheme, 
  codelist, hierarchicalcodelist, organisationscheme, agencyscheme, dataproviderscheme, 
  dataconsumerscheme, organisationunitscheme, dataflow, metadataflow, reportingtaxonomy, 
  provisionagreement, structureset, process, categorisation, contentconstraint, 
  attachmentconstraint, category, concept, code, organisation, agency, dataprovider, 
  dataconsumer, organisationunit, reportingcategory, data
  - RFC5988: alternate, copyright, glossary, help, index.
  - Miscellaneous: calendar (link to a release calendar), source (information about the 
  source of data), request (the SDMX RESTful query that triggered the SDMX-JSON response).

The *URL* captured in the `href` attribute can be *absolute* or *relative*. If you intend 
to archive the SDMX-JSON message, it is recommended to use absolute URLs.

# Handling component values

Let's say that the following message needs to be processed:

```json
{
    "header": {
        "id": "62b5f19d-f1c9-495d-8446-a3661ed24753",
        "prepared": "2012-11-29T08:40:26Z",
        "sender": {
            "id": "ECB",
            "name": "European Central Bank"
        }
    },
    "structure": {
        "id": "ECB_EXR_WEB",
        "links": [
          {
            "href": "https://sdw-wsrest.ecb.europa.eu/service/dataflow/ECB/EXR/1.0",
            "rel": "dataflow"
          }
        ],
        "dimensions": {
            "dataSet": [
                {
                    "id": "FREQ",
                    "name": "Frequency",
                    "keyPosition": 0,
                    "values": [
                        {
                            "id": "D",
                            "name": "Daily"
                        }
                    ]
                },
                {
                    "id": "CURRENCY_DENOM",
                    "name": "Currency denominator",
                    "keyPosition": 2,
                    "values": [
                        {
                            "id": "EUR",
                            "name": "Euro"
                        }
                    ]
                },
                {
                    "id": "EXR_TYPE",
                    "name": "Exchange rate type",
                    "keyPosition": 3,
                    "values": [
                        {
                            "id": "SP00",
                            "name": "Spot rate"
                        }
                    ]
                },
                {
                    "id": "EXR_SUFFIX",
                    "name": "Series variation - EXR context",
                    "keyPosition": 4,
                    "values": [
                        {
                            "id": "A",
                            "name": "Average or standardised measure"
                        }
                    ]
                }
            ],
            "series": [
                {
                    "id": "CURRENCY",
                    "name": "Currency",
                    "keyPosition": 1,
                    "values": [
                        {
                            "id": "NZD",
                            "name": "New Zealand dollar"
                        }, {
                            "id": "RUB",
                            "name": "Russian rouble"
                        }
                    ]
                }
            ],
            "observation": [
                {
                    "id": "TIME_PERIOD",
                    "name": "Time period or range",
                    "values": [
                        {
                            "id": "2013-01-18",
                            "name": "2013-01-18"
                        }, {
                            "id": "2013-01-21",
                            "name": "2013-01-21"
                        }
                    ]
                }
            ]
        },
        "attributes": {
            "dataSet": [],
            "series": [
                {
                    "id": "TITLE",
                    "name": "Series title",
                    "values": [
                        {
                            "name": "New zealand dollar (NZD)"
                        }, {
                            "name": "Russian rouble (RUB)"
                        }
                    ]
                }
            ],
            "observation": [
                {
                    "id": "OBS_STATUS",
                    "name": "Observation status",
                    "values": [
                        {
                            "id": "A",
                            "name": "Normal value"
                        }
                    ]
                }
            ]
        },
        "annotations": [
            {
              "title": "Sample series annotation title",
              "type": "example",
              "text": "Sample series annotation text",
              "id": "ABC123456"
            },
            {
              "title": "Sample observation annotation title",
              "type": "example",
              "text": "Sample observation annotation text",
              "id": "XYZ98765"
            }
        ]        
    },
    "dataSets": [
        {
            "action": "Information",
            "series": {
                "0": {
                    "annotations": [0],
                    "attributes": [0],
                    "observations": {
                        "0": [1.5931, 0],
                        "1": [1.5925, 0]
                    }
                },
                "1": {
                    "attributes": [1],
                    "observations": {
                        "0": [40.3426, 0],
                        "1": [40.3000, 0, 1]
                    }
                }
            }
        }
    ]
}
```

There is one `dataSet` in the message, and it contains two `series`.

```json
"0": {
    "annotations": [0],
    "attributes": [0],
    "observations": {
        "0": [1.5931, 0],
        "1": [1.5925, 0]
    }
},
"1": {
    "attributes": [1],
    "observations": {
        "0": [40.3426, 0],
        "1": [40.3000, 0, 1]
    }
}
```

The `structure.dimensions` field tells us that, out of the 6 dimensions, 4 have 
the same value for the 2 series and are therefore attached to the `dataSet` level.

We see that, for the first series, we get the value 0:

    "0": { ... }

From the structure.dimensions.series information, we know that CURRENCY is 
the (only) series dimension.

```json
"series": [
  {
    "id": "CURRENCY",
    "name": "Currency",
    "keyPosition": 1,
    "values": [
      {
        "id": "NZD",
        "name": "New Zealand dollar"
      }, {
        "id": "RUB",
        "name": "Russian rouble"
      }
    ]
  }
]
```

The value "0" identified previously is the index of the item in the 
collection of values for this component. In this case, the dimension 
value is therefore "New Zealand dollar".

Now, for the first observation of the first series, we get the value 0:

    "0": [...],

From the `structure.dimensions.observation` information, we know that 
TIME_PERIOD is the (only) dimension at `observation` level.

```json
"observation": [
  {
    "id": "TIME_PERIOD",
    "name": "Time period or range",
    "values": [
      {
        "id": "2013-01-18",
        "name": "2013-01-18"
      }, {
        "id": "2013-01-21",
        "name": "2013-01-21"
      }
    ]
  }
]
```

The value "0" identified previously is the index of the item in the 
collection of values for this component. In this case, the dimension value 
is therefore "2013-01-18".

Now, for the first (and only) attribute of the first observation of the 
first series, we get the value 0 (here the last value in array):

    "0": [1.5931, 0],

From the `structure.attributes.observation` information, we know that 
OBS_STATUS is the (only) attribute at `observation` level.

```json
"observation": [
  {
    "id": "OBS_STATUS",
    "name": "Observation status",
    "values": [
      {
        "id": "A",
        "name": "Normal value"
      }
    ]
  }
]
```

The value 0 identified previously is the index of the item in the 
collection of values for this component. In this case, the attribute value 
is therefore "Normal value".

The same logic applies for mapping the other observations, its attributes and annotations.

# Security Considerations

This document defines a response format for SDMX RESTful Web Services in JSON
and it raises no new security considerations. SDMX Web Services Guidelines
includes the security considerations associated with its usage.

# Extending SDMX-JSON

The objects defined in SDMX-JSON are "open", i.e. they can be extended by implementers 
with properties not defined in this specification. Providers of SDMX-JSON messages 
are therefore welcome to add support for features not covered in this specification. 
Whenever appropriate, providers who opt to do so are invited to inform us, 
so that future versions of SDMX-JSON may integrate these extensions, thereby 
improving interoperability.

The snippet below shows an example of an `error` object, extended with 
a `wsCustomErrorCode`:

```
"errors": [
  {
    "code": 150,
    "message": "Invalid number of dimensions in the key parameter",
    "wsCustomErrorCode": 39272
  }
]
```
