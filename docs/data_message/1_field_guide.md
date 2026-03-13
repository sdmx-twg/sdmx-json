# Field Guide to SDMX-JSON Data Message 2.1.0 Objects (aligned with SDMX 3.1)

## message

Message is the top level object and it contains the data as well as the
structural metadata needed to interpret those data.

- $schema - *String* *optional*. Contains the URL to the schema allowing to
    validate the message. This also allows identifying the version of SDMX-JSON
    format used in this message.
    **Providing the link to the SDMX-JSON schema is recommended.**
- meta - *Object*. A *[meta](#meta)* object that contains non-standard
    meta-information and basic technical information about the message, such as
    when it was prepared and who has sent it.
- data - *Object* *optional*. *[Data](#data)* contains the message's "primary
    data".
- errors - *Array* *optional* of *[statusInformation](#statusinformation)*
  objects providing - when appropriate - more detail to the HTTP status codes
  in the RESTful SDMX web service responses.

The properties data and errors CAN coexist in the same message.

??? example

    ``` json
    {
      "$schema": "https://https://json.sdmx.org/2.1/sdmx-json-data-schema.json",
      "meta": {
          # meta object #
      },
      "data": {
          # data object #
      },
      "errors": [
          {
            # statusInformation object #
          }
      ]
    }
    ```

## meta

*Object* Used to include non-standard meta-information and basic
technical information about the message, such as when it was prepared and who
has sent it. Any members MAY be specified within `meta` objects.

- schema - *String* *optional*. Deprecated and replaced by the `$schema`
    property at the root level, which allows for automated validations.
- id - *String*. Unique string that identifies the message for further
    references.
- test - *Boolean* *optional*. Indicates whether the message is for test
    purposes or not. False for normal messages.
- prepared - *String*. A date or date-time indicating when the message was prepared.
    Values must follow the ISO 8601 syntax for dates or combined dates and times,
    including time zone.
- contentLanguages - *Array* *optional*. Array of strings containing the
    identifier of all languages used anywhere in the message for localized
    elements, and thus the languages of the intended audience, representing in
    an array format the same information than the http Content-Language response
    header, e.g. "en, fr-fr". See [IETF Language
    Tags](https://tools.ietf.org/html/rfc5646#section-2.1). The array's first
    element indicates the main language used in the message for localized
    elements. **The usage of this property is recommended.**
- name - *String* *optional*. Human-readable (best-language-match) name for
    the transmission.
- names - *Object* *optional*. Human-readable localised *[names](#names)* for
    the transmission.
- sender - *Object*. *[Sender](#sender)* contains information about the party
    that is transmitting the message.
- receivers - *Array* *optional* of *[Receiver](#receiver)* objects thats
    contain information about the party that is receiving the message. This can
    be useful if the WS requires authentication.
- links - *Array* *optional*. *Links* field is an array of *[meta link](#meta-link)*
    objects. If appropriate, a collection of links to additional external
    resources for the header.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ``` json
    "meta": {
      "copyright": "Copyright 2017 Statistics hotline",
      "id": "b1804c51-1ee3-45a9-bb75-795cd4e06489",
      "prepared": "2018-01-03T12:54:12",
      "test": false,
      "contentLanguages": [ "en", "fr-fr" ],
      "name": "Transmission name",
      "names": {
        # name object #
      },
      "sender": {
        # sender object #
      },
      "receivers": [
        {
          # receiver object #
        }
      ],
      "links": [
        {
          # link object #
        }
      ]
    }
    ```

### sender

*Object*. Information about the party that is transmitting the message. Sender
contains the following fields:

- id - *String*. A unique identifier of the party.
- name - *String* *nullable*. A human-readable (best-language-match) name of
    the sender.
- names - *Object* *optional*. A list of human-readable localised
    *[names](#names)* of the sender.
- contacts - *Array* *optional*. A collection of *[contacts](#contact)*.
    Provides contact information for the party in regard to the transmission of
    the message.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ``` json
    "sender": {
      "id": "ECB",
      "name": "European Central Bank",
      "names": { "en": "European Central Bank",
            "fr": "Banque Centrale Européenne" },
      "contacts": [
        {
          # contact objects #
        }
      ]
    }
    ```

#### names

*Object* containing all appropriate localised names, one per object property:

- One or more of: IETF Language Tag according to [RFC 5646
    documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for
    specifying locals in HTTP - *String*. The localised name.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ``` json
    {
      "en": "This is an English name",
      "en-GB": "This is a British name",
      "fr": "C'est un nom français"
    }
    ```

#### contact

*Object*. A collection of contact details. Each object in the collection may
contain the following field:

- id - *String*. Identifier for the resource.
- name - *String* *optional*. Human-readable (best-language-match) name of the
    contact.
- names - *Object* *optional*. Human-readable localised *[names](#names)* of
    the contact.
- department - *String* *optional*. Human-readable (best-language-match) name
    of the organisational structure for the contact.
- departments - *Object* *optional*. Human-readable localised
    *[names](#names)* of the organisational structure for the contact.
- role - *String* *optional*. Human-readable (best-language-match) name of the
    responsibility of the contact.
- roles - *Object* *optional*. Human-readable localised *[names](#names)* of
    the responsibility of the contact.
- telephones - *Array* *optional*. An array of telephone numbers for the
    contact.
- faxes - *Array* *optional*. An array of fax numbers for the contact person.
- uris - *Array* *optional*. An array of uris. Each uri holds an information
    URL for the contact.
- emails - *Array* *optional*. An array of email addresses for the contact
    person.
- x400s - *Array* *optional*. An array of X.400 addresses for the contact
    person.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ``` json
    {
      "id": "HOTLINE",
      "name": "Statistics hotline",
      "names": { "en": "Statistics hotline",
            "fr": "Service d'assistance téléphonique des Statistiques" },
      "department": "Statistics hotline",
      "departments": { "en": "Statistics hotline",
              "fr": "Service d'assistance téléphonique des Statistiques" },
      "role": "Statistics hotline",
      "roles": { "en": "Statistics hotline",
            "fr": "Service d'assistance téléphonique des Statistiques"},
      "telephones": [ "+00 0 00 00 00 00" ],
      "faxes": [ "+00 0 00 00 00 01" ],
      "uris": [ "www.xyz.org" ],
      "emails": [ "statistics@xyz.org" ]
    }
    ```

### receiver

*Object* *optional*. Information about the party that is receiving the message.
This can be useful if the WS requires authentication. Receiver contains the same
fields as [sender](#sender).

### meta link

See the section on [linking mechanism](./2_linking_mechanism.md) for all
information on links.

## data

*Object* *optional*. Header contains the message's "primary data".

- structures - *Array* *optional*. *Structures* field is an array of
    *[Structure](#structure)* objects that contain the information needed to
    interpret the data available in the message, such as the list of concepts
    used. Since SDMX 3.0.0, SDMX restful web service requests can query data for
    several structures. Each returned structure is presented separately. Also
    the underlying data are presented in separate data sets.
- dataSets - *Array* *optional*. *DataSets* field is an array of
    *[dataSet](#dataset)* objects. That's where the data (i.e.: the
    observations) will be. The dataset order matters because a data message with
    its datasets represents one ACID transaction when used to update an SDMX
    storage system. Indeed, the datasets are to be processed in the order of
    their presence in the message.

??? example

    ``` json
    "data": {
      "structures": [
        {
          # structure object #
        }
      ],
      "dataSets": [
        {
          # dataSet object #
        }
      ]
    }
    ```

## structure

*Object* *optional*. Provides the structural metadata necessary to interpret the
data contained in the message. It tells you which are the components
(`dimensions`, `measures` and `attributes`) used in the message and also
describes to which level in the hierarchy (`dataSet`, `dimensionGroup`,
`series`, `observations`) these components are attached.

- links - *Array*. *Links* field is an array of *[link](#link)* objects. A
  collection of links to structural metadata or to additional information
  regarding the structure. **If data was requested for a dataflow, then the**
  **dataflow identification is to be provided. If data was requested for a DSD,**
  **then the DSD identification is to be provided. If data was requested for a**
  **data provision agreement, then the data provision agreement identification is**
  **to be provided. This identification is to be listed as the first link and must**
  **include the URN.** Therefore, other identification properties are not needed.
- name - *String*. Human-readable (best-language-match) name of the related structure.
- names - *Object* *optional*. Human-readable localised *[names](#names)* of the
  related structure.
- description - *String* *optional*. Human-readable (best-language-match)
  description of the related structure. The description is typically longer than
  the text provided for the name field.
- descriptions - *Object* *optional*. Human-readable localised descriptions
  (see *[names](#names)*) of the related structure. A descriptions is typically
  longer than the text provided for the name field.
- dimensions - *Object*. Describes the
    *[dimensions](#dimensions-measures-attributes)* used in the message as well
    as the levels (`dataSet`, `series`, `observations`) at which these
    `dimensions` are presented.
- measures - *Object* *optional*. Describes the
    *[measures](#dimensions-measures-attributes)* used in the message.
    *Measures* are always presented at the `observations` level. For
    backward-compatibility, the `measures` object can be omitted if there is
    only one measure with the ID "OBS_VALUE". In this case, the measure values
    (of an indeterministic type) are written directly into the dataSet. SDMX
    3+.0.0 implementations should always use the `measures` object. In case an
    SDMX 3+.0.0 data structure definition has no measures, the `measures` object
    must be present but empty.
- attributes - *Object* *optional*. Describes the
    *[attributes](#dimensions-measures-attributes)* used in the message as well
    as the levels (`dataSet`, `dimensionGroup`, `series`, `observations`) at
    which these *attributes* are presented. Reference metadata attributes are
    also included here as needed.
- annotations - *Array* *optional*. *Annotations* field is an array of
    *[annotation](#annotation)* objects. If appropriate, provides a list of
    `annotations` that can be referenced by `structure`, `component`,
    `component value`, `dataSets`, `series` and `observations`.
- dataSets - *Array* *optional*. *dataSets* field is an array of integers. It
    contains the indexes of the *dataSet* objects in the *dataSets* array of the
    *data* object, which contain the data for this *structure*. If omitted, then
    all data included in the message are assumed to be described by the
    *structure*.

??? example

    ``` json
    {
      "links": [
        {
          # link object #
        }
      ],
      "dimensions": {
        # dimensions object #
      },
      "measures": {
        # measures object #
      },
      "attributes": {
        # attributes object #
      },
      "annotations": [
        {
          # annotation object #
        }
      ],
      "dataSets": [0]
    }
    ```

### structure link

See the section on [linking mechanism](./2_linking_mechanism.md) for all
information on links.

### dimensions, measures, attributes

*Object*. Describes the dimensions/measures/attributes used in the message as
well as the levels (`dataSet`, `dimensionGroup`, `series`, `observations`) at
which these dimensions/measures/attributes are presented.

- dataSet - *Array* *optional*. *DataSet* field is an array of
    *[component](#component)* objects. Optional array to be provided if
    components (only dimensions or attributes) are presented at the `dataSet`
    level. It is recommended, when technically feasible, to present all
    dimensions and attributes at the `dataSet` level for which the message
    contains only 1 single value, e.g. dimensions with single selections in the
    query filter and attributes that do not very with any of the dimensions.
- dimensionGroup - *Array* *optional*. *DimensionGroup* field is an array of
    *[component](#component)* objects. Optional array to be provided if
    components (only attributes) are presented at the `dimensionGroup` level.
- series - *Array* *optional*. *Series* field is an array of
    *[component](#component)* objects. Optional array to be provided if
    components (only dimensions or attributes) are presented at the `series`
    level.
- observation - *Array* *optional*. *Observation* field is an array of
    *[component](#component)* objects. Optional array to be provided if
    components (dimensions, measures or attributes) are presented at the
    `observation` level. When using the SDMX API, then the dimension(s)
    specified in the parameter "dimensionAtObservation" would be presented at
    `observation` level. If "dimensionAtObservation=AllDimensions" then all
    dimensions, except those with only one value possibly presented at the
    `dataSet` level, would be presented at `observation` level. Measures must
    always be presented at observation level.

??? example

    ``` json
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
    },
    "measures": {
      "observation": [
        {
          # component object #
        }
      ]
    },
    "attributes": {
      "dataSet": [
        {
          # component object #
        }
      ],
      "dimensionGroup": [
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
    ```

#### component

*Object* *optional*. A component represents a `dimension`, a `measure` or an
`attribute` used in the message. It contains basic information about the
component (such as its `name` and `id`) as well as the list of `values` used in
the message for this particular component. Dimensions must always present their
values in the `values` array because the dataSets will only be able to use the
corresponding indexes of these values in the `values` array. Measures should
present their values in the `values` array when they are coded. If they are
non-coded then the measure values are directly written into the dataSets.
Attributes should present their values in the `values` array at least when they
are coded, or if they are presented at dataset, group or series level (in order
to avoid repetition). If they are non-coded and presented at observation level
then instead of using the component's `values` array, the attribute values can
be directly written into the dataSets.\
Each of the components may contain the following fields:

- id - *String*. Identifier for the component.
- name - *String* *optional*. Human-readable (best-language-match) name for
    the component.
- names - *Object* *optional*. Human-readable localised *[names](#names)* for
    the component.
- description - *String* *optional*. Human-readable (best-language-match)
    description for the component.
- descriptions - *Object* *optional*. Human-readable localised descriptions
    (see *[names](#names)*) for the component.
- keyPosition - *Number*. **This field is mandatory for `dimensions` but not
    to** **be supplied for `measures` or `attributes`.** Indicates the position
    of the `dimension` in the Data Structure Definition, starting at 0. It needs
    to be provided also for the special `dimensions` such as Time or Measure
    dimensions. The information in this field is consistent with the order of
    `dimensions` in the "key" parameter string (i.e. D.USD.EUR.SP00.A) for data
    queries in the SDMX API.
- roles - *Array* of *String*s *optional*. Defines the component role(s), if
    any. Roles are represented by the id of a concept defined as [SDMX
    cross-domain
    concept](https://sdmx.org/wp-content/uploads/01_sdmx_cog_annex_1_cdc_2009.pdf).
    Several of the concepts defined as SDMX cross-domain concepts are useful for
    data visualisation, such as for example, the series title ("TITLE"), the
    unit of measure ("UNIT_MEASURE"), the number of decimals to be displayed
    ("DECIMALS"), the country or geographic reference area ("REF_AREA",
    e.g. when using maps), the period of time to which the measured observation
    refers ("REF_PERIOD"), the time interval at which observations occur over a
    given time period ("FREQ"), etc. It is strongly recommended to identify any
    component that can be useful for data visualisation purposes by using the
    appropriate SDMX cross-domain concept as role.
- isMandatory - *Boolean* *optional*. **Only for `measures` and
    `attributes`.** If `true` then that `measure` or `attribute` is mandatory in
    the exchange of complete datasets. The default is `false`.
- relationship - *Object* *optional*. **This field is mandatory for data**
    **`attributes` but optional for reference metadata `attributes` and not**
    **to be supplied for `measures` or `dimensions`.**
    *[Attribute relationship](#attribute-relationship)* describes how the value
    of this attribute varies with the values of other components. This
    relationship expresses the attachment level of the attribute as defined in
    the data structure definition. Depending on the message context (especially
    the data query) an attribute value can however be attached physically in
    the message at a lower level.
- format - *Object* *optional*. *[Format](#format)* describes the allowed
    components representation (permitted type and cardinality of the values). It
    contains essential information to determine the location of the component
    values and the related necessity of using indexes for referencing. `format`
    is only used when the component values are not defined by an enumerated list
    of identifiable items (codelist), in which case instead of `id` and `name`,
    the component values will use:  
      - if `maxOccurs` is greater than 1: the `values` property,
      - otherwise: the `value` property.
- default - *String* or *Number* *optional*. Defines a default `value` for the
    component (**valid for `attributes` only!**). If no value is provided in the
    data part of the message then this value applies.
- minOccurs - *Non-negative integer* *optional* **Only for `measures` and
    `attributes`.** Indicates the minimum number of value that must be reported
    for the component. If missing than there is no lower limit on its occurrences.
    The default is 1.
- maxOccurs - *Positive integer*/*String* *optional* **Only for `measures` and
    `attributes`.** Indicates the maximum number of values that can be reported
    for the component. If set to the string "unbounded" than there is no upper
    limit on its occurrences. For non-enumerated components, if > 1 then the
    component values will use the `values` property of the component value instead
    of `value`. The default is 1. `maxOccurs` must not be smaller than
    `minOccurs`.
- links - *Array* *optional*. *Links* field is an array of *[component link](#component-link)*
    objects. If appropriate, a collection of links to additional information
    regarding the component.
- annotations - *Array* *optional*. *[Annotations](#annotation)* is a
    collection of indices of the corresponding *annotations* for the component.
    Indices refer back to the array of *annotations* in the structure field.
- values - *Array* *optional*. *Values* field is an array of
  *[component value](#component-value)* objects. Note that `dimensions` and
  `attributes` presented at `dataSet` level can only have one single component
  value. ***Values* are *optional* for `measures` and `attributes`, but
  generally *required* for `dimensions` unless used only for attributes at
  `dimensionGroup` level**. Whenever the *values* field is provided, the
  component values in the dataSets will always contain the corresponding array
  indexes, otherwise they will contain the values themselves.

If `format` is not used (thus the component values are enumerated) then the
component values will be flat-listed using their `id` and `name`, independently
from the value of `maxOccurs`. For attributes and measures, if multiple
enumerated values are allowed then the indexes of the corresponding component
values are to listed in arrays in the related `dataSet`.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ```json
    {
      "id": "FREQ",
      "name": "Frequency",
      "names": { "en": "Frequency",
          "fr": "Fréquence" },
      "description": "The time interval at which observations occur over a given time period.",
      "descriptions": { "en": "The time interval at which observations occur over a given time period.",
            "fr": "L'intervalle de temps avec lequel les observations sont faites sur une période donnée." },
      "keyPosition": 0,
      "role": [ "FREQ" ],
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

    {
      "id": "OBS_VALUE",
      "name": "Observation value",
      "names": { "en": "Observation value",
          "fr": "Valeur d'observation" },
      "role": [ "PRIMARY" ],
      "format": {
        # format object #
      }
    }

    {
      "id": "SOURCE",
      "name": "Source",
      "names": { "en": "Source",
            "fr": "Source" },
      "description": "The source of the data depending on the time period.",
      "descriptions": { "en": "The source of the data depending on the time period.",
                "fr": "La source des données dependante de la période de temps." },
      "relationship": {
        # attribute-relationship object #
      },
      "format": {
        # format object #
      },
      "default": "MAFF_Agricultural Statistics",
      "minOccurs": 0,
      "maxOccurs": 3,
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
    ```

##### component link

See the section on [linking mechanism](./2_linking_mechanism.md) for all
information on links.

##### attribute relationship

*Object*. The attribute's relationship defines the relationship between an
attribute and other data structure definition components as defined in the data
structure definition. It provides the original "attachment level", but depending
on the message context (especially the data query) an attribute value can
however be presented physically in the message at a lower level. The attribute
relationship serves also to define the scope, meaning to which measures an
attribute applies.

- dataflow - Empty *Object* *optional*. This means that the value of the
    attribute does **not depend** on any dimension component, and therefore that
    the value of the attribute can **vary** only per dataflow. It is the data
    modeller's responsibility to design or use non-overlapping dataflows that do
    not have observations in common, otherwise the integrity of dataflow-specific
    attribute values is not assured by the model, e.g. when querying those data
    through its DSD.
- dimensions - *Array* of *String*s *optional*. This means that the value of the
    attribute **depends** on only **some** of the dimension components, of which
    the array contains the ID(s), and therefore that the value of the attribute
    can **vary** only with each partial key combination of those dimensions (e.g.,
    a group, a time series or a section).
- observation - Empty *Object* *optional*. This means that the value of the
    attribute **depends** on **all** of the dimension components, and therefore
    that the value of the attribute can **vary** with each observation.
- primaryMeasure - *String* *optional*. Deprecated value having the same
    meaning than the relationship `observation`. For backward-compatibility
    only.
- measures - *Array* of *String*s *optional*. One or more ID(s) of (a) local
    measure(s) in the data structure definition to which the values of this
    attribute **apply**. This is only for informational and presentational
    purposes. If the `measures` relationship is not present then the attribute
    values apply to whole observations.

Exactly one of `dataflow`, `dimensions` and (`observation` or `primaryMeasure`)
is required. Note that relationships defined in data structure definitions 
through a `group` are to be resolved by the server conveniently for the client 
into above `dimensions`.

??? example

    ```json
    {
      "dataflow": {}
    }

    {
      "dimensions": [
        "FREQ", "CURRENCY"
      ]
    }

    {
      "observation": {}
    }

    {
      "measures": [
        "MEAS1", "MEAS2"
      ]
    }

    {
      "primaryMeasure": "OBS_VALUE"  # Deprecated, for backward-compatibility only.
    }
    ```

##### format

*Object*. The format object defines the representation for a component. It
describes the possible text formats for component values (including XHTML and
multi-lingual values) when the component is not enumerated (not using a codelist
or valuelist).

- minOccurs - *Non-negative integer* *optional* **Only for `measures` and
    `attributes`.** Indicates the minimum number of value that must be reported
    for the component. If missing than there is no lower limit on its
    occurrences. If \> 1 then the component values will use the `values`
    property of the component value instead of `id` and `name` or `value`. The
    default is 1.
- maxOccurs - *Positive integer*/*String* *optional* **Only for `measures` and
    `attributes`.** Indicates the maximum number of values that can be reported
    for the component. If set to the string "unbounded" than there is no upper
    limit on its occurrences. If \> 1 then the component values will use the
    `values` property of the component value instead of `id` and `name` or
    `value`. The default is 1.
- dataType - *String* *optional*. Describes the type of data format allowed for
    the representation of the component. Only the following dataTypes are
    supported: "String", "Alpha", "AlphaNumeric", "Numeric", "BigInteger",
    "Integer", "Long", "Short", "Decimal", "Float", "Double", "Boolean", "URI",
    "Count", "InclusiveValueRange", "ExclusiveValueRange", "Incremental",
    "ObservationalTimePeriod", "StandardTimePeriod", "BasicTimePeriod",
    "GregorianTimePeriod", "GregorianYear", "GregorianYearMonth", "GregorianDay",
    "ReportingTimePeriod", "ReportingYear", "ReportingSemester",
    "ReportingTrimester", "ReportingQuarter", "ReportingMonth", "ReportingWeek",
    "ReportingDay", "DateTime", "TimeRange", "Month", "MonthDay", "Day", "Time",
    "Duration", "GeospatialInformation" and "XHTML". `Dimensions` do not support
    the type "XHTML". Time `dimensions` (having the id and role "TIME_PERIOD")
    only support the types "ObservationalTimePeriod", "StandardTimePeriod",
    "BasicTimePeriod", "GregorianTimePeriod", "GregorianYear",
    "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod", "ReportingYear",
    "ReportingSemester", "ReportingTrimester", "ReportingQuarter",
    "ReportingMonth", "ReportingWeek", "ReportingDay", "DateTime", "TimeRange".
    The default data type is "String" except for time `dimensions`, which takes
    "ObservationalTimePeriod" as default.
- isSequence - *Boolean* *optional*. Indicates whether the values are intended
    to be ordered, and it may work in combination with the interval, startValue,
    and endValue attributes or the timeInterval, startTime, and endTime,
    attributes. If this attribute holds a value of 'true', a start value or time
    and a numeric or time interval must supplied. If an end value is not given,
    then the sequence continues indefinitely.
- interval - *Integer* *optional*. Specifies the permitted interval
    (increment) in a sequence. In order for this to be used, the isSequence
    attribute must have a value of 'true'.
- startValue - *Number* *optional*. Is used in conjunction with the isSequence
    and interval attributes (which must be set in order to use this attribute).
    This attribute is used for a numeric sequence, and indicates the starting
    point of the sequence. This value is mandatory for a numeric sequence to be
    expressed.
- endValue - *Number* *optional*. Is used in conjunction with the isSequence
    and interval attributes (which must be set in order to use this attribute).
    This attribute is used for a numeric sequence, and indicates that ending
    point (if any) of the sequence.
- timeInterval - *String* *optional*. Indicates the permitted duration in a
    time sequence. It complies with the time duration specification of the ISO
    8601 standard. In order for this to be used, the isSequence attribute must
    have a value of 'true'.
- startTime - *String* *optional*. Is used in conjunction with the isSequence
    and timeInterval attributes (which must be set in order to use this
    attribute). This attribute is used for a time sequence, and indicates the
    start time of the sequence. This value is mandatory for a time sequence to
    be expressed. It must be a valid standard time period (gYear, gYearMonth,
    date, dateTime and SDMX time periods).
- endTime - *String* *optional*. Is used in conjunction with the isSequence
    and timeInterval attributes (which must be set in order to use this
    attribute). This attribute is used for a time sequence, and indicates that
    ending point (if any) of the sequence. It must be a valid standard time
    period (gYear, gYearMonth, date, dateTime and SDMX time periods).
- minLength - *Positive integer* *optional*. Specifies the minimum length of
    the value in characters.
- maxLength - *Positive integer* *optional*. Specifies the maximum length of
    the value in characters.
- minValue - *Number* *optional*. Is used for inclusive and exclusive ranges,
    indicating what the lower bound of the range is. If this is used with an
    inclusive range, a valid value will be greater than or equal to the value
    specified here. By default, the minValue is assumed to be inclusive.
- maxValue - *Number* *optional*. Is used for inclusive and exclusive ranges,
    indicating what the upper bound of the range is. If this is used with an
    inclusive range, a valid value will be less than or equal to the value
    specified here. By default, the maxValue is assumed to be inclusive.
- decimals - *Positive integer* *optional*. Indicates the number of characters
    allowed after the decimal separator.
- pattern - *String* *optional*. Holds any standard regular expression.
- isMultiLingual - *Boolean* *optional*. **Only for `measures` and
    `attributes`.** This indicates for a text format of type "String" or
    "XHTML", whether the uncoded component value should allow for multiple
    values in different languages. The default is `false`. If `true`, the
    component values will use instead of `id` and `name`:
    - if `minOccurs` and `maxOccurs` are equal to 1: the `value` property,
    - otherwise: the `values` property.
- sentinelValues - *Array* of *Object*s *optional*. When present, the
    sentinelValues array indicates that sentinel values are defined for the data
    format. Each *[sentinelValue](#sentinelvalue)* object indicates a reserved
    value in an otherwise open value domain that holds a specific meaning. For
    example, a value of -1 can be defined to indicate a non-applicable value.

??? example

    ```json
    {
      "dataType": "String",
      "minLength": 4,
      "maxLength": 4,
      "pattern": "^[A-Za-z][A-Za-z0-9_-]*$",
      "isMultilingual": true
    }

    {
      "dataType": "Double",
      "isMultilingual": false,
      "sentinelValues": [
        {
          # sentinel value object #
        }
      ]
    }
    ```
  
###### sentinelValue

*Object*. It defines a reserved value (within the value domain of the data
format) along with its meaning.

- value - *Number* or *String* *optional*. The sentinel value (within the
    value domain of the data format) being described.
- name - *String* *optional*. Human-readable (best-language-match) name (or
    meaning) of the sentinel value.
- names - *Object* *optional*. Human-readable localised *[names](#names)* (or
    meanings) of the sentinel value.
- description - *String* *optional*. Human-readable (best-language-match)
    description for the sentinel value.
- descriptions - *Object* *optional*. Human-readable localised descriptions
    (see *[names](#names)*) for the sentinel value.

??? example

    ```json
    {
      "value": "-1",
      "name": "Non-response",
      "names": { "en": "Non-response",
          "fr": "Non-réponse" },
      "description": "Description for non-response.",
      "descriptions": { "en": "Description for non-response.",
            "fr": "Description de non-réponse." }
    }
    ```

##### component value

*Object* *optional*. A particular value for a component in a message.

- id - *String*. Unique identifier for a coded component value.
- name - *String* *optional*. Human-readable (best-language-match) name for a
    coded component value.
- names - *Object* *optional*. Human-readable localised *[names](#names)* for
    a coded component value.
- value - *String*, *Number*, *Integer*, *Boolean* or localised
    *[Strings](#names)* *optional*. Only for non-coded unique (non-localised or
    localised) component values, e.g. for non-coded dimensions.
- values - *Array* *optional* of *String*, *Number*, *Integer*, *Boolean*,
    localised *[Strings](#names)* and recursive *Array* of such arrays. Only for
    non-coded multivalued (non-localised or localised) component values. Only
    nested multi-valued metadata attribute values (according to the nesting
    defined in the Metadata Structure Definition) can use arrays of arrays.
- description - *String* *optional*. Human-readable (best-language-match)
    description for the component value. The description is typically longer
    than the text provided for the name field.
- descriptions - *Object* *optional*. Human-readable localised descriptions
    (see *[names](#names)*) for the component value. A descriptions is typically
    longer than the text provided for the name field.
- start, end - *String* *optional*. Start and end are instances of time that
    define the actual Gregorian calendar period covered by the values for the
    time dimension. The algorithm for computing start and end fields for any
    supported reporting period is defined in the SDMX Technical Notes. These
    fields should be used only when the component value represents one of the
    values for the time dimension. Values are considered as inclusive both for
    the start field and the end field. Values must follow the ISO 8601 syntax
    for combined dates and times, including time zone. These fields are useful
    for visualisation tools, when selecting the appropriate point in time for
    the time axis. Statistical data, can be collected, for example, at the
    beginning, the middle or the end of the period, or can represent the average
    of observations through the period. Based on this information and using the
    start and end fields, it is easy to get or calculate the desired point in
    time to be used for the time axis.
- parent - *String* *optional*. Contains the ID for the parent of the
    component value (the component value of the parent might also be present in
    the message if according observations are also returned).
- order - *Integer* *optional*. Contains the original order number of the
    component value enabling the reconstruction of the ordered component value
    hierarchy. Note that, to allow for a streamed message generation, the orders
    of observations and of component values in the component value array are not
    significant.
- links - *Array* *optional*. *Links* field is an array of *[component value link](#component-value-link)*
    objects. If appropriate, a collection of links to additional information
    regarding the component value.
- annotations - *Array* *optional*. *[Annotations](#annotation)* is a
    collection of indices of the corresponding *annotations* for the component
    value. Indices refer back to the array of *annotations* in the structure
    field.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ```json
    {
      "id": "AUS",
      "name": "Australia",
      "names": { "en": "Australia", 
            "fr": "Australie" },
      "description": "Description for Australia.",
      "descriptions": { "en": "Description for Australia.",
                "fr": "Déscription pour l'Australie." },
      "parent": "OECD",
      "order": 56,
      "links": [
        {
          # link object #
        }
      ],
      "annotations": [ 5, 49 ]
    }

    {
      "value": "2010",
      "description": "Description for 2010.",
      "descriptions": { "en": "Description for 2010.",
            "fr": "Déscription pour 2010." },
      "start": "2010-01-01T00:00Z",
      "end": "2010-12-31T23:59:59Z"
    }

    {
      "value": "123.4"
    }

    {
      "value": "AAA"
    }

    {
      "value": { "en": "Value", "fr": "Valeur" }
    }

    {
      "values": [1234, 567.8]
    }

    {
      "values": ["AAA", "BBB"]
    }

    {
      "values": [
        { "en": "Value 1", "fr": "Valeur 1" },
        { "en": "Value 2", "fr": "Valeur 2" }
      ]
    }

    {
      "values": [
        ["CONTACT 1 Name 1","CONTACT 1 Name 2"],
        ["CONTACT 2 Name 1","CONTACT 2 Name 2"]
      ]
    }
    ```

###### component value link

See the section on [linking mechanism](./2_linking_mechanism.md) for all
information on links.

### annotation

*Object* *optional*. An `annotation` object can be referenced through its
`annotations` array index by `structure`, `component`, `component value`,
`dataSets`, `series` and `observations`. It contains the following optional
information:

- id - *String* *optional*. ID provides a non-standard identification of an
    annotation. It can be used to disambiguate annotations.
- title - *String* *optional*. Provides a non-localised title for the
    annotation.
- type - *String* *optional*. Type is used to distinguish between annotations
    designed to support various uses. The types are not enumerated, and these
    can be freely specified by the creator of the annotations. The definitions
    and use of annotation types should be documented by their creator.
- value - *String* *optional*. A non-localised value text of the annotation.
- text - *String* *optional*. A human-readable (best-language-match) text of
    the annotation.
- texts - *Object* *optional*. A list of human-readable localised texts (see
    *[names](#names)*) of the annotation.
- links - *Array* *optional*. *Links* field is an array of *[annotation link](#annotation-link)*
    objects. If appropriate, a link to an additional external resource which may
    contain or supplement the annotation.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ```json
    {
      "title": "Sample annotation",
      "type": "reference",
      "value": "123456",
      "text": "Sample annotation text",
      "texts": {
        "en": "Sample annotation text",
        "fr": "Exemple de texte d'annotation"
      },
      "id": "74747",
      "links": [
        {
          # link object #
        }
      ]
    }
    ```

#### annotation link

See the section on [linking mechanism](./2_linking_mechanism.md) for all
information on links.

## dataSet

*Object*. That's where the data (i.e.: the `observations`) will be.

In typical cases, the file will contain only one `dataSet`. However, in some
cases, such as when retrieving, from an SDMX 2.1 web service, what has changed
in the data source since in particular point in time, the web service might
return more than one `dataSet`. Since SDMX 3.0.0, SDMX restful web servic
requests can query data for several structures. In this case, the response
includes one or more separate data sets per returned structure.

Several levels may appear in a `dataSet` object, depending on the way data is
organized. A `dataSet` may contain a flat list of `observations`. In this
scenario, we have 2 main levels in the data part of the message: the `dataSet`
level and the `observation` level. A `dataSet` may also organize observations in
logical groups called `series`. These groups can represent time series or series
by other dimensions (also called cross-sections). In this scenario, we have 3
main levels in the data part of the message: the `dataSet` level, the `series`
level and the `observation` level.

`Dimensions` and `attributes` may be presented at any of these main levels.
However, attributes attached to only specific dimensions maybe presented also at
an intermediate `dimensionGroup` level inbetween the `dataSet` level and the
`series` level. `attributes` may also include reference metadata attributes.
`Measures` are always presented at the `observation` level. In
case the `dataSet` is a flat list of `observations`, `observations` will be
found directly under a `dataSet` object. In case the `dataSet` represents series
of time or series of another dimension, the `observations` will be found under
the `series` elements.

The `dataSet` properties are:

- structure - *Integer* *optional*. *Structure* contains the index of the
    *structure* in the *structures* array of the *data* object, which describe
    the data in this *dataSet*. If omitted, then the data in this *dataSet* are
    assumed to be described by the first *structure* (default: 0).
- action - *String* *optional*. Action provides a list of actions, describing
  the intention of the data transmission from the sender's side. When absent
  (not recommended), `Merge` is assumed.
    - `Merge` - Data or data-related reference metadata is to be merged, through
      either update or insertion depending on already existing information. This
      operation does not allow deleting any component values. Updating
      individual values in multi-valued measure, attribute or data-related
      reference metadata values is not supported either. The complete
      multi-valued value is to be provided. Only non-dimensional components
      (measure, attribute or data-related reference metadata values) can be
      **omitted** (`null` or absent) as long as at least one of those components
      is present. Bulk merges are thus not supported. Only the provided values
      are merged. Dimension values for higher-level (data-related reference
      metadata) attributes can be **switched-off** (using `~`) when those are
      not attached to these dimensions. All observations as well as the sets of
      data-related reference metadata attributes at specific dimension
      combinations impacted by the `Merge` action change their time stamp when
      used to update an SDMX storage system.
    - `Replace` - Data or data-related reference metadata is to be replaced,
      through either update, insert or delete depending on already existing
      information. A full replacement is hereby assumed to take place at
      specific “replacement levels”: for entire observations and for any
      specific dimension combination for data-related reference metadata
      attributes. Within these “replacement levels” the provided values are
      inserted or updated, and omitted values are deleted. Values provided for
      the other attributes (those above the observation level) are merged (see
      `Merge` action). Only non-dimensional components (measure, attribute or
      data-related reference metadata values) can be **omitted** (`null` or
      absent). Bulk replacing is thus not supported. Dimension values for
      higher-level (data-related reference metadata) attributes can be
      **switched-off** (using `~`) when those are not attached to these
      dimensions. Replacing non-existing elements is not resulting in an error.
      All observations as well as the sets of data-related reference metadata
      attributes at specific dimension combinations impacted by the replace
      action change their time stamp when used to update an SDMX storage system.
      Because the `Replace` action always takes place at specific levels, it
      cannot be used to replace a whole dataset or a whole series. However, a
      “replace all” effect can be achieved by combining a `Delete` dataset
      containing a completely wildcarded key (where all dimension values are
      omitted) with a `Merge` or `Replace` dataset within the same data message.
      Similarly, to replace a whole series, a message can combine a `Delete`
      dataset containing only the partial key of the series (where the not used
      dimension values are omitted) with a `Merge` or `Replace` dataset for that
      series.
    - `Delete` - Data or data-related reference metadata is to be deleted.
      Deletion is hereby assumed to take place at the lowest level of detail
      provided in the message. Any component (including dimensions) can be
      **omitted** (dimensions: empty, others: `null` or absent). Omitting
      dimension values allows for bulk deletions. Partially omitting
      non-dimension component values allows restricting the deletion of measure,
      attribute or data-related reference metadata values to the ones being
      present. Instead of real values for non-dimensional components, it is
      sufficient to use any valid value. With this, whole datasets, any slices
      of observations for dimension groups such as time series, observations or
      individual measure, attribute and data-related reference metadata
      attributes values can be deleted. Dimension values for higher-level
      (data-related reference metadata) attributes can be **switched-off**
      (using `~`) when those are not attached to these dimensions. Deleting
      non-existing elements or values is not resulting in an error. All
      observations as well as the sets of attributes and data-related reference
      metadata at higher partial keys impacted by the `Delete` action change
      their time stamp when used to update an SDMX storage system.
    - `Append` - Deprecated. When used to update an SDMX storage system, the
      `Merge` action is assumed.
    - `Information` - Deprecated. When used to update an SDMX storage system,
      the `Merge` action is assumed.
- reportingBegin - *String* *optional*. The start of the time period covered
    by the message.
- reportingEnd - *String* *optional*. The end of the time period covered by
    the message.
- validFrom - *String* *optional*. The validFrom indicates the inclusive start
    time indicating the validity of the information in the data.
- validTo - *String* *optional*. The validTo indicates the inclusive end time
    indicating the validity of the information in the data.
- publicationYear - *String* *optional*. The publicationYear holds the ISO
    8601 four-digit year.
- publicationPeriod - *String* *optional*. The publicationPeriod specifies the
    period of publication of the data in terms of whatever provisioning
    agreements might be in force (i.e., "2005-Q1" if that is the time of
    publication for a `dataSet` published on a quarterly basis).
- links - *Array* *optional*. *Links* field is an array of *[dataSet link](#dataset-link)*
    objects. If appropriate, a collection of links to additional information
    regarding the dataSet.
- annotations - *Array* *optional*. *[Annotations](#annotation)* is a
    collection of indices of the corresponding *annotations* for the dataSet.
    Indices refer back to the array of *annotations* in the structure field.
- attributes - *Array* *optional*.
    *[Attributes](#dimensions-measures-attributes)* is a collection of
    *attribute values or indices of the corresponding values*, depending on the
    presence of the `values` array in the component definition, of all
    attributes presented at the `dataSet` level. Indexes correspond to the
    indexes of the `values` array of the respective *component* object within
    the `structure.attributes.dataSet` array, and are always *zero* because
    attributes presented at the `dataSet` level can only have one value. This is
    typically the case for `attributes` that always have the same value for all
    the `observations` available in the `dataSet`. In order to avoid repetition,
    that value can simply be presented at the `dataSet` level. When an attribute
    has no value for a specific `dataSet`, then *null* is used instead of the
    value or its index.
- dimensionGroupAttributes - *Object* *optional*.
    *[DimensionGroupAttributes](#dimensiongroupattributes)* is a collection of
    *attribute values or indices of the corresponding values*, depending on the
    presence of the `values` array in the component definition, of all
    attributes presented at the `dimensionGroup` level.
- series - *Object* *optional*. A collection of *[series](#series)* objects,
    to be used when the `observations` contained in the `dataSet` are presented
    in logical groups (time series or cross-sections), e.g. when using the SDMX
    API with the parameter "dimensionAtObservation=TIME_PERIOD" (default option)
    or with the "dimensionAtObservation" parameter with an ID of any other
    specific `dimension`. This element must **not** be used in case the
    `dataSet` presents a flat view of `observations`.
- observations - *Object* *optional*. *[Observations](#observations)* is a
    collection of *measure and/or attribute values or indices of the
    corresponding values*, depending on the presence of the `values` array in
    the component definition, of all measures and of all attributes presented at
    the `observation` level. It also contains the indexes of annotations
    applying to observations. This collection is used in case when a `dataSet`
    is presented as a flat view of `observations`, e.g. when using the SDMX API
    with the parameter "dimensionAtObservation=AllDimensions". All `dimensions`,
    except those with only one `value` possibly presented at the `dataSet`
    level, would be presented at `observation` level. Alternatively, in case the
    `observations` are to be presented in logical groups (time series or
    cross-sections), the *[series](#series)* element is to be used instead.

For information on how to handle the indexes for `dimensions`, `measures`,
`attributes` and `annotations` see the section dedicated to [handling
indexes](./3_handling_indexes.md).

Important notes about the `action` types:

The terms “delete”, “merge” and “replace” do not imply a physical replacement or
deletion of values in the underlying database. To minimize the physical resource
requirements, SDMX web service implementations that do not support the
`includeHistory` and `asOf` URL parameters might physically replace the existing
values in the database. SDMX web services that neither support the
`updatedAfter` URL parameter might also implement physical deletions. However,
SDMX web services that support these parameters (or other time-machine
features), would not overwrite or delete the physical values.  
SDMX web services that support the `includeHistory` or `asOf` URL parameters
should never allow deleting their historic data content because this would
interfere with the interests of data consumers, such as data aggregators.
Therefore, a specific feature to physically delete previous (outdated) content
is intentionally not added to the SDMX standard syntax. If such a feature is
required by an organisation, then it needs to be implemented as a custom feature
outside the SDMX standard.  
Likewise, all SDMX-compliant systems that do (or are configured to) support the
`updatedAfter` URL parameter need to systematically retain the information about
deleted data (or data-related reference metadata).  
All datasets – even with varying actions – within a single data message have
always to be treated as **ACID transaction** to guarantee “transactional safety”
(full data consistency and validity despite errors, power failures, and other
mishaps). These datasets are to be processed in the order of appearance in the
message. The advantage of such data messages is thus the ability to bundle
separate `Delete` and `Replace` or `Merge` actions into one transactional data
message.

Recommended dataset actions in SDMX web service responses to GET data queries:

1. Without the `updatedAfter`, `includeHistory`, `detail`, `attributes` or
   `measures` URL parameters: The response message should contain the retrieved
   data in a replace dataset (instead of the previous information dataset).
2. Without the `updatedAfter` and `includeHistory`, but with `detail`,
   `attributes` or `measures` URL parameters: The response message should
   contain the retrieved data in a merge dataset (instead of the previous
   information dataset).
3. With the `updatedAfter` URL parameter: The response must include the
   information of all previously updated, inserted and deleted data or
   data-related reference metadata, even if bulk deletions have been used,
   because the DB synchronization use case requires that the generated response
   must always allow achieving to replicate the exact same punctual data content
   as currently stored in the queried data source. One of the two approaches are
   possible:
    - a `Delete` dataset for entirely deleted observations and for entirely
      deleted sets of (data-related reference metadata) attribute values
      attached to specific dimension combinations and a `Replace` dataset for
      all other changed observations and changed attribute and data-related
      reference metadata values attached to specific dimension combinations, or
    - a `Delete` dataset for entirely deleted observations, for entirely deleted
      sets of (data-related reference metadata) attribute values attached to
      specific dimension combinations and for individually deleted mesure,
      attribute and data-related reference metadata values and a `Merge` dataset
      for all other updated or inserted observation, attribute and data-related
      reference metadata values.
4. With the `includeHistory` URL parameter: Using a number of datasets with
   `Delete`, `Replace` or `Merge` actions and limited in their validity time
   span that allow achieving to replicate the exact same punctual data contents
   as previously stored in the queried data source.
5. With the `asOf` URL parameter: The recommendations of 1 and 2 apply depending
   on other parameters. In addition, the returned dataset should have its
   validity time span limited to the point in time requested in the `asOf`
   parameter.

??? example

    ```json
    {
      "structure": 0,
      "action": "Merge",
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
      "attributes": [ 
        0, 
        null, 
        456.7, 
        "This is a string value", 
        {"en": "English Text", "fr": "Texte français"}, 
        [123.4, 456.7], ["A", "B"], 
        [{"en": "English Text 1", "fr": "Texte français 1"}, {"en": "English Text 2", "fr": "Texte français 2"}] ],
      "dimensionGroupAttributes": {
        # dimensionGroupAttributes object #
      },
      "series": {
        # series object #
      }
    }

    {
      "structure": 0,
      "action": "Replcac",
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
      "attributes": [ 
          0, 
          null, 
          456.7, 
          "This is a string value", 
          {"en": "English Text", "fr": "Texte français"}, 
          [123.4, 456.7], 
          ["A", "B"], 
          [{"en": "English Text 1", "fr": "Texte français 1"}, {"en": "English Text 2", "fr": "Texte français 2"}] ],
      "dimensionGroupAttributes": {
        # dimensionGroupAttributes object #
      },
      "observations": {
        # observations object #
      }
    }
    ```

### dataSet link

See the section on [linking mechanism](./2_linking_mechanism.md) for all
information on links.

### dimensionGroupAttributes

*Object* *optional*. Collection of *values or value indexes* of all attributes
presented at the `dimensionGroup` level, in form of JSON *name/value pairs*.
Values are presented if the attribute definition doesn't contain the `values`
array.\
Indexes of values are presented if the attribute definition does contain the
`values` array.

The `dimensionGroupAttributes` object contains a JSON *name/value pair* for each
*partial dimension value combination* for which a `dimensionGroup` level
attribute value exists.

The *name* in the JSON *name/value pair* represents the unique (partial)
dimension value combination to which the dimensionGroup attribute values are
attached. "Partial" combination means that DimensionGroup attributes are not
attached to all dimensions but only to a subset. This combination is the
concatenation of the indexes of the corresponding `values` of the `dimensions`,
separated by a colon character (":"), e.g. "::4:0:". Note that dimension values
to which the attribute value is attached take the indexes from the `values`
array of the respective *component* objects within the `structure.dimensions`
object, whereas the other dimensions (which are not part of the partial key)
have no value (are left empty). All dimensions have therefore their well-defined
position in the *name* combination independently from the level at which
dimensions are presentated elsewhere in the message.\
In the example "::4:0:", the data has altogether 5 dimensions (including the
time dimension), and the related attribute values are attached only to the 3rd
and 4th dimensions, for which the dimension value indexes are 4 and 0. The
dimension 1, 2 and 5 have no related value and their positions in the
combination are left empty.

The *value* in the JSON *name/value pair* is an array containing for the given
`dimensionGroup` combination:

- first: the corresponding values of **all** *attributes* presented in the
    `structure.attributes.dimensionGroup` array or the indexes of these values,
    depending on the presence of the `values` array in the component definition,
- and last: the indexes of the values of all corresponding `annotations`.

Elements for `annotations` are only included if there are corresponding
`annotations`. **If `annotations` are present, then all corresponding
`attributes` must be included.** Otherwise, if there are no corresponding
`annotations`, then beginning from the end of the array, the corresponding
`attributes` can be omitted if:

- the `attribute` has no value (e.g. for optional attributes) or
- the `attribute` value corresponds to the default value.

If an attribute that has to be listed in the array does not have (a) value(s)
then *null* is used instead of a value.

The *name/value pair* is only present if there is at least one corresponding
`attribute` or `annotation` value.

The data type for the array `attribute` elements must be *integer*, *number*,
*string*, *object of localised strings* (see: *[names](#names)*), arrays of these
4 types or *null*. Indexes for `attribute` values are of type *integer* and
correspond to the indexes in the `values` array of the respective `component`
object within the `structure.attributes.dimensionGroup` array.\
The data type for the array `annotation` elements is *integer*. Indexes for
`annotation` values correspond to the indexes in the array of `annotations` in
the `structure` element.

For information on how to handle the indexes for `dimensions`, `attributes` and
`annotations` see the section dedicated to [handling
indexes](./3_handling_indexes.md).

??? example

    For this example, to ease understanding, let's consider a CSV format with
    horizontal time series (with header row):

    ``` csv
    DIM1,DIM2,DIM3,MEAS1,MEAS2,ATTR1_OBS,ATTR2_DIMGROUP,ATTR3_DIMGROUP
    DIM1_VALUE_1,DIM2_VALUE_1,DIM3_VALUE_1,10,20,ATTR1_VALUE_1,ATTR2_VALUE_1,ATTR3_VALUE_1
    DIM1_VALUE_1,DIM2_VALUE_1,DIM3_VALUE_2,11,21,ATTR1_VALUE_2,ATTR2_VALUE_2,ATTR3_VALUE_1
    DIM1_VALUE_1,DIM2_VALUE_2,DIM3_VALUE_1,12,22,ATTR1_VALUE_3,ATTR2_VALUE_1,ATTR3_VALUE_1
    DIM1_VALUE_1,DIM2_VALUE_2,DIM3_VALUE_2,13,23,ATTR1_VALUE_4,ATTR2_VALUE_2,ATTR3_VALUE_1
    ```

    !!! note
        Attribute `ATTR1_OBS` is attached at observation level and thus depends on all
        dimensions.
        Attribute `ATTR2_DIMGROUP` is attached to the dimensions `DIM1` and `DIM3` only.
        Its values only vary with the values
        of those dimensions.
        Attribute `ATTR3_DIMGROUP` is attached to the dimension `DIM1` only. Its
        values only vary with the values of this dimension.

    In SDMX-JSON, dimension values are replaced by their indices. Attribute values
    may be replaced by their indices,
    which is **not** done in this example:

    ```json
    "dimensionGroupAttributes": {
    "0::": { null, 0 },
    "0::0": { "ATTR2_VALUE_1" },
    "0::1": { "ATTR2_VALUE_2" }
    },
    "observations": {
    "0:0": [10, 20, "ATTR1_VALUE_1"],
    "0:1": [11, 21, "ATTR1_VALUE_2"],
    "1:0": [12, 22, "ATTR1_VALUE_3"],
    "1:1": [13, 23, "ATTR1_VALUE_4"]
    }

    /*
    dimensionGroup 1: "0::" corresponds to the 1 index for "DIM1":"DIM1_VALUE_1". All other dimensions are discarded. 
            The attributes for this dimension group are: "ATTR2_DIMGROUP": null (no correspondance), "ATTR3_DIMGROUP": "ATTR3_VALUE_1"
            
    dimensionGroup 2: "0::0" corresponds to the 2 indexes for "DIM1":"DIM1_VALUE_1" and "DIM3":"DIM3_VALUE_1". 
    The "DIM2" dimension is discarded.
            The attributes for this dimension group are: "ATTR2_DIMGROUP": "ATTR2_VALUE_1", "ATTR3_DIMGROUP": 
            no correspondance and omitted since last array element

    dimensionGroup 2: "0::1" corresponds to the 2 indexes for "DIM1":"DIM1_VALUE_1" and "DIM3":"DIM3_VALUE_2". 
    The "DIM2" dimension is discarded.
            The attributes for this dimension group are: "ATTR2_DIMGROUP": "ATTR2_VALUE_2", "ATTR3_DIMGROUP": 
            no correspondance and omitted since last array element

    Observation 1: "0:0" corresponds to the 2 indices for "DIM2":"DIM2_VALUE_1", "DIM3":"DIM3_VALUE_1" 
    (DIM1 is omitted since presented at dataSet level to avoid its repetition for each observation)
                    The measures for this observation are: 
                "MEAS1": 10
                "MEAS2": 20
                    The attributes for this observation are: 
                    "ATTR1_OBS": "ATTR1_VALUE_1"
    Observation 2: "0:0" corresponds to the 2 indices for "DIM2":"DIM2_VALUE_1", "DIM3":"DIM3_VALUE_2" 
    (DIM1 is omitted since presented at dataSet level to avoid its repetition for each observation)
                    The measures for this observation are: 
                "MEAS1": 11
                "MEAS2": 21
                    The attributes for this observation are: 
                    "ATTR1_OBS": "ATTR1_VALUE_2"
    Observation 3: "0:0" corresponds to the 2 indices for "DIM2":"DIM2_VALUE_2", "DIM3":"DIM3_VALUE_1" 
    (DIM1 is omitted since presented at dataSet level to avoid its repetition for each observation)
                    The measures for this observation are: 
                "MEAS1": 12
                "MEAS2": 22
                    The attributes for this observation are: 
                    "ATTR1_OBS": "ATTR1_VALUE_3"
    Observation 4: "0:0" corresponds to the 2 indices for "DIM2":"DIM2_VALUE_2", "DIM3":"DIM3_VALUE_2" 
    (DIM1 is omitted since presented at dataSet level to avoid its repetition for each observation)
                    The measures for this observation are: 
                "MEAS1": 13
                "MEAS2": 23
                    The attributes for this observation are: 
                    "ATTR1_OBS": "ATTR1_VALUE_4"    
    */

    "dimensions": {
    "dataSet": [
        {
        "id": "DIM1",
        "name": "Dimension 1",
        "names": { "en": "Dimension 1" },
        "values": [
            {
            "id": "DIM1_VALUE_1",
            "name": "Dimension 1 - Value 1 with index 0",
            "names": { "en": "Dimension 1 - Value 1 with index 0" }
            }
        ]
        }
    ],
    "series": [],
    "observation": [
        {
        "id": "DIM2",
        "name": "Dimension 2",
        "names": { "en": "Dimension 2" },
        "values": [
            {
            "id": "DIM2_VALUE_1",
            "name": "Dimension 2 - Value 1 with index 0",
            "names": { "en": "Dimension 2 - Value 1 with index 0" }
            },
            {
            "id": "DIM2_VALUE_2",
            "name": "Dimension 2 - Value 2 with index 1",
            "names": { "en": "Dimension 2 - Value 2 with index 1" }
            }
        ]
        },
        {
        "id": "DIM3",
        "name": "Dimension 3",
        "names": { "en": "Dimension 3" },
        "values": [
            {
            "id": "DIM3_VALUE_1",
            "name": "Dimension 3 - Value 1 with index 0",
            "names": { "en": "Dimension 3 - Value 1 with index 0" }
            },
            {
            "id": "DIM3_VALUE_2",
            "name": "Dimension 3 - Value 2 with index 1",
            "names": { "en": "Dimension 3 - Value 2 with index 1" }
            }
        ]
        }
    ]
    },
    "attributes": {
    "dataSet": [],
    "dimensionGroup": [
        {
        "id": "ATTR2_DIMGROUP",
        "name": "Attribute 2",
        "names": { "en": "Attribute 2" },
        "format": {
            "dataType": "String"
        }
        },
        {
        "id": "ATTR3_DIMGROUP",
        "name": "Attribute 3",
        "names": { "en": "Attribute 3" },
        "values": [
            {
            "id": "ATTR3_VALUE_1",
            "name": "Attribute 3 - Value 1 with index 0",
            "names": { "en": "Attribute 3 - Value 1 with index 0" }
            }
        ]
        }
    ],
    "series": [],
    "observation": [
        {
        "id": "ATTR1_OBS",
        "name": "Attribute 1",
        "names": { "en": "Attribute 1" },
        "format": {
            "dataType": "String"
        }
        }
    ]
    }
    ```

  !!! note
      For attributes "ATTR2_DIMGROUP" and "ATTR1_OBS", the values are omitted in
      the attribute definitions and thus presented directly in the dataSets'
      `dimensionGroupAttributes` and `observations` (instead of indexes).

### series

*Object* *optional*. Collection of series in form of JSON *name/value* pairs,
when the `observations` contained in the `dataSet` are used into logical groups
(time series or cross-sections). Each underlying series is represented as a JSON
*name/value pair* ("name": "value") in the `series` object.

The *name* in the JSON *name/value pair* represents the unique series
identifier, which is the concatenation of the indexes of the `values` of all
`dimensions` presented at `series` level (i.e. indexes of the fields in the
`values` array of the respective *component* object within the
`structure.dimensions.series` array) separated by a colon character (":"),
e.g. "0:0:4:10:2".

The *value* in the JSON *name/value pair* is an object containing:

- annotations - *Array* *optional*. *[Annotations](#annotation)* is a
    collection of indices of the corresponding *annotations* for the series.
    Indices refer back to the array of `annotations` in the structure field.
- attributes - *Array* *optional*.
    *[Attributes](#dimensions-measures-attributes)* is a collection of
    *attribute values or indexes of the corresponding values*, depending on the
    presence of the `values` array in the component definition, of all
    `attributes` presented at the `series` level. Indexes correspond to the
    indexes in the `values` array of the respective `component` object within
    the `structure.attributes.series` array. This is typically the case for
    `attributes` that always have the same value for all the `observations`
    available in the series. In order to avoid repetition, that value can simply
    be presented at the `series` level. When an attribute has no value for a
    specific series, then *null* is used instead of the index.
- observations - *Object* *optional*. *[Observations](#observations)* is a
    collection of *measure and/or attribute values or indices of the
    corresponding values*, depending on the presence of the `values` array in
    the component definition, of all measures and of all attributes presented at
    the `observation` level. It also contains the indexes of annotations
    applying to observations. The `dimension` specified in the
    "dimensionAtObservation" parameter of the SDMX API is used to identify the
    observations of a series. By default that dimension is the time dimension
    (with ID "TIME_PERIOD").

For information on how to handle the indexes for `dimensions`, `measures`,
`attributes` and `annotations` see the section dedicated to [handling
indexes](./3_handling_indexes.md).

??? example

    For this example, to ease understanding, let's consider a CSV format with
    horizontal time series (with header row):

    ```csv
    DIM1,DIM2,Value for 2016,Value for 2017,ATTR1,ATTR2,ANNOT,ATTR3 for 2016,ATTR3 for 2017
    DIM1_VALUE_1,DIM2_VALUE_1,1.5931,1.5925,ATTR1_VALUE_1,"""en:English Text 1;fr:Texte français 1"";""en:English Text 2;fr:Texte français 2""",,ATTR3_VALUE_1,
    DIM1_VALUE_1,DIM2_VALUE_2,40.3426,40.3000,ATTR1_VALUE_2,,ANNOT_VALUE1,ATTR3_VALUE_1,ATTR3_VALUE_1
    ```

    In SDMX-JSON, using "dimensionAtObservation=TIME_PERIOD" (default) the observations are presented in a similar way, 
    grouped by time series (with the TIME_PERIOD dimension at observation level), but dimension values are replaced by 
    their indices. Attribute values may also be replaced by their indexes, which - in this example - is done for ATTR1 
    and ATTR3 but **not** for ATTR2:
  
    ```json
    "series": {
      "0:0": {
        "attributes": [
          0, 
          [{"en": "English Text 1", "fr": "Texte français 1"}, {"en": "English Text 2", "fr": "Texte français 2"}]
          ],
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
              The attributes for this series are: "ATTR1":"ATTR1_VALUE_1", 
              "ATTR2":"""en:English Text 1;fr:Texte français 1"";""en:English Text 2;fr:Texte français 2"""
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
              The attributes for this series are: "ATTR1":"ATTR1_VALUE_2" 
              (omitted because this is the default value and since the only and last attribute)
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
          "names": { "en": "Dimension 1" },
          "values": [
            {
              "id": "DIM1_VALUE_1",
              "name": "Dimension 1 - Value 1 with index 0",
              "names": { "en": "Dimension 1 - Value 1 with index 0" }
            }
          ]
        },
        {
          "id": "DIM2",
          "name": "Dimension 2",
          "names": { "en": "Dimension 2" },
          "values": [
            {
              "id": "DIM2_VALUE_1",
              "name": "Dimension 2 - Value 1 with index 0",
              "names": { "en": "Dimension 2 - Value 1 with index 0" }
            },
            {
              "id": "DIM2_VALUE_2",
              "name": "Dimension 2 - Value 2 with index 1",
              "names": { "en": "Dimension 2 - Value 2 with index 1" }
            }
          ]
        }
      ],
      "observation": [
        {
          "id": "TIME_PERIOD",
          "name": "Time Period",
          "names": { "en": "Time Period" },
          "values": [
            {
              "value": "2016"
            },
            {
              "value": "2017"
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
          "names": { "en": "Attribute 1" },
          "default": "ATTR1_VALUE_2",
          "values": [
            {
              "id": "ATTR1_VALUE_1",
              "name": "Attribute 1 - Value 1 with index 0",
              "names": { "en": "Attribute 1 - Value 1 with index 0" }
            },
            {
              "id": "ATTR1_VALUE_2",
              "name": "Attribute 1 - Value 2 with index 1",
              "names": { "Attribute 1 - Value 2 with index 1" }
            }
          ]
        },
        {
          "id": "ATTR2",
          "name": "en": "Attribute 2",
          "names": { "en": "Attribute 2" },
          "format": {
            "maxOccurs": 2,
            "dataType": "String",
            "isMultiLingual": true
          }
        }
      ],
      "observation": [
        {
          "id": "ATTR3",
          "name": "Attribute 3",
          "names": { "en": "Attribute 3" },
          "default": "ATTR3_VALUE_2",
          "values": [
            {
              "id": "ATTR3_VALUE_1",
              "name": "Attribute 3 - Value 1 with index 0",
              "names": { "en": "Attribute 3 - Value 1 with index 0" }
            },
            {
              "id": "ATTR3_VALUE_2",
              "name": "Attribute 3 - Value 2 with index 1",
              "names": { "en": "Attribute 3 - Value 2 with index 1" }
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
        "texts": { "en": "Sample annotation text" },
        "id": "ANNOT_VALUE1"
      }
    ]
    ```

    !!! note
        For attribute "ATTR2", the values are omitted in the attribute definition 
        and thus presented directly in the dataSets' `observations` (instead of indexes).

### observations

*Object* *optional*. Collection of observations in form of JSON *name/value
pairs*. Each underlying observation is represented as a JSON *name/value pair*
in the `observations` object.

The *name* in the JSON *name/value pair* represents the unique observation
identifier, which is the concatenation of the indexes of the `values` of all
`dimensions` presented at `observation` level (i.e. indexes of the fields in the
`values` array of the respective *component* object within the
`structure.dimensions.observation` array) separated by a colon character (":").
The `dimension` specified in the "dimensionAtObservation" parameter of the SDMX
API is used to identify the observations. By default that dimension is the time
dimension (with ID "TIME_PERIOD"). Thus, there is only one single index in the
observation identifier, e.g. "2". With "dimensionAtObservation=allDimensions",
when the data are represented as a flat view of observations, all dimensions
with more than 1 value will be presented at observation level. Here, there can
be several concatenated indexes in the observation identifier,
e.g. "0:0:4:10:2".

The *value* in the JSON *name/value pair* is an array containing:

- first: the corresponding values of **all** *measures* (as presented in the
    `structure.measures.observation` array) or the indexes of these values,
    depending on the presence of the `values` array in the component definition,
- followed by: the corresponding values of **all** *attributes* presented in
    the `structure.attributes.observation` array or the indexes of these values,
    depending on the presence of the `values` array in the component definition,
- and last: the indexes of the values of all `annotations` of that
    observation.

Therefore, the array elements after the `measures` are for the `observation`
level `attributes` and for `annotations` of that observation. Elements for
`annotations` are only included if there are `annotations` for that observation.
**If `annotations` are present for an observation, then all `attributes` defined
at `observation` level must be included.** Otherwise, if the observation has no
`annotations`, then beginning from the end of the array, `observation` level
`attributes` can be omitted if:

- the `attribute` is not set for this observation (possible for optional
    attributes) or
- the `attribute` value for this observation corresponds to the default value.

The data type for the array `measure` and `attribute` elements is *integer*,
*number*, *string*, *object of localised strings* (see: *[names](#names)*),
arrays of these 4 types or *null*. The last is for a reported `measure` measure
value or for unused optional `attributes` when the attribute position in the
array needs to be filled. Indexes for `measure` values are of type *integer* and
correspond to the indexes in the `values` array of the respective `component`
object within the `structure.measures.observation` array. Indexes for
`attribute` values are of type *integer* and correspond to the indexes in the
`values` array of the respective `component` object within the
`structure.attributes.observation` array. The data type for the array
`annotation` elements is *integer*. Indexes for `annotation` values correspond
to the indexes in the array of `annotations` in the `structure` element.

For information on how to handle the indexes for `dimensions`, `measures`,
`attributes` and `annotations` see the section dedicated to [handling
indexes](./3_handling_indexes.md).

??? example
    For this example, to ease understanding, let's consider data in a flat CSV
    format (with header row):

    ```csv
    DIM1,DIM2,MEAS1,MEAS2,ATTR1,ATTR2,ATTR3,ANNOT
    DIM1_VALUE_1,DIM2_VALUE_1,105.6,120.8,ATTR1_VALUE_1;ATTR1_VALUE_2,ATTR2_VALUE_1,,ANNOT_VALUE1
    DIM1_VALUE_1,DIM2_VALUE_2,105.9,120.2,ATTR1_VALUE_1,ATTR2_VALUE_2,,
    ```

    In SDMX-JSON, the observations are presented in a similar flattened way,
    but dimension and attribute values are replaced by their indices:
    
    ```json
    "observations": {
      "0:0": [105.6, 120.8, ["ATTR1_VALUE_1","ATTR1_VALUE_2"], 0, null, 0],
      "0:1": [105.9, 120.2, ["ATTR1_VALUE_1"], 1]
    }

    /*
    Observation 1: "0:0" corresponds to the 2 indices for "DIM1":"DIM1_VALUE_1", "DIM2":"DIM2_VALUE_1"
                  The measures for this observation are:
              105.6
              120.8
                  The attributes for this observation are:
                    "ATTR1": "ATTR1_VALUE_1", "ATTR1_VALUE_2"
        "ATTR2": "ATTR2_VALUE_1" (0 is the index of the corresponding element in the attribute's "values" array)
                    "ATTR2": null (null is used as placeholder before the next array position for the following annotation)
                  The annotation for this observation is:
              "ANNOT": "ANNOT_VALUE1" (0 is the index of the corresponding element in the annotation array)
    Observation 2: "0:1" corresponds to the 2 indices for "DIM1":"DIM1_VALUE_1", "DIM2":"DIM2_VALUE_2"
                  The measures for this observation are:
              105.9
              120.2
                  The attributes for this observation are:
                    "ATTR1":"ATTR1_VALUE_1"
        "ATTR2":"ATTR2_VALUE_2" (1 is the index of the corresponding element in the attribute's "values" array)
    */

    "dimensions": {
      "dataSet": [],
      "series": [],
      "observation": [
        {
          "id": "DIM1",
          "name": "Dimension 1",
          "names": { "en": "Dimension 1" },
          "values": [
            {
              "id": "DIM1_VALUE_1",
              "name": "Dimension 1 - Value 1 with index 0",
              "names": { "en": "Dimension 1 - Value 1 with index 0" }
            }
          ]
        },
        {
          "id": "DIM2",
          "name": "Dimension 2",
          "names": { "en": "Dimension 2" },
          "values": [
            {
              "id": "DIM2_VALUE_1",
              "name": "Dimension 2 - Value 1 with index 0",
              "names": { "en": "Dimension 2 - Value 1 with index 0" }
            },
            {
              "id": "DIM2_VALUE_2",
              "name": "Dimension 2 - Value 2 with index 1",
              "names": { "en": "Dimension 2 - Value 2 with index 1" }
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
          "names": { "en": "Attribute 1" },
          "format": {
            "dataType": "String",
            "maxValue": 2
          }
        },
        {
          "id": "ATTR2",
          "name": "Attribute 2",
          "names": { "en": "Attribute 2" },
          "values": [
            {
              "id": "ATTR2_VALUE_1",
              "name": "Attribute 2 - Value 1 with index 0",
              "names": { "en": "Attribute 2 - Value 1 with index 0" }
            },
            {
              "id": "ATTR2_VALUE_2",
              "name": "Attribute 2 - Value 2 with index 1",
              "names": { "en": "Attribute 2 - Value 2 with index 1" }
            }
          ]
        },
        {
          "id": "ATTR3",
          "name": "Attribute 3",
          "names": { "en": "Attribute 3" },
          "default": "ATTR3_VALUE_1",
          "values": [
            {
              "id": "ATTR3_VALUE_1",
              "name": "Attribute 3 - Value 1 with index 0",
              "names": { "en": "Attribute 3 - Value 1 with index 0" }
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
        "texts": { "en": "Sample annotation text" },
        "id": "ANNOT_VALUE1"
      }
    ]
    ```
    
    !!! note
        For attribute "ATTR1", the values are omitted in the attribute definition and 
        thus presented directly in the dataSets' `observations` (instead of indexes).

## statusInformation

*Object* *optional*. Used to provide status information with more detail to the
HTTP status codes in the RESTful SDMX web service responses. The following
pieces of information should be provided:

- code - *Number*. Provides an HTTP status code number for the status
  information if appropriate. See
  [RESTful SDMX web service error handling and status information](https://github.com/sdmx-twg/sdmx-rest/blob/master/doc/status.md).
- title - *String* *optional*. A short, human-readable (best-language-match)
    summary of the situation that SHOULD NOT change from occurrence to
    occurrence of the status, except for purposes of localization.
- titles - *Object* *optional*. A list of short, human-readable localised
    summaries (see *[names](#names)*) of the status that SHOULD NOT change from
    occurrence to occurrence of the status, except for purposes of localization.
- detail - *String* *optional*. A human-readable (best-language-match)
    explanation specific to this occurrence of the status. Like title, this
    field's value can be localized. It is fully customizable by the service
    providers and should provide enough detail to ease understanding the reasons
    of the status.
- details - *Object* *optional*. A list of human-readable localised
    explanations (see *[names](#names)*) specific to this occurrence of the
    status. Like titles, this field's value can be localized. It is fully
    customizable by the service providers and should provide enough detail to
    ease understanding the reasons of the status.
- links - *Array* *optional*. *Links* field is an array of *[link](#link)*
    objects. If appropriate, a collection of links to additional external
    resources for the status information.


See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ```json
    {
      "code": 400,
      "title": "Invalid number of dimensions in the key parameter",
      "titles": { "en": "Invalid number of dimensions in the key parameter"
            "fr": "Nombre invalide de dimensions dans le paramètre 'key'" }
    }
    ```
