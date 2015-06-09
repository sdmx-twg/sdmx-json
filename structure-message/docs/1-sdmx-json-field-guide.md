# Introduction

See the SDMX-JSON data message docs for a brief introduction of the SDMX information model.

Before we start, let's clarify a few more things about this guide:

-  New fields may be introduced in later versions. Therefore
consuming applications should tolerate the addition of new fields with ease.
- The ordering of fields in objects is undefined. The fields may appear in any order
and consuming applications should not rely on any specific ordering. It is safe to consider a
nulled field and the absence of a field as the same thing.
- Not all fields appear in all contexts. For example response with error messages
may not contain fields for data, dimensions and attributes.

# Field Guide to SDMX-JSON Objects

## Message

Message is the top level object and it contains the data as well as the metadata needed to interpret those data.
Example:

    {
      "header": {
          # header fields #
      },
      "structure": {
          # structure objects #
      },
      "errors": [
          # Error messages #
      ]
    }

### header

*Object* *nullable*. *[Header](#Header)* contains basic technical information about
the message, such as when it was prepared and who has sent it. Example:

    "header": {
      "id": "b1804c51-1ee3-45a9-bb75-795cd4e06489",
      "prepared": "2012-05-04T03:30:00"
    }

### structure

*Object* *nullable*. *[Structure](#Structure)* contains the structural information of the concerned dataset,
such as the list of concepts used. Example:

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
        }
        "annotations": [
            # annotation objects #
        ]
    }

### errors

*Array* *nullable*. RESTful web services indicates errors using the HTTP status
codes. In addition, whenever appropriate, the error messages can also be returned using
this error field. Error is an array of error messages. Example:

    "errors": [
      {
        "code": 150,
        "message": "Invalid number of dimensions in the key parameter"
      }
    ]

#### code

*number*. Provides a code number for the error message. Code numbers are defined
in the SDMX 2.1 Web Services Guidelines. Example:

    "code": 150

#### message

*string*. Provides the error message. Error messages are fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the error. Example:

    "message": "Invalid number of dimensions in the key parameter"

## header

Header contains basic information about the message, such as when it was prepared and
who has sent it. Example:

    "header": {
      "id": "b1804c51-1ee3-45a9-bb75-795cd4e06489",
      "prepared": "2013-01-03T12:54:12",
      "sender: {
        "id": "SDMX"
      }
    }

### id

*String*. Unique string that identifies the message for further references.
Example:

    "id": "TEC00034"

### test

*Boolean* *nullable*. Indicates whether the message is for test purposes or not. False for normal messages. Example:

    "test": false

### prepared

*String*. A timestamp indicating when the message was prepared. Values must
follow the ISO 8601 syntax for combined dates and times, including time zone. Example:

    "prepared": "2012-05-04T03:30:00Z"

### sender

*Object*. Information about the party that is transmitting the message. Sender contains the following fields:

* id - *String*. A unique identifier of the party.
* name - *String* *nullable*. A human-readable name of the sender.
* contact - *Array* *nullable*. A collection of contact details.

Example:

    "sender": {
      "id": "ECB",
      "name": "European Central Bank",
      "contact": [
        # contact details #
      ]
    }

#### contact

*Array* *nullable*. Information on how the party can be contacted.
Each object in the collection may contain the following field:

* name - *String*. The contact's name.
* department - *String* *nullable*. The organisational structure for the contact.
* role - *String* *nullable*. The responsibility of the contact.
* telephone - *Array* *nullable*. An array of telephone numbers for the contact.
* fax - *Array* *nullable*. An array of fax numbers for the contact person.
* uri - *Array* *nullable*. An array of uris. Each uri holds an information URL for the contact.
* email - *Array* *nullable*. An array of email addresses for the contact person.

Example:

    "contact": [
        {
            "name": "Statistics hotline",
            "email": [ "statistics@xyz.org" ]
        }
    ]

### receiver

*Object* *nullable*. Information about the party that is receiving the message. This can be
useful if the WS requires authentication. Receiver contains the same fields as sender (see above):

Example:

    "receiver": {
      "id": "SDMX"
    }

### links

*Array* *nullable*. A collection of links to additional resources for the headers.

Example:

    "links": [
      {
        "href": "https://data.sdmx.org/ws/rest/data/EXR/D.USD.EUR.SP00.A",
        "rel": "request"
      }
    ]

#### link

*Object* *nullable*. A link to an external resource. Example:

    {
      "href": "https://registry.sdmx.org/help.html",
      "rel": "help",
      "title": "Documentation about the SDMX Global Registry",
      "type": "text/html"
    }

The `href` and `rel` attributes are mandatory, while the `title` and `type` are optional.

See the section about the [linking mechanism](#linking-mechanism) for additional information.

## structure

*Object* *nullable*. Provides the structural metadata necessary to interpret the data contained in the message.
It tells you which are the components (dimensions and attributes) used in the message and also describes to which
level in the hierarchy (data set, series, observations) these components are attached originally.

Example:

    "structure": {
        "links": [
          # links array #
        ],
        "dimensions": {
            # dimensions object #
        },
        "attributes": {
            # attributes object #
        },
        "annotations": {
            # annotations object #
        }
    }

### links

*Array* *nullable*. A collection of links to structural metadata or to additional information regarding the structure.

Example:

    "links": [
      {
        "href": "https://registry.sdmx.org/ws/rest/dataflow/ECB/EXR",
        "rel": "dataflow"
      },
      {
        "href": "https://registry.sdmx.org/ws/rest/datastructure/ECB/ECB_EXR1",
        "rel": "datastructure"
      }
    ]

#### link

*Object* *nullable*. A link to an external resource. Example:

    {
      "href": "https://registry.sdmx.org/help.html",
      "rel": "help",
      "title": "Documentation about the SDMX Global Registry",
      "type": "text/html"
    }

The `href` and `rel` attributes are mandatory, while the `title` and `type` are optional.

See the section about the [linking mechanism](#linking-mechanism) for additional information.

### name

*String* *nullable*. Data flow name. Example:

    "name": "Sample dataflow"

### description

*String* *nullable*. Descriptio of the data flow. Example:

    "description": "Data flow description."

### dimensions

*Object*. Describes the dimensions used in the message as well as the levels in the hierarchy (data set, series,
observations) to which these dimensions are attached originally. Example:

    "dimensions": {
        "dataSet": [
            # Component objects #
        ],
        "series": [
            # Component objects #
        ],
        "observation": [
            # Component object #
        ]
    }

### attributes

*Object*. Describes the attributes used in the message as well as the levels in the hierarchy (data set, series,
observations) to which these attributes are attached originally. Example:

    "attributes": {
        "dataSet": [
            # Component objects #
        ],
        "series": [
            # Component objects #
        ],
        "observation": [
            # Component objects #
        ]
    }

### dataSet

*Array* *nullable*. Optional array to be provided if components (dimensions or attributes) are attached to the data set level.

### series

*Array* *nullable*. Optional array to be provided if components (dimensions or attributes) are attached to the series level.

### observation

*Array* *nullable*. Optional array to be provided if components (dimensions or attributes) are attached to the observation level.

#### component

A component represents a dimension or an attribute used in the message. It contains basic information about the component
(such as its name and id) as well as the list of values used in the message for this particular component. Example:

    {
      "id": "FREQ",
      "name": "Frequency",
      "keyPosition": 0,
      "role": "FREQ",
      "values": [
        {
          # value object #
        }
      ]
    }

Each of the components may contain the following fields

##### id

*String*. Identifier for the component.
Example:

    "id": "FREQ"

##### name

*String*. Name provides a human-readable name for the component.
Example:

    "name": "Frequency"

##### description

*String* *nullable*. Provides a description for the component. Example:

    "description": "The time interval at which observations occur over a given time period."

##### keyPosition

*Number* *nullable*. Indicates the position of the dimension in the key, starting at 0.
This field should not be supplied for attributes and it may also be omitted for dimensions. This field could be used to build the "key" parameter string (i.e. D.USD.EUR.SP00.A) for data queries, whenever the order of the dimensions cannot easily be derived from the structural metadata information available in the data message. Example:

    "keyPosition": 0

##### role

*String* *nullable*. Defines the component role(s), if any. Roles are represented by the id of a concept defined as [SDMX cross-domain concept](http://sdmx.org/?page_id=11). Several of the concepts defined as SDMX cross-domain concepts are useful for data visualisation, such as for example, the series title ("TITLE"), the unit of measure ("UNIT_MEASURE"), the number of decimals to be displayed ("DECIMALS"), the  country or geographic reference area ("REF_AREA", e.g. when using maps), the period of time to which the measured observation refers ("REF_PERIOD"), the time interval at which observations occur over a given time period ("FREQ"), etc. It is strongly recommended to identify any component that can be useful for data visualisation purposes by using the appropriate SDMX cross-domain concept as role. Example:

    "role": "TITLE"

##### default

*String* or *Number* *nullable*. Defines a default value for the component (valid for attributes only!). If
no value is provided in the data part of the message then this value applies. Example:

    "default": "A"

##### values

*Array*. Array of [values](#component_values) for the component. Example:

    "values": [
      {
        "id": "M",
        "name": "Monthly"
      }
    ]

##### Component value

*Object* *nullable*. A particular value for a component in a message. Example:

    {
        "id": "M",
        "name": "Monthly"
    }

###### id

*String*. Unique identifier for a value. Example:

    "id": "A"

###### name

*String*. Human-readable name for a value. Example:

    "name": "Missing value; data cannot exist"

###### description

*String* *nullable*. Description provides a human-readable description of the value. The description is typically longer
than the text provided for the name field. Example:

    "description": "Description for missing value."

###### start and end fields

*String* *nullable*. Start and end are instances of time that define the actual
Gregorian calendar period covered by the values for the time dimension. The algorithm
for computing start and end fields for any supported reporting period is defined
in the SDMX Technical Notes.

These fields should be used only when the component value represents one of the
values for the time dimension.

Values are considered as inclusive both for the start field and the end field.
Values must follow the ISO 8601 syntax for combined dates and times, including time zone.

Example:

    {
        "id": "2010",
        "name": "2010",
        "start": "2010-01-01T00:00Z",
        "end": "2010-12-31T23:59:59Z"
    }

These fields are useful for visualisation tools, when selecting the appropriate point in time for the time axis.
Statistical data, can be collected, for example, at the beginning, the middle or the end of the period, or can
represent the average of observations through the period. Based on this information and using the start and end
fields, it is easy to get or calculate the desired point in time to be used for the time axis.

### Annotations

*Array* *nullable*. Provides a list of annotation objects. Annotations can be attached
to data sets, series and observations.

    "annotations": [
      {
        "title": "Sample annotation",
        "uri": "http://sample.org/annotations/74747"
      }
    ]

Each annotation object contains the following optional information:

#### title

*string* *nullable*. Provides a title for the annotation. Example:

    "title": "Sample annotation"

#### type

*string* *nullable*. Type is used to distinguish between annotations designed to
support various uses. The types are not enumerated, and these can be freely specified by
the creator of the annotations. The definitions and use of annotation types
should be documented by their creator. Example:

    "type": "reference"

#### uri

*string* *nullable*. URI - typically a URL - which points to an external resource
which may contain or supplement the annotation. If a specific behavior is desired,
an annotation type should be defined which specifies the use of this field more exactly.

    "uri": "http://sample.org/annotations/74747"

#### text

*string* *nullable*. Contains the text of the annotation.

    "text": "Sample annotation text"

#### id

*string* *nullable*. ID provides a non-standard identification of an annotation.
It can be used to disambiguate annotations. Example:

    "id": "74747"

# Linking mechanism

Collections of links can be attached to various elements in SDMX-JSON.

Similarily with standards such as HTML5 and Atom, link elements in SDMX-JSON *must* define a *URL* (the `href` attribute) and a *semantic* (the `rel` attribute). This allows clients to follow the links they care about and ignore the ones whose semantic they are not interested in. In addition, links in SDMX-JSON *may* define a `title` (a human-friendly description of the target link) and a `type` (a hint about the type of representation returned by the link).

SDMX-JSON offers a list of predefined semantics, but implementers are free to extend it. The list of predefined semantics comes from the list of SDMX artefacts that can be returned by SDMX RESTful web services, semantics defined in [RFC5988](https://tools.ietf.org/rfc/rfc5988.txt) and additional items deemed to be useful in the context of statistical data dissemination. These semantics are:

  - SDMX artefacts: datastructure, metadatastructure, categoryscheme, conceptscheme, codelist, hierarchicalcodelist, organisationscheme, agencyscheme, dataproviderscheme, dataconsumerscheme, organisationunitscheme, dataflow, metadataflow, reportingtaxonomy, provisionagreement, structureset, process, categorisation, contentconstraint, attachmentconstraint, category, concept, code, organisation, agency, dataprovider, dataconsumer, organisationunit, reportingcategory, data
  - RFC5988: alternate, copyright, glossary, help, index.
  - Miscellaneous: calendar (link to a release calendar), source (information about the source of data), request (the SDMX RESTful query that triggered the SDMX-JSON response).

The *URL* captured in the `href` attribute can be *absolute* or *relative*. If you intend to archive the SDMX-JSON message, it is recommended to use absolute URLs.

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
                            "name": "2013-01-18",
                            "start": "2013-01-18T00:00:00Z",
                            "end": "2013-01-18T23:59:59Z"
                        }, {
                            "id": "2013-01-21",
                            "name": "2013-01-21",
                            "start": "2013-01-21T00:00:00Z",
                            "end": "2013-01-21T23:59:59Z"
                        }
                    ]
                }
            ]},
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
        }
    },
}
```

# Security Considerations

This document defines a response format for SDMX RESTful Web Services in JSON
and it raises no new security considerations. SDMX Web Services Guidelines
includes the security considerations associated with its usage.


# Extending SDMX-JSON

The objects defined in SDMX-JSON are "open", i.e. they can be extended with properties not defined in this specification. Providers of SDMX-JSON messages are therefore welcome to add support for features not covered in this specification. Whenever appropriate, providers who opt to do so are invited to inform us, so that future versions of SDMX-JSON may integrate these extensions, thereby improving interoperability.

The snippet below shows an example of an `error` object, extended with a `wsCustomErrorCode`:

```
"errors": [
  {
    "code": 150,
    "message": "Invalid number of dimensions in the key parameter",
    "wsCustomErrorCode": 39272
  }
]
```
