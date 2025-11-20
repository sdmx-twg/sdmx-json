# Field Guide to SDMX-JSON 2.0 Metadata Message Objects (aligned with SDMX 3.0.0)

## message

Message is the top level object and it contains the requested information
(referential metadata) as well as the meta-information describing the (technical)
context of the message and, possibly, status information.

- meta - _Object_ _optional_. A _[meta](#meta)_ object that contains
  non-standard meta-information and basic technical information about the
  message, such as when it was prepared and who has sent it.
- data - _Object_ _optional_. _[Data](#data)_ contains the message's “primary
  data”.
- errors - _Array_ _optional_. _Errors_ field is an array of
  _[statusMessage](#statusmessage)_ objects. When appropriate provides a list of
  status messages in addition to RESTful web services HTTP error status codes.

The members data and status CAN coexist in the same message.

??? example

    ```json
    {
      "meta": {
        # meta object #
      },
      "data": {
        # data object #
      },
      "errors": [
        {
          # statusMessage object #
        }
      ]
    }
    ```

## meta

_Object_ _optional_. Used to include non-standard meta-information and basic
technical information about the message, such as when it was prepared and who
has sent it. Any members MAY be specified within `meta` objects.

- schema - _String_ _optional_. Contains the URL to the schema allowing to
  validate the message. This also allows identifying the version of SDMX-JSON
  format used in this message. **Providing the link to the SDMX-JSON schema is
  recommended.**
- id - _String_. Unique string that identifies the message for further
  references.
- test - _Boolean_ _optional_. Indicates whether the message is for test
  purposes or not. False for normal messages.
- prepared - _String_. A timestamp indicating when the message was prepared.
  Values must follow the ISO 8601 syntax for combined dates and times, including
  time zone.
- contentLanguages - _Array_ _optional_. Array of strings containing the
  identifier of all languages used anywhere in the message for localized
  elements, and thus the languages of the intended audience, representing in an
  array format the same information than the http Content-Language response
  header, e.g. "en, fr-fr". See
  [IETF Language Tags](https://tools.ietf.org/html/rfc5646#section-2.1).
  The array's first element
  indicates the main language used in the message for localized elements. **The
  usage of this property is recommended.**
- name - _String_ _optional_. Human-readable (best-language-match) name for the
  transmission.
- names - _Object_ _optional_. Human-readable localised _[names](#names)_ for
  the transmission.
- sender - _Object_. _[Sender](#sender)_ contains information about the party
  that is transmitting the message.
- receivers - _Array_ _optional_ of _[Receiver](#receiver)_ objects thats
  contain information about the party that is receiving the message. This can be
  useful if the WS requires authentication.
- links - _Array_ _optional_. _Links_ field is an array of _[link](#meta-link)_
  objects. If appropriate, a collection of links to additional external
  resources for the header.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    "meta": {
      "schema": "https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/metadata-message/tools/schemas/sdmx-json-metadata-schema.json",
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

_Object_. Information about the party that is transmitting the message. Sender
contains the following fields:

- id - _String_. A unique identifier of the party.
- name - _String_ _nullable_. A human-readable (best-language-match) name of the
  sender.
- names - _Object_ _optional_. A list of human-readable localised
  _[names](#names)_ of the sender.
- contacts - _Array_ _optional_. A collection of _[contacts](#contact)_.
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

_Object_ containing all appropriate localised names, one per object property:

- One or more of: IETF Language Tag according to
  [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for
  specifying locals in HTTP - _String_. The localised name.

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

_Object_. A collection of contact details. Each object in the collection may
contain the following field:

- id - _String_. Identifier for the resource.
- name - _String_ _optional_. Human-readable (best-language-match) name of the
  contact.
- names - _Object_ _optional_. Human-readable localised _[names](#names)_ of the
  contact.
- department - _String_ _optional_. Human-readable (best-language-match) name of
  the organisational structure for the contact.
- departments - _Object_ _optional_. Human-readable localised _[names](#names)_
  of the organisational structure for the contact.
- role - _String_ _optional_. Human-readable (best-language-match) name of the
  responsibility of the contact.
- roles - _Object_ _optional_. Human-readable localised _[names](#names)_ of the
  responsibility of the contact.
- telephones - _Array_ _optional_. An array of telephone numbers for the
  contact.
- faxes - _Array_ _optional_. An array of fax numbers for the contact person.
- uris - _Array_ _optional_. An array of uris. Each uri holds an information URL
  for the contact.
- emails - _Array_ _optional_. An array of email addresses for the contact
  person.
- x400s - _Array_ _optional_. An array of X.400 addresses for the contact
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

_Object_ _optional_. Information about the party that is receiving the message.
This can be useful if the WS requires authentication. Receiver contains the same
fields as _[sender](#sender)_.

### meta link

See the section on _[linking mechanism](./2_linking_mechanism.md)_ for all information
on links.

## data

_Object_ _optional_. Header contains the message's “primary data”.

- metadataSets - _Array_ _optional_. This field is an array of
  _[metadataSet](#metadataset)_ objects. A metadata set contains a collection of
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

_Object_. Contains a collection of reported metadata against a set of values for
a given full or partial target identifier, as described in a metadata structure
definition. The metadata set may contain reported metadata for multiple report
structures defined in a metadata structure definition.

- action - _String_ _optional_. Action provides a list of actions, describing
  the intention of the data transmission from the sender's side.
    - `Append` - this is an incremental update for an existing `dataSet` or the
        provision of new data or documentation (attribute values) formerly absent.
        If any of the supplied data or metadata is already present, it will not
        replace these data.
    - `Replace` - data are to be replaced, and may also include additional data to
        be appended.
    - `Delete` - data are to be deleted.
    - `Information` (default) - data are being exchanged for informational
        purposes only, and not meant to update a system.
- publicationPeriod - _String_ _optional_. The publicationPeriod specifies the
  period of publication of the data in terms of whatever provisioning agreements
  might be in force (i.e., "2005-Q1" if that is the time of publication for a
  `metadataSet` published on a quarterly basis).
- publicationYear - _String_ _optional_. The publicationYear holds the ISO 8601
  four-digit year.
- reportingBegin - _String_ _optional_. The start of the time period covered by
  the message.
- reportingEnd - _String_ _optional_. The end of the time period covered by the
  message.
- id - _String_. Identifier for the metadata set.
- agencyID - _String_. ID of the agency maintaining this metadata set.
- version - _String_ _optional_. Version of this metadata set according to
  semantic versioning. If not specified then the metadata set is non-versioned.
- isExternalReference - _Boolean_ _optional_. If set to “true” it indicates that
  the content of the metadata set is held externally.
- metadataflow - _String_. URN reference to the metadataflow definition (either
  _metadataflow_ or _metadataProvisionAgreement_ is required).
- metadataProvisionAgreement - _String_. URN reference to the metadata provision
  definition (either _metadataflow_ or _metadataProvisionAgreement_ is
  required).
- validFrom - _String_ _optional_. The validFrom indicates the inclusive start
  time indicating the validity of the information in the data.
- validTo - _String_ _optional_. The validTo indicates the inclusive end time
  indicating the validity of the information in the data.
- annotations - _Array_ _optional_. _[Annotations](#annotation)_ is a collection
  of indices of the corresponding _annotations_ for the metadata set.
- links - _Array_ _optional_. _Links_ field is an array of _[link](#meta-link)_
  objects. If appropriate, a collection of links to additional information
  regarding the metadata set.
- name - _String_. Human-readable (best-language-match) name of the metadata
  set.
- names - _Object_ _optional_. Human-readable localised _[names](#names)_ of the
  metadata set.
- description - _String_ _optional_. Human-readable (best-language-match)
  description of the metadata set.
- descriptions - _Object_ _optional_. Human-readable localised descriptions (see
  _[names](#names)_) of the metadata set.
- targets - Non-empty _array_ of _String_. Each _target_ holds a valid SDMX
  Registry URN (see SDMX Registry Specification for details) of the object to
  which the reported metadata apply. The same metadata set can be linked to
  multiple targets.
- attributes - Non-empty _array_ of recursive _[attribute](#attribute)_ objects.
  Contains the reported metadata attribute values for the reported metadata and
  recursively their child metadata attributes.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    {
      "action": "Information",
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

_Object_. Defines the structure for a reported metadata attribute. A value for
the attribute can be supplied as either a single value, or multi-lingual text
values (either structured or unstructured). An optional set of child metadata
attributes is also available if the metadata attribute definition defines nested
metadata attributes.

- id - _String_. ID for the reported metadata attribute.
- annotations - _Array_ _optional_. _[Annotations](#annotation)_ is a collection
  of indices of the corresponding _annotations_ for the reported metadata
  attribute.
- format - _Object_ _optional_. _[Format](#format)_ describes the allowed
  metadata attribute representation. It is only used when the metadata
  attributes are not defined by an enumerated list of identifiable items
  (codelist).
- value - _String_, _Number_, _Integer_, _Boolean_ or localised _String_ (see
  _[names](#names)_) _object_, _optional_. Value for the reported metadata
  attribute. Also HTML strings are supported.
- attributes - Non-empty _array_ of recursive _[attribute](#attribute)_ objects.
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

_Object_. The format object defines the representation for a component. It
describes the possible content for component values, which could be text
(including XHTML and multi-lingual values).

- dataType - _String_ _optional_. Describes the type of data format allowed for
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
- isSequence - _Boolean_ _optional_. Indicates whether the values are intended
  to be ordered, and it may work in combination with the interval, startValue,
  and endValue attributes or the timeInterval, startTime, and endTime,
  attributes. If this attribute holds a value of 'true', a start value or time
  and a numeric or time interval must supplied. If an end value is not given,
  then the sequence continues indefinitely.
- interval - _Integer_ _optional_. Specifies the permitted interval (increment)
  in a sequence. In order for this to be used, the isSequence attribute must
  have a value of 'true'.
- startValue - _Number_ _optional_. Is used in conjunction with the isSequence
  and interval attributes (which must be set in order to use this attribute).
  This attribute is used for a numeric sequence, and indicates the starting
  point of the sequence. This value is mandatory for a numeric sequence to be
  expressed.
- endValue - _Number_ _optional_. Is used in conjunction with the isSequence and
  interval attributes (which must be set in order to use this attribute). This
  attribute is used for a numeric sequence, and indicates that ending point (if
  any) of the sequence.
- timeInterval - _String_ _optional_. Indicates the permitted duration in a time
  sequence. It complies with the time duration specification of the ISO 8601
  standard. In order for this to be used, the isSequence attribute must have a
  value of 'true'.
- startTime - _String_ _optional_. Is used in conjunction with the isSequence
  and timeInterval attributes (which must be set in order to use this
  attribute). This attribute is used for a time sequence, and indicates the
  start time of the sequence. This value is mandatory for a time sequence to be
  expressed. It must be a valid standard time period (gYear, gYearMonth, date,
  dateTime and SDMX time periods).
- endTime - _String_ _optional_. Is used in conjunction with the isSequence and
  timeInterval attributes (which must be set in order to use this attribute).
  This attribute is used for a time sequence, and indicates that ending point
  (if any) of the sequence. It must be a valid standard time period (gYear,
  gYearMonth, date, dateTime and SDMX time periods).
- minLength - _Positive integer_ _optional_. Specifies the minimum length of the
  value in characters.
- maxLength - _Positive integer_ _optional_. Specifies the maximum length of the
  value in characters.
- minValue - _Number_ _optional_. Is used for inclusive and exclusive ranges,
  indicating what the lower bound of the range is. If this is used with an
  inclusive range, a valid value will be greater than or equal to the value
  specified here. By default, the minValue is assumed to be inclusive.
- maxValue - _Number_ _optional_. Is used for inclusive and exclusive ranges,
  indicating what the upper bound of the range is. If this is used with an
  inclusive range, a valid value will be less than or equal to the value
  specified here. By default, the maxValue is assumed to be inclusive.
- decimals - _Positive integer_ _optional_. Indicates the number of characters
  allowed after the decimal separator.
- pattern - _String_ _optional_. Holds any standard regular expression.
- isMultiLingual - _Boolean_ _optional_. This indicates for a text format of
  type "String" or "XHTML", whether the uncoded component value should allow for
  multiple values in different languages. The default is `false`.
- sentinelValues - _Array_ of *Object*s _optional_. When present, the
  sentinelValues array indicates that sentinel values are defined for the data
  format. Each _[sentinelValue](#sentinelvalue)_ object indicates a reserved
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

_Object_. It defines a reserved value (within the value domain of the data
format) along with its meaning.

- value - _Number_ or _String_ _optional_. The sentinel value (within the value
  domain of the data format) being described.
- name - _String_ _optional_. Human-readable (best-language-match) name (or
  meaning) of the sentinel value.
- names - _Object_ _optional_. Human-readable localised _[names](#names)_ (or
  meanings) of the sentinel value.
- description - _String_ _optional_. Human-readable (best-language-match)
  description for the sentinel value.
- descriptions - _Object_ _optional_. Human-readable localised descriptions (see
  _[names](#names)_) for the sentinel value.

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

_Object_ _optional_. Provides all information about an annotation.

- id - _String_ _optional_. ID provides a non-standard identification of an
  annotation. It can be used to disambiguate annotations.
- title - _String_ _optional_. Provides a non-localised title for the
  annotation.
- type - _String_ _optional_. Type is used to distinguish between annotations
  designed to support various uses. The types are not enumerated, and these can
  be freely specified by the creator of the annotations. The definitions and use
  of annotation types should be documented by their creator.
- value - _String_ _optional_. Provides a non-localised value text for the
  annotation.
- text - _String_ _optional_. A human-readable (best-language-match) text of the
  annotation.
- texts - _Object_ _optional_. A list of human-readable localised texts (see
  _[names](#names)_) of the annotation.
- links - _Array_ _optional_. _Links_ field is an array of _[link](#meta-link)_
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

## statusMessage

_Object_ _optional_. Used to provide status messages in addition to RESTful web
services HTTP error status codes. The following pieces of information should be
provided:

- code - _Number_. Provides a code number for the status message if appropriate.
  Standard code numbers are defined in the SDMX 2.1 Web Services Guidelines.
- title - _String_ _optional_. A short, human-readable (best-language-match)
  summary of the situation that SHOULD NOT change from occurrence to occurrence
  of the status, except for purposes of localization.
- titles - _Object_ _optional_. A list of short, human-readable localised
  summaries (see _[names](#names)_) of the status that SHOULD NOT change from
  occurrence to occurrence of the status, except for purposes of localization.
- detail - _String_ _optional_. A human-readable (best-language-match)
  explanation specific to this occurrence of the status. Like title, this
  field’s value can be localized. It is fully customizable by the service
  providers and should provide enough detail to ease understanding the reasons
  of the status.
- details - _Object_ _optional_. A list of human-readable localised explanations
  (see _[names](#names)_) specific to this occurrence of the status. Like
  titles, this field’s value can be localized. It is fully customizable by the
  service providers and should provide enough detail to ease understanding the
  reasons of the status.
- links - _Array_ _optional_. _Links_ field is an array of _[link](#meta-link)_
  objects. If appropriate, a collection of links to additional external
  resources for the status message.

See the section on [localised strings](./4_localised_text_elements.md) on how the
message deals with languages.

??? example

    ```json
    {
      "code": 150,
      "title": "Invalid parameter",
      "titles": {
        "en": "Invalid parameter",
        "fr": "Paramètre invalide"
      }
    }
    ```
