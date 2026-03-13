# Field Guide to SDMX-JSON 2.1 Metadata Message Objects (aligned with SDMX 3.1.0)

## message

Message is the top level object and it contains the requested information
(referential metadata) as well as the meta-information describing the (technical)
context of the message and, possibly, status information.

- $schema - *String* *optional*. Contains the URL to the schema allowing to
  validate the message. This also allows identifying the version of SDMX-JSON
  format used in this message. **Providing the link to the SDMX-JSON schema is
  recommended.**
- meta - *Object* *optional*. A *[meta](#meta)* object that contains
  non-standard meta-information and basic technical information about the
  message, such as when it was prepared and who has sent it.
- data - *Object* *optional*. *[Data](#data)* contains the message's “primary
  data”.
- errors - *Array* *optional* of *[statusInformation](#statusinformation)*
  objects providing - when appropriate - more detail to the HTTP status codes in
  the RESTful SDMX web service responses.

The properties data and errors CAN coexist in the same message.

??? example

    ```json
    {
      "$schema": "https://json.sdmx.org/2.1/sdmx-json-metadata-schema.json",
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

*Object* *optional*. Used to include non-standard meta-information and basic
technical information about the message, such as when it was prepared and who
has sent it. Any members MAY be specified within `meta` objects.

- schema - *String* *optional*. Deprecated and replaced by the `$schema`
  property at the root level, which allows for automated validations.
- id - *String*. Unique string that identifies the message for further
  references.
- test - *Boolean* *optional*. Indicates whether the message is for test
  purposes or not. False for normal messages.
- prepared - *String*. A date or date-time indicating when the message was prepared.
  Values must follow the ISO 8601 syntax for dates or combined dates and times, including
  time zone.
- contentLanguages - *Array* *optional*. Array of strings containing the
  identifier of all languages used anywhere in the message for localized
  elements, and thus the languages of the intended audience, representing in an
  array format the same information than the http Content-Language response
  header, e.g. "en, fr-fr". See
  [IETF Language Tags](https://tools.ietf.org/html/rfc5646#section-2.1).
  The array's first element
  indicates the main language used in the message for localized elements. **The
  usage of this property is recommended.**
- name - *String* *optional*. Human-readable (best-language-match) name for the
  transmission.
- names - *Object* *optional*. Human-readable localised *[names](#names)* for
  the transmission.
- sender - *Object*. *[Sender](#sender)* contains information about the party
  that is transmitting the message.
- receivers - *Array* *optional* of *[Receiver](#receiver)* objects thats
  contain information about the party that is receiving the message. This can be
  useful if the WS requires authentication.
- links - *Array* *optional*. *Links* field is an array of *[link](#meta-link)*
  objects. If appropriate, a collection of links to additional external
  resources for the header.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
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
- name - *String* *nullable*. A human-readable (best-language-match) name of the
  sender.
- names - *Object* *optional*. A list of human-readable localised
  *[names](#names)* of the sender.
- contacts - *Array* *optional*. A collection of *[contacts](#contact)*.
  Provides contact information for the party in regard to the transmission of
  the message.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    "sender": {
      "id": "ECB",
      "name": "European Central Bank",
      "names": {
        "en": "European Central Bank",
        "fr": "Banque Centrale Européenne"
      },
      "contacts": [
        {
          # contact objects #
        }
      ]
    }
    ```

#### names

*Object* containing all appropriate localised names, one per object property:

- One or more of: IETF Language Tag according to
  [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for
  specifying locals in HTTP - *String*. The localised name.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
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
- names - *Object* *optional*. Human-readable localised *[names](#names)* of the
  contact.
- department - *String* *optional*. Human-readable (best-language-match) name of
  the organisational structure for the contact.
- departments - *Object* *optional*. Human-readable localised *[names](#names)*
  of the organisational structure for the contact.
- role - *String* *optional*. Human-readable (best-language-match) name of the
  responsibility of the contact.
- roles - *Object* *optional*. Human-readable localised *[names](#names)* of the
  responsibility of the contact.
- telephones - *Array* *optional*. An array of telephone numbers for the
  contact.
- faxes - *Array* *optional*. An array of fax numbers for the contact person.
- uris - *Array* *optional*. An array of uris. Each uri holds an information URL
  for the contact.
- emails - *Array* *optional*. An array of email addresses for the contact
  person.
- x400s - *Array* *optional*. An array of X.400 addresses for the contact
  person.

See the section on [localised strings](./4_localised_text_elements.md) on how the
message deals with languages.

??? example

    ```json
    {
      "id": "HOTLINE",
      "name": "Statistics hotline",
      "names": {
        "en": "Statistics hotline",
        "fr": "Service d'assistance téléphonique des Statistiques"
      },
      "department": "Statistics hotline",
      "departments": {
        "en": "Statistics hotline",
        "fr": "Service d'assistance téléphonique des Statistiques"
      },
      "role": "Statistics hotline",
      "roles": {
        "en": "Statistics hotline",
        "fr": "Service d'assistance téléphonique des Statistiques"
      },
      "telephones": [ "+00 0 00 00 00 00" ],
      "faxes": [ "+00 0 00 00 00 01" ],
      "uris": [ "www.xyz.org" ],
      "emails": [ "statistics@xyz.org" ]
    }
    ```

### receiver

*Object* *optional*. Information about the party that is receiving the message.
This can be useful if the WS requires authentication. Receiver contains the same
fields as *[sender](#sender)*.

### meta link

See the section on *[linking mechanism](./2_linking_mechanism.md)* for all information
on links.

## data

*Object* *optional*. Header contains the message's “primary data”.

- metadataSets - *Array* *optional*. This field is an array of
  *[metadataSet](#metadataset)* objects. A metadata set contains a collection of
  reported metadata against a set of values for a given full or partial target
  identifier, as described in a metadata structure definition. The metadata set
  may contain reported metadata for multiple report structures defined in a
  metadata structure definition.

??? example

    ```json
    "data": {
      "metadataSets": [
        {
          # metadataSet object #
        }
      ]
    }
    ```

### metadataSet

*Object*. Contains a collection of reported metadata against a set of values for
a given full or partial target identifier, as described in a metadata structure
definition. The metadata set may contain reported metadata for multiple report
structures defined in a metadata structure definition.

- action - *String* *optional*. Deprecated. Instead, actions are defined by the
  HTTP action verb used. See below for more details.
- isPartialLanguage - *Boolean* *optional*. Default: `false`. Set to `true` if
  the metadata set doesn't contain the complete set of all available languages,
  e.g., when obtained as a response to a GET query that requested specific
  languages through the HTTP header “Accept-Language”.
- publicationPeriod - *String* *optional*. The publicationPeriod specifies the
  period of publication of the data in terms of whatever provisioning agreements
  might be in force (i.e., "2005-Q1" if that is the time of publication for a
  `metadataSet` published on a quarterly basis).
- publicationYear - *String* *optional*. The publicationYear holds the ISO 8601
  four-digit year.
- reportingBegin - *String* *optional*. The start of the time period covered by
  the message.
- reportingEnd - *String* *optional*. The end of the time period covered by the
  message.
- id - *String*. Identifier for the metadata set.
- agencyID - *String*. ID of the agency maintaining this metadata set.
- version - *String* *optional*. Version of this metadata set according to
  semantic versioning. If not specified then the metadata set is non-versioned.
- isExternalReference - *Boolean* *optional*. If set to “true” it indicates that
  the content of the metadata set is held externally.
- metadataflow - *String*. URN reference to the metadataflow definition (either
  *metadataflow* or *metadataProvisionAgreement* is required).
- metadataProvisionAgreement - *String*. URN reference to the metadata provision
  definition (either *metadataflow* or *metadataProvisionAgreement* is
  required).
- validFrom - *String* *optional*. The validFrom indicates the inclusive start
  time indicating the validity of the information in the data.
- validTo - *String* *optional*. The validTo indicates the inclusive end time
  indicating the validity of the information in the data.
- annotations - *Array* *optional*. *[Annotations](#annotation)* is a collection
  of indices of the corresponding *annotations* for the metadata set.
- links - *Array* *optional*. *Links* field is an array of *[link](#meta-link)*
  objects. If appropriate, a collection of links to additional information
  regarding the metadata set.
- name - *String*. Human-readable (best-language-match) name of the metadata
  set.
- names - *Object* *optional*. Human-readable localised *[names](#names)* of the
  metadata set.
- description - *String* *optional*. Human-readable (best-language-match)
  description of the metadata set.
- descriptions - *Object* *optional*. Human-readable localised descriptions (see
  *[names](#names)*) of the metadata set.
- targets - Non-empty *array* of *String*. Each *target* holds a valid SDMX
  Registry URN (see SDMX Registry Specification for details) of the object to
  which the reported metadata apply. The same metadata set can be linked to
  multiple targets.
- attributes - Non-empty *array* of recursive *[attribute](#attribute)* objects.
  Contains the reported metadata attribute values for the reported metadata and
  recursively their child metadata attributes.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

Details for handling of actions:

Reference metadatasets are maintainable and thus for actions behave like
structural metadata (artefacts): When interacting with SDMX Rest web services,
the HTTP action verbs GET, PUT and POST are used to indicate the intended action
per web request. Consequently, different actions cannot be bundled and executed
with "transactional ACIDity". Note that metadatasets retrieved using the HTTP
header `Accept-Language` may contain only partial languages, and thus should be
marked with its `isPartialLanguage` property set to `true`. Submitting such a
partial metadataset to update an SDMX storage system will only add or update the
included languages but not change other languages.  
The former message header or metadataset property `DataSetAction` is deprecated.
To avoid conflicts, it is now ignored if still present.

??? example

    ```json
    {
      "isPartialLanguage": true,
      "publicationPeriod": "2018-Q1",
      "publicationYear": "2018",
      "reportingBegin": "1960",
      "reportingEnd": "2020",
      "id": "METADATASET",
      "agencyID": "ECB.DISS",
      "version": "1.0",
      "isExternalReference": false,
      "metadataflow": "urn:sdmx:org.sdmx.infomodel.metadatastructure.Metadataflow=ECB.DISS:MDF(1.0)",
      # "metadataProvisionAgreement": "urn:sdmx:org.sdmx.infomodel.registry.MetadataProvisionAgreement=ECB.DISS:MDPA(1.0.0)",
      "validFrom": "2018-04-01",
      "validTo": "2018-07-01",
      "annotations": [
        {
          # annotation object #
        }
      ],
      "links": [
        {
          # link object #
        }
      ],
      "name": "Metadata set",
      "names": {
        "en": "Metadata set",
        "fr": "Set de métadonnées"
      },
      "description": "This is the description of the metadata set",
      "descriptions": {
        "en": "This is the description of the metadata set",
        "fr": "Ceci est la description de l'ensemble des métadonnées"
      },
      "targets": ["urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)"],
      "attributes": [
        {
          # attribute object #
        }
      ]
    }
    ```

#### attribute

*Object*. Defines the structure for a reported metadata attribute. A value for
the attribute can be supplied as either a single value, or multi-lingual text
values (either structured or unstructured). An optional set of child metadata
attributes is also available if the metadata attribute definition defines nested
metadata attributes.

- id - *String*. ID for the reported metadata attribute.
- annotations - *Array* *optional*. *[Annotations](#annotation)* is a collection
  of indices of the corresponding *annotations* for the reported metadata
  attribute.
- format - *Object* *optional*. *[Format](#format)* describes the allowed
  metadata attribute representation. It is only used when the metadata
  attributes are not defined by an enumerated list of identifiable items
  (codelist).
- value - *String*, *Number*, *Integer*, *Boolean* or localised *String* (see
  *[names](#names)*) *object*, *optional*. Value for the reported metadata
  attribute. Also HTML strings are supported.
- attributes - Non-empty *array* of recursive *[attribute](#attribute)* objects.
  Contains the reported metadata attribute values for the reported metadata and
  recursively their child metadata attributes.

One of the properties is required: value, values, text or structuredText.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    {
      "id": "REPORT_ATTRIBUTE1",
      "format": {
        # format object #
      },
      "value": "CODED_TEXT",
      "annotations": [
        {
          # annotation object #
        }
      ],
      "attributes": [
        {
          # attribute object #
        }
      ]
    }

      "value": "<p>An XHTML text</p>"

      "value": {
        "en": "<p>An XHTML text</p>"
      }
    ```

##### format

*Object*. The format object defines the representation for a component. It
describes the possible content for component values, which could be text
(including XHTML and multi-lingual values).

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
  "Duration", "GeospatialInformation" and "XHTML". The default data type is
  "String".
- isSequence - *Boolean* *optional*. Indicates whether the values are intended
  to be ordered, and it may work in combination with the interval, startValue,
  and endValue attributes or the timeInterval, startTime, and endTime,
  attributes. If this attribute holds a value of 'true', a start value or time
  and a numeric or time interval must supplied. If an end value is not given,
  then the sequence continues indefinitely.
- interval - *Integer* *optional*. Specifies the permitted interval (increment)
  in a sequence. In order for this to be used, the isSequence attribute must
  have a value of 'true'.
- startValue - *Number* *optional*. Is used in conjunction with the isSequence
  and interval attributes (which must be set in order to use this attribute).
  This attribute is used for a numeric sequence, and indicates the starting
  point of the sequence. This value is mandatory for a numeric sequence to be
  expressed.
- endValue - *Number* *optional*. Is used in conjunction with the isSequence and
  interval attributes (which must be set in order to use this attribute). This
  attribute is used for a numeric sequence, and indicates that ending point (if
  any) of the sequence.
- timeInterval - *String* *optional*. Indicates the permitted duration in a time
  sequence. It complies with the time duration specification of the ISO 8601
  standard. In order for this to be used, the isSequence attribute must have a
  value of 'true'.
- startTime - *String* *optional*. Is used in conjunction with the isSequence
  and timeInterval attributes (which must be set in order to use this
  attribute). This attribute is used for a time sequence, and indicates the
  start time of the sequence. This value is mandatory for a time sequence to be
  expressed. It must be a valid standard time period (gYear, gYearMonth, date,
  dateTime and SDMX time periods).
- endTime - *String* *optional*. Is used in conjunction with the isSequence and
  timeInterval attributes (which must be set in order to use this attribute).
  This attribute is used for a time sequence, and indicates that ending point
  (if any) of the sequence. It must be a valid standard time period (gYear,
  gYearMonth, date, dateTime and SDMX time periods).
- minLength - *Positive integer* *optional*. Specifies the minimum length of the
  value in characters.
- maxLength - *Positive integer* *optional*. Specifies the maximum length of the
  value in characters.
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
- isMultiLingual - *Boolean* *optional*. This indicates for a text format of
  type "String" or "XHTML", whether the uncoded component value should allow for
  multiple values in different languages. The default is `false`.
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

- value - *Number* or *String* *optional*. The sentinel value (within the value
  domain of the data format) being described.
- name - *String* *optional*. Human-readable (best-language-match) name (or
  meaning) of the sentinel value.
- names - *Object* *optional*. Human-readable localised *[names](#names)* (or
  meanings) of the sentinel value.
- description - *String* *optional*. Human-readable (best-language-match)
  description for the sentinel value.
- descriptions - *Object* *optional*. Human-readable localised descriptions (see
  *[names](#names)*) for the sentinel value.

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

#### annotation

*Object* *optional*. Provides all information about an annotation.

- id - *String* *optional*. ID provides a non-standard identification of an
  annotation. It can be used to disambiguate annotations.
- title - *String* *optional*. Provides a non-localised title for the
  annotation.
- type - *String* *optional*. Type is used to distinguish between annotations
  designed to support various uses. The types are not enumerated, and these can
  be freely specified by the creator of the annotations. The definitions and use
  of annotation types should be documented by their creator.
- value - *String* *optional*. Provides a non-localised value text for the
  annotation.
- text - *String* *optional*. A human-readable (best-language-match) text of the
  annotation.
- texts - *Object* *optional*. A list of human-readable localised texts (see
  *[names](#names)*) of the annotation.
- links - *Array* *optional*. *Links* field is an array of *[link](#meta-link)*
  objects. If appropriate, a link to an additional external resource which may
  contain or supplement the annotation.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    {
      "id": "74747",
      "title": "Sample annotation",
      "type": "reference",
      "value": "123456",
      "text": "Sample annotation text",
      "texts": {
        "en": "Sample annotation text",
        "fr": "Exemple de texte d'annotation"
      },
      "links": [
        {
          # link object #
        }
      ]
    }
    ```

#### metadataSet link

See the section on [linking mechanism](./2_linking_mechanism.md) for all information
on links. Providing links allowing accessing the underlying SDMX Data Structure
Definition, Dataflow and/or Provision Agreements is recommended.

## statusInformation

*Object* *optional*. Used to provide status information with more detail to the
HTTP status codes in the RESTful SDMX web service responses. The following
pieces of information should be provided:

- code - *Number*. Provides an HTTP status code number for the status
  information if appropriate. See
  [RESTful SDMX web service error handling and status information](https://github.com/sdmx-twg/sdmx-rest/blob/master/doc/status.md).
- title - *String* *optional*. A short, human-readable (best-language-match)
  summary of the situation that SHOULD NOT change from occurrence to occurrence
  of the status, except for purposes of localization.
- titles - *Object* *optional*. A list of short, human-readable localised
  summaries (see *[names](#names)*) of the status that SHOULD NOT change from
  occurrence to occurrence of the status, except for purposes of localization.
- detail - *String* *optional*. A human-readable (best-language-match)
  explanation specific to this occurrence of the status. Like title, this
  field’s value can be localized. It is fully customizable by the service
  providers and should provide enough detail to ease understanding the reasons
  of the status.
- details - *Object* *optional*. A list of human-readable localised explanations
  (see *[names](#names)*) specific to this occurrence of the status. Like
  titles, this field’s value can be localized. It is fully customizable by the
  service providers and should provide enough detail to ease understanding the
  reasons of the status.
- links - *Array* *optional*. *Links* field is an array of *[link](#meta-link)*
  objects. If appropriate, a collection of links to additional external
  resources for the status information.

See the section on [localised text elements](./4_localised_text_elements.md) on how the
message deals with languages.

??? example

    ```json
    {
      "code": 400,
      "title": "Invalid parameter",
      "titles": {
        "en": "Invalid parameter",
        "fr": "Paramètre invalide"
      }
    }
    ```
