# Field Guide to SDMX-JSON Structure Message 2.0.0 Objects (aligned with SDMX 3.0.0)

## message

Message is the top level object and it contains the requested information
("data") as well as the meta-information describing the (technical) context of
the message and, possibly, error information.

- meta - _Object_ _optional_. A _[meta](#meta)_ object that contains
  non-standard meta-information and basic technical information about the
  message, such as when it was prepared and who has sent it.
- data - _Object_ _optional_. _[Data](#data)_ contains the message's “primary
  data”.
- errors - _Array_ _optional_. _Errors_ field is an array of _[error](#error)_
  objects. When appropriate provides a list of error messages in addition to
  RESTful web services HTTP error status codes.

The members data and errors MUST NOT coexist in the same message.

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
                # error object #
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
- receiver - _Object_ _optional_. _[Receiver](#receiver)_ contains information
  about the party that is receiving the message. This can be useful if the WS
  requires authentication.
- links - _Array_ _optional_. _Links_ field is an array of _[link objects](#link)_.
  If appropriate, a collection of links to additional external
  resources for the header.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    "meta": {
        "schema": "https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/data-message/tools/schemas/sdmx-json-data-schema.json",
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
        "receiver": {
            # receiver object #
        },
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
            # name object #
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

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

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
      "telephones": ["+00 0 00 00 00 00"],
      "faxes": ["+00 0 00 00 00 01"],
      "uris": ["www.xyz.org"],
      "emails": ["statistics@xyz.org"]
    }
    ```

### receiver

_Object_ _optional_. Information about the party that is receiving the message.
This can be useful if the WS requires authentication. Receiver contains the same
fields as [sender](#sender).

### link

See the section on [linking mechanism](./2_linking_mechanism.md) for all information
on links.

## data

_Object_ _optional_. Header contains the message's “primary data”.

- _[Artefact type]_ - _Array_ _optional_. This field is an array of objects of
  one of the corresponding SDMX Information Model artefact types:
  _dataStructure_, _metadataStructure_, _categoryScheme_, _conceptScheme_,
  _codelist_, _geographicCodelists_, _geoGridCodelists_, _valueLists_,
  _hierarchy_, _hierarchyAssociation_, _agencyScheme_, _dataProviderScheme_,
  _dataProviderScheme_, _metadataProviderScheme_, _organisationUnitScheme_,
  _dataflow_, _metadataflow_, _reportingTaxonomy_, _provisionAgreement_,
  _metadataProvisionAgreement_, _structureMap_, _representationMap_,
  _conceptSchemeMap_, _categorySchemeMap_, _organisationSchemeMap_,
  _reportingTaxonomyMap_, _process_, _categorisation_, _dataConstraint_,
  _metadataConstraint_, _customTypeScheme_, _vtlMappingScheme_,
  _namePersonalisationScheme_, _rulesetScheme_, _transformationScheme_ and
  _userDefinedOperatorScheme_. Each of the corresponding object properties is
  allowed at maximum one time. Contains the requested structural information
  according to the definition of this artefact. For more information, please
  see:
    - _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_
    - _[Common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_

    - _[dataStructures](#datastructure)_
    - _[metadataStructures](#metadatastructure)_
    - _[categorySchemes](#categoryscheme)_
    - _[conceptSchemes](#conceptscheme)_
    - _[codelists](#codelist)_
    - _[geographicCodelists](#geographiccodelist)_
    - _[geoGridCodelists](#geogridcodelist)_
    - _[valueLists](#valuelist)_
    - _[hierarchies](#hierarchy)_
    - _[hierarchyAssociations](#hierarchyassociation)_
    - _[agencySchemes](#agencyscheme)_
    - _[dataProviderSchemes](#dataproviderscheme)_
    - _[dataConsumerSchemes](#dataconsumerscheme)_
    - _[metadataProviderSchemes](#metadataproviderscheme)_
    - _[organisationUnitSchemes](#organisationunitscheme)_
    - _[dataflows](#dataflow)_
    - _[metadataflows](#metadataflow)_
    - _[reportingTaxonomies](#reportingtaxonomy)_
    - _[provisionAgreements](#provisionagreement)_
    - _[metadataProvisionAgreements](#metadataprovisionagreement)_
    - _[structureMaps](#structuremap)_
    - _[representationMaps](#representationmap)_
    - _[conceptSchemeMaps](#conceptschememap)_
    - _[categorySchemeMaps](#categoryschememap)_
    - _[organisationSchemeMaps](#organisationschememap)_
    - _[reportingTaxonomyMaps](#reportingtaxonomymap)_
    - _[processes](#process)_
    - _[categorisations](#categorisation)_
    - _[dataConstraints](#dataconstraint)_
    - _[metadataConstraints](#metadataconstraint)_
    - _[customTypeSchemes](#customtypescheme)_
    - _[vtlMappingSchemes](#vtlmappingscheme)_
    - _[namePersonalisationSchemes](#namepersonalisationscheme)_
    - _[rulesetSchemes](#rulesetscheme)_
    - _[transformationSchemes](#transformationscheme)_
    - _[userDefinedOperatorSchemes](#userdefinedoperatorscheme)_

??? example

    ```json
    "data": {
        "dataStructures": [
            {
                # dataStructureDefinition object #
            }
        ],
        "dataflows": [
            {
                # dataflow object #
            }
        ]
    }
    ```

### Common SDMX artefact properties

All SDMX artefact types share the following common object properties:

- id - _String_. Identifier for the resource.
- agencyID - _String_ _optional_. ID of the agency maintaining this resource.
- version - _String_ _optional_. Version of this resource. Can be a legacy
  version with two version parts (e.g. '1.0') for fully mutable artefacts, or a
  semantic version according to SDMX versioning rules with 3 parts (e.g.
  '1.0.0') for immutable artefacts or with 4 parts (e.g. '1.0.0-draft') for
  artefacts allowed to change within a certain scope.
- name - _String_ _optional_. Human-readable (best-language-match) name of the
  resource.
- names - _Object_ _optional_. Human-readable localised _[names](#names)_ of the
  resource.
- description - _String_ _optional_. Human-readable (best-language-match)
  description of the resource.
- descriptions - _Object_ _optional_. Human-readable localised descriptions (see
  _[names](#names)_) of the resource.
- validFrom - _String_ _optional_. A timestamp from which the version is valid.
  Values must follow the ISO 8601 syntax for combined dates and times, including
  time zone.
- validTo - _String_ _optional_. A timestamp from which the version is
  superseded. Values must follow the ISO 8601 syntax for combined dates and
  times, including time zone.
- isExternalReference - _Boolean_ _optional_. If set to “true” it indicates that
  the content of the resource is held externally.
- annotations - _Array_ _optional_. Provides a list of annotation objects.
- links - _Array_ _optional_. A collection of links to additional resources for
  the resource. See the section _[link](#link)_. **It is recommended to
  systematically include as the first link a self-referencing hyperlink (link
  with "rel"="self") to indicate the URL address of the resource, and its URN.**

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    {
        "id": "MOBILE_NAVI_PUB",
        "agencyID": "ECB.DISS",
        "version": "1.0.0",
        "name": "Economic concepts",
        "names": {
            "en": "Economic concepts",
            "fr": "Concepts économiques"
        },
        "description": "This is the description of Economic concepts",
        "descriptions": {
            "en": "This is the description of Economic concepts",
            "fr": "Ceci est la description des concepts économiques"
        },
        "validFrom": "2012-05-04",
        "validTo": "2015-05-04",
        "isExternalReference": false,
        "annotations":[
            {
                # annotation object#
            }
        ],
        "links": [
            {
                # link object#
            }
        ]
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
- links - _Array_ _optional_. _Links_ field is an array of _[link](#link)_
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

### Common properties of SDMX artefacts of base type "ItemScheme"

All SDMX artefacts of base type "ItemScheme" (CategoryScheme, ConceptScheme,
Codelist, GeographicCodelist, GeoGridCodelist, AgencyScheme, DataProviderScheme,
MetadataProviderSchemes, DataConsumerScheme, OrganisationUnitScheme,
ReportingTaxonomy, CustomTypeScheme, VtlMappingScheme,
NamePersonalisationScheme, RulesetScheme, UserDefinedOperatorScheme) share the
_[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

In addition, they share the following common object properties:

- isPartial - _Boolean_ _optional_. If set to true, it indicates that the
  resource contains only a sub-set of items.
- categories / concepts / codes / geoFeatureSetCodes / geoGridCodes / agencies /
  dataProviders / dataConsumers / metadataProviders / organisationUnits /
  reportingCategories / customTypes / vtlMappings / namePersonalisations /
  rulesets / transformations / userDefinedOperators -
  _Array_ _optional_. Provides a list of _[items](#item)_ if the resource
  inherits from the ItemScheme. **Note that the order of items is significant.
  In the use case of a submission of a partial list is is necessary to include
  preceding and succeeding items to allow determining the correct positioning
  of the submitted items.**

??? example

    ```json
    {
        "id": "MY_DOMAINS",
        "isPartial": false,
        "categories": [
            {
                # item object #
            }
        ]
    }
    ```

#### item

_Object_ _optional_. Abstract generic item within the ItemScheme (if the resource
is a CategoryScheme, ConceptScheme, Codelist, GeographicCodelist,
GeoGridCodelist, AgencyScheme, DataProviderScheme, MetadataProviderSchemes,
DataConsumerScheme, OrganisationUnitScheme, ReportingTaxonomy, CustomTypeScheme,
VtlMappingScheme, NamePersonalisationScheme, RulesetScheme or
UserDefinedOperatorScheme).

- id - _String_. Identifier for the item.
- name - _String_ _optional_. Human-readable (best-language-match) name of the
  item.
- names - _Object_ _optional_. Human-readable localised _[names](#names)_ of the
  item.
- description - _String_ _optional_. Human-readable (best-language-match)
  description of the item. The description is typically longer than the text
  provided for the name field.
- descriptions - _Object_ _optional_. Human-readable localised descriptions (see
  _[names](#names)_) of the item. A descriptions is typically longer than the
  text provided for the name field.
- parent - _String_ _optional_. Contains the ID or the URN for the parent of the
  item (which is itself an item) enabling the reconstruction of the ordered item
  hierarchy. Prohibited in certain item scheme types.
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources for
  the item. See the section [link](#link).
- categories / concepts / codes / geoFeatureSetCodes / geoGridCodes / agencies /
  dataProviders / dataConsumers / metadataProviders / organisationUnits /
  reportingCategories / customTypes / vtlMappings / namePersonalisations /
  rulesets / transformations / userDefinedOperators -
  _Array_ _optional_. Provides a list of child items of the item. **Note that
  the order of items is significant. In the use case of a submission of a
  partial list is is necessary to include preceding and succeeding items to
  allow determining the correct positioning of the submitted items.**

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    {
        "id": "01",
        "name": "Population and migration",
        "names": {
            "en": "Population and migration",
            "fr": "Population et migration"
        },
        "description": "Description for Population and migration",
        "descriptions": {
            "en": "Description pour Population et migration",
            "fr": "Description for Population and migration"
        },
        "parent": "T",
        "links":[
            {
                # link object#
            }
        ],
        "annotations": [
            {
                # annotation object #
            }
        ],
        "categories": [
            {
                # item object (recursive) #
            }
        ]
    }
    ```

### dataStructure

_Object_. Describes the structure of a data structure definition. A data
structure definition is defined as a collection of metadata concepts, their
structure and usage when used to collect or disseminate data.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

- dataStructureComponents - _Object_ _optional_. The
  _[dataStructureComponents](#datastructurecomponents)_ object defines the
  grouping of the sets of structural metadata concepts that have a defined
  structural role in the data structure definition, like dimensions and time
  dimension, measures, attributes and relationships with external metadata
  attributes. Note that for any component or group defined in a data structure
  definition, its id must be unique. This applies to the identifiers explicitly
  defined by the components as well as those inherited from the concept identity
  of a component. For example, if two dimensions take their identity from
  concepts with same identity (regardless of whether the concepts exist in
  different schemes) one of the dimensions must be provided a different explicit
  identifier. Although there are XML schema constraints to help enforce this,
  these only apply to explicitly assigned identifiers. Identifiers inherited
  from a concept from which a component takes its identity cannot be validated
  against this constraint. Therefore, systems processing data structure
  definitions will have to perform this check outside of the XML validation.
  There are also three reserved identifiers in a data structure definition:
  TIME_PERIOD, REPORTING_PERIOD_START_DAY and REPORTING_PERIOD_END_DAY. These
  identifiers may not be used outside of their respective definitions
  (TimeDimension and Attribute). This applies to both the explicit identifier
  that can be assigned to the components or groups as well as an identifier
  inherited by a component from its concept identity. For example, if an
  ordinary dimension (i.e. not the time dimension) takes its concept identity
  from a concept with the identifier TIME_PERIOD, that dimension must provide a
  different explicit identifier.
- metadata - _String_ _optional_. The URN of a a metadata structure definition.
  A data structure definition may be related to a metadata structure definition
  in order to use its metadata attributes as part of the data. Note that the
  referenced metadata set cannot contain nested metadata attributes, as these
  are not supported in the data. By default all metadata attributes can be
  associated at any level of the data. However, a metadata attribute usage can
  be used to provide a specific attribute relationships for a given metadata
  attribute.

??? example

    ```json
    {
        "id": "DSD1",
        "version": "1.0.0",
        "agencyID": "SDMX",
        "dataStructureComponents": {
            # dataStructureComponents object #
        },
        "metadata": "urn:sdmx:org.sdmx.infomodel.metadatastructuredefinition.MetadataStructureDefinition=OECD:METADATA(1.0.0)"
    }
    ```

#### dataStructureComponents

_Object_ _optional_. DataStructureComponents describes the structure of the
grouping to the sets of structural concepts that have a defined structural role
in the data structure definition. At a minimum at least one dimension must be
defined.

- attributeList - _Object_ _optional_. The _[attributeList](#attributelist)_
  object is a collection of structural concepts that define the attributes of
  the data structure definition. Attributes can relate to one or more measures,
  and be reported at the level of dataflow, several or all dimensions
  (observation).
- dimensionList - _Object_. The _[dimensionList](#dimensionlist)_ object is an
  ordered set of structural concepts that, combined, classify a statistical
  series, such as a time series, and whose values, when combined (the key) in an
  instance such as a data set, uniquely identify a specific series.
- groups - _Array_ _optional_. Array of _[group](#group)_ objects that are sets
  of structural concepts (and possibly their values) that define a partial key
  derived from the key descriptor in a data structure definition.
- measureList - _Object_ _optional_. The _[measureList](#measurelist)_ object is
  a collection of structural concepts that define the measures of the data
  structure definition.

??? example

    ```json
    {
        "attributeList": {
            # attributeList object #
        },
        "dimensionList": {
            # dimensionList object #
        },
        "groups": [
            {
                # group object #
            }
        ],
        "measureList": {
            # measureList object #
        }
    }
    ```

#### attributeList

_Object_ _optional_. AttributeList describes the attributes in the data
structure definition.

- id - _String_. Identifier for the attributeList. It is provided only for
  completeness. However, its value is fixed to "AttributeDescriptor".
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- attributes - _Array_ _optional_. The _[attribute](#attribute)_ object
  describes the definition of a data attribute, which is defined as a
  characteristic of an object or entity. The attribute list may contain the
  specialized data attribute that states the month and day at which the
  reporting year begins or ends, and which provides important context to the
  time dimension when the value of the time dimension is one of the standard
  reporting periods. This attribute provides a reference point from which the
  actual calendar dates covered by these periods can be determined. If this
  attribute does not occur in a data set, then the reporting year start/end day
  will be assumed to be January 1/December 31. This attribute can be recognised
  by its identifier "REPORTING_PERIOD_START_DAY" or "REPORTING_PERIOD_END_DAY".
- metadataAttributeUsages - _Array_ _optional_. The
  _[metadataAttributeUsage](#metadataattributeusage)_ object refines the details
  of how a metadata attribute from the metadata structure referenced from the
  data structure is used. By default, metadata attributes can be expressed at
  any level of the data. This allows an attribute relationship to be defined in
  order restrict the reporting of a metadata attribute to a specific part of the
  data.

??? example

    ```json
    {
        "id": "AttributeDescriptor",
        "attributes": [
            {
                # attribute object #
            }
        ],
        "metadataAttributeUsages": [
            {
                # metadataAttributeUsage object #
            }
        ]
    }
    ```

##### attribute

_Object_ _optional_. Attribute describes the structure of a data attribute,
which is defined as a characteristic of an object or entity. The attribute takes
its semantic, and in some cases it representation, from its concept identity. An
attribute can be coded by referencing a code list from its coded local
representation. It can also specify its data format, which is used as the
representation of the attribute if a coded representation is not defined.
Neither the coded or uncoded representation are necessary, since the attribute
may take these from the referenced concept. An attribute specifies its
relationship with other data structure components and is given an assignment
status. These two properties dictate where in a data message the attribute will
be attached, and whether or not the attribute will be required to be given a
value. A set of roles defined in concept scheme can be assigned to the
attribute.

- id - _String_ _optional_. Identifier for the attribute.
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- usage - Constant _String_ `mandatory` or `optional` _optional_. Indication
  whether reporting a given attribute is mandatory or optional. Default:
  `optional`, except for REPORTING_YEAR_START_DAY and REPORTING_YEAR_END_DAY
  attributes, for which the default is `mandatory`.
- attributeRelationship - _Object_. The
  _[attributeRelationship](#attributerelationship)_ object describes how the
  value of this attribute varies with the values of other components. These
  relationships will be used to determine the attachment level of the attribute
  in the various data formats.
- measureRelationship - _Array_ of *String*s _optional_. The measureRelationship
  array identifies the measures that the attribute applies to. If this is not
  used, the attribute is assumed to apply to all measures. If used, it contains
  one or more identifiers of (a) local measure(s).
- conceptIdentity - _String_. Urn reference to a concept where the
  identification of the concept scheme which defines it is contained in another
  context. The reporting year start and end day attributes take their semantics
  from their respective concept identity (usually the REPORTING_YEAR_START_DAY
  and REPORTING_YEAR_END_DAY concepts), yet always have a fixed identifier
  (REPORTING_YEAR_START_DAY and REPORTING_YEAR_END_DAY).
- conceptRoles - _Array_ of *String*s _optional_. ConceptRole references
  (through URNs) the concepts which define roles which this attribute serves. If
  the concept from which the attribute takes its identity also defines a role
  the concept serves, then the isConceptRole indicator can be set to true on the
  concept identity rather than repeating the reference here.
- localRepresentation - _Object_ _optional_. The
  _[localRepresentation](#localrepresentation)_ object defines the
  representation for the attribute. The representation for the reporting year
  start or end day attribute that has the conceptRole with concept ID
  "REPORTING_PERIOD_START_DAY"/"REPORTING_PERIOD_END_DAY" and states the month
  and day at which the reporting year begins or ends, does not allow for
  enumerated values and its text format is fixed to be a day and month in the
  ISO 8601 format of '--MM-DD'.

??? example

    ```json
    {
        "id": "OBS_STATUS",
        "usage": "optional",
        "attributeRelationship": {
            # attributeRelationship object #
        },
        "measureRelationship": [
            "MEASURE1", "MEASURE2"
        ],
        "conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).OBS_STATUS",
        "conceptRoles": [
            "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).OBS_STATUS"
        ],
        "localRepresentation": {
            # localRepresentation object #
        }
    }
    ```

    Reporting year start day attribute:

    ```json
    {
        "id": "REPORTING_YEAR_START_DAY",
        "usage": "mandatory",
        "attributeRelationship": {
            "dataflow": {}
        },
        "conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).REPORTING_YEAR_START_DAY",
        "conceptRoles": [
            "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).REPORTING_YEAR_START_DAY"
        ],
        "localRepresentation": {
            "format": {
                "dataType": "MonthDay"
            },
            "maxOccurs": 1
        }
    }
    ```json

###### attributeRelationship

_Object_ _optional_. AttributeRelationship defines the structure for stating the
relationship between an attribute and other data structure definition
components.

- dataflow - Empty _Object_ _optional_. This means that the value of the
  attribute **varies** per dataflow. It is the data modeller's responsibility to
  design or use non-overlapping dataflows that do not have observations in
  common, otherwise the integrity of dataflow-specific attribute values is not
  assured by the model, e.g. when querying those data through its DSD. The level
  at which the unique attribute value will be presented in data messages depends
  on the data format and which dimensions are referenced.
- dimensions - _Array_ of *String*s _optional_. One or more identifiers of (a)
  local dimension(s). This is used to reference dimensions in the data structure
  definition with which the value of this attribute may vary. An attribute using
  this relationship can be either a group, series (or section), or observation
  level attribute. The level at which the attribute values will be presented in
  data messages depends on the data format and which dimensions are referenced.
  The array cannot be empty.
- areDimensionsOptional - _Array_ of *Boolean*s _optional_. This is used to
  indicate those dimensions to which the attributes might optionally not be
  attached. The array structure must follow that for the dimensions. The array
  cannot be empty.
- group - _String_ _optional_. Identifier of a local GroupKey Descriptor. This
  is used as a convenience to referencing all of the dimension defined by the
  referenced group. The level at which the attribute values will be presented in
  data messages depends on the data format and which dimensions are referenced.
  If the group (level) is available in the data format used then the attribute
  values should be presented at that group level.
- observation - Empty _Object_ _optional_. This is used to specify that the
  value of the attribute may vary with any of the local dimensions and thus is
  dependent upon the observed value. An attribute with this relationship will
  its values always have presented at observation level.

As with any other attribute, this relationship should be carefully selected also
for the reporting year start or end day attribute as it will determine what type
of data the data structure definition will allow. For example, if an attribute
relationship of `dataflow` is specified, this will mean that the data sets for
this dataflow can only contain data with standard reporting periods where the
all reporting periods have the same start day. In this case, data reported as
standard reporting periods from two entities with different fiscal year start
days could not be contained in the same data set.

??? example

    ```json
    {
        "dataflow": {}
    }

    {
        "dimensions": [
            "FREQ", "CURRENCY"
        ]
        "areDimensionsOptional": [
            false, true
        ]
    }

    {
        "group": "MY_GROUP"
    }

    {
        "observation": {}
    }
    ```

###### localRepresentation

_Object_ _optional_. LocalRepresentation defines the representation for the
attribute. A data attribute can be text (including XHTML and multi-lingual
values), a simple value, or an enumerated value.

- enumeration - _String_ _optional_. Urn reference to an item scheme (such as a
  codelist) or a value list. Dimensions cannot reference a value list.
- enumerationFormat - _Object_ _optional_. The
  _[enumerationFormat](#enumerationformat)_ object defines a restricted version
  of a _[format](#format)_ that only allows facets and text types applicable to
  items (codes) in the referenced scheme. Although the time facets permit any
  value, an actual code identifier does not support the necessary characters for
  time. Therefore these facets should not contain time in their values.
- format - _Object_ _optional_. As an exclusive alternative to an item scheme
  reference the _[format](#format)_ object defines the information for
  describing a full range of data formats and may place restrictions on the
  component's values.
- minOccurs - _Non-negative Integer_ _optional_. Indicates the minimum number of
  values that can be reported for the component. If missing than there is no
  lower limit on its occurrences. The default is 1.
- maxOccurs - _Positive Integer_/_String_ _optional_. Indicates the maximum
  number of values that can be reported for the component. If set to the string
  "unbounded" than there is no upper limit on its occurrences. The default is 1.

The representation for the reporting year start/end day attribute that has the
identifier "REPORTING_PERIOD_START_DAY"/"REPORTING_PERIOD_END_DAY" and states
the month and day at which the reporting year begins/ends, does not allow for
enumerated values and its text format is fixed to be a day and month in the ISO
8601 format of '--MM-DD'. Its _format_ has thus only one single property
_dataType_ fixed to "MonthDay". This type exists solely for the purpose of
fixing the representation of the reporting year start/end day attribute. Its
maxOccurs property is a _Positive Integer_ and is fixed to 1.

??? example

    ```json
    {
        "enumeration": "urn:sdmx:org.sdmx.infomodel.codelist.Codelist=ECB:CL_ORGANISATION(1.0)",
        "enumerationFormat": {
            # enumerationFormat object #
        }
    }

    {
        "format": {
            # format object #
        }
    }

    Local representation of the reporting year start/end day attributes:

    {
        "format": {
            "dataType": "MonthDay"
        },
        "maxOccurs": 1
    }
    ```

##### enumerationFormat

_Object_ _optional_. A restricted version of the _[format](#format)_ that only
allows facets and text types applicable to codes. Although the time facets
permit any value, an actual code identifier does not support the necessary
characters for time. Therefore these facets should not contain time in their
values.

- dataType - _String_ _optional_. Describes the type of data format allowed for
  the representation of the component. Only the following dataTypes are
  supported: "String", "Alpha", "AlphaNumeric", "Numeric", "BigInteger",
  "Integer", "Long", "Short", "Boolean", "URI", "Count", "InclusiveValueRange",
  "ExclusiveValueRange", "Incremental", "ObservationalTimePeriod",
  "StandardTimePeriod", "BasicTimePeriod", "GregorianTimePeriod",
  "GregorianYear", "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod",
  "ReportingYear", "ReportingSemester", "ReportingTrimester",
  "ReportingQuarter", "ReportingMonth", "ReportingWeek", "ReportingDay",
  "Month", "MonthDay", "Day" and "Duration". Time `dimensions` (having the id
  and role "TIME_PERIOD") only support the types "ObservationalTimePeriod",
  "StandardTimePeriod", "BasicTimePeriod", "GregorianTimePeriod",
  "GregorianYear", "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod",
  "ReportingYear", "ReportingSemester", "ReportingTrimester",
  "ReportingQuarter", "ReportingMonth", "ReportingWeek" and "ReportingDay". The
  default data type is "String" except for time `dimensions`, which takes
  "ObservationalTimePeriod" as default.
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
- pattern - _String_ _optional_. Holds any standard regular expression.

??? example

    ```json
    {
        "dataType": "String",
        "pattern": "^[0-9][0-9]$"
    }
    ```

###### format

_Object_ _optional_. Format defines the information for describing a range of
data formats restricted to the representations allowed for all components except
for target objects.

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
  "Duration", "GeospatialInformation" and "XHTML". `Dimensions` do not support
  the type "XHTML". Time `dimensions` (having the id and role "TIME_PERIOD")
  only support the types "ObservationalTimePeriod", "StandardTimePeriod",
  "BasicTimePeriod", "GregorianTimePeriod", "GregorianYear",
  "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod", "ReportingYear",
  "ReportingSemester", "ReportingTrimester", "ReportingQuarter",
  "ReportingMonth", "ReportingWeek", "ReportingDay", "DateTime", "TimeRange".
  The default data type is "String" except for time `dimensions`, which takes
  "ObservationalTimePeriod" as default.
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
- isMultiLingual - _Boolean_ _optional_. **Only for `measures` and
  `attributes`.** This indicates for a text format of type "String" or "XHTML",
  whether the uncoded component value should allow for multiple values in
  different languages. The default is `false`.
- sentinelValues - _Array_ of *Object*s _optional_. When present, the
  sentinelValues array indicates that sentinel values are defined for the data
  format. Each _[sentinelValue](#sentinelvalue)_ object indicates a reserved
  value in an otherwise open value domain that holds a specific meaning. For
  example, a value of -1 can be defined to indicate a non-applicable value.

??? example

    ```json
    {
        "dataType": "String",
        "maxLength": 1050,
        "pattern": "^[A-Za-z][A-Za-z0-9_-]*$",
        "isMultilingual": true,
        "sentinelValues": [
            # sentinelValue object #
        ]
    }
    ```

###### sentinelValue

_Object_. It defines a reserved value (within the value domain of the data
format) along with its meaning.

- value - _Number_ or _String_. The sentinel value being described.
- name - _String_. Human-readable (best-language-match) name (or meaning) of the
  sentinel value.
- names - _Object_ _optional_. Human-readable localised _[names](#names)_ (or
  meanings) of the sentinel value.
- description - _String_ _optional_. Human-readable (best-language-match)
  description for the sentinel value.
- descriptions - _Object_ _optional_. Human-readable localised descriptions (see
  _[names](#names)_) for the sentinel value.

??? example

    ```json
    {
        "value": -1,
        "name": "Special meaning",
        "names": {
            "en": "Special meaning",
            "fr": "Signification particulière"
        },
        "description": "Description for special meaning.",
        "descriptions": { "en": "Description for special meaning.",
                  "fr": "Description de signification particulière." }
    }
    ```

##### metadataAttributeUsage

_Object_. MetadataAttributeUsage defines how a metadata attribute is used in a
data structure. This is a local reference to a metadata attribute from the
metadata structure referenced by this data structure. An attribute relationship
can be defined in order to describe the relationship of the metadata attribute
to the data structure components.

- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- metadataAttributeReference - _String_. MetadataAttributeReference is a local
  (nested ID) reference to a metadata attribute defined in the metadata
  structure referenced by this data structure.
- attributeRelationship - _Object_. The
  _[attributeRelationship](#attributerelationship)_ object defines the
  relationship between the referenced metadata attribute and the components of
  the data structure.

??? example

    ```json
    {
        "metadataAttributeReference": "ATTR1.ATTR1-1",
        "attributeRelationship": {
            # attributeRelationship object #
        }
    }
    ```

#### dimensionList

_Object_ _optional_. DimensionList describes the key descriptor for a data
structure definition. The order of the declaration of child dimensions is
significant: it is used to describe the order in which they will appear in data
formats for which key values are supplied in an ordered fashion (exclusive of
the time dimension, which is not represented as a member of the ordered key).
Any data structure definition which uses the time dimension should also declare
a frequency dimension, conventionally the first dimension in the key (the set of
ordered non-time dimensions). It is not necessary to assign a time dimension, as
data can be organised in any fashion required.

- id - _String_ _optional_. Identifier for the dimensionList. However, this
  value is fixed to "DimensionDescriptor".
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- dimensions - _Array_ _optional_ of _[dimension](#dimension)_ objects that
  describe the structure of a dimension, which is defined as a statistical
  concept used (most probably together with other statistical concepts) to
  identify a statistical series, such as a time series, e.g. a statistical
  concept indicating certain economic activity or a geographical reference area.
  The list must not include the time dimension.
- timeDimension - _Object_ _optional_. The _[timeDimension](#timedimension)_
  object describes a special dimension which designates the period in time in
  which the data identified by the full series key applies.

??? example

    ```json
    {
        "id": "DimensionDescriptor",
        "dimensions": [
            {
                # dimension object #
            }
        ],
        "timeDimension": {
            # timeDimension object #
        },
    }
    ```

##### dimension

_Object_ _optional_. Dimension describes the structure of an ordinary dimension,
which is defined as a statistical concept used (most probably together with
other statistical concepts) to identify a statistical series, such as a time
series, e.g. a statistical concept indicating certain economic activity or a
geographical reference area. The dimension takes its semantic, and in some cases
it representation, from its concept identity. A dimension can be coded by
referencing a code list from its coded local representation. It can also specify
its text format, which is used as the representation of the dimension if a coded
representation is not defined. Neither the coded or uncoded representation are
necessary, since the dimension may take these from the referenced concept.

- id - _String_ _optional_. Identifier for the dimension. If not provided, the
  id is taken from the identifying concept.
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- position - _Integer_ _optional_. Positive integer (minimum: 0). The order of
  the dimensions in the key descriptor (DimensionList element) defines the order
  of the dimensions in the data structure, starting at 0. This position
  attribute explicitly specifies the position of the dimension in the data
  structure. It is optional and if specified must be consistent with the
  position of the dimension in the key descriptor.
- conceptIdentity - _String_. Urn reference to a concept where the
  identification of the concept scheme which defines it is contained in another
  context.
- conceptRoles - _Array_ of *String*s _optional_. ConceptRole references
  concepts (through URNs) which define roles which this dimension serves. If the
  concept from which the dimension takes its identity also defines a role the
  concept serves, then the isConceptRole indicator can be set to true on the
  concept identity rather than repeating the reference here.
- localRepresentation - _Object_ _optional_. The
  _[localRepresentation](#localrepresentation)_ object defines the
  representation for the dimension. Note that for dimensions the maxOccurs
  property must be 1, thus cannot be changed. Also the isMultiLingual format
  property cannot be set to true for dimensions.

??? example

    ```json
    {
        "id": "FREQ",
        "position": 0,
        "conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).FREQ",
        "conceptRoles": [
            "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).FREQ"
        ],
        "localRepresentation": {
            # localRepresentation object #
        }
    }
    ```

##### timeDimension

_Object_ _optional_. TimeDimension describes the structure of a time dimension.
The time dimension takes its semantic from its concept identity (usually the
TIME_PERIOD concept), yet is always has a fixed identifier (TIME_PERIOD). The
time dimension always has a fixed text format, which specifies that its format
is always the in the value set of the observational time period (see
common:ObservationalTimePeriodType). It is possible that the format may be a
sub-set of the observational time period value set. For example, it is possible
to state that the representation might always be a calendar year. See the
enumerations of the dataType attribute in the localRepresentation/format for
more details of the possible sub-sets. It is also possible to facet this
representation with start and end dates. The purpose of such facts is to
restrict the value of the time dimension to occur within the specified range. If
the time dimension is expected to allow for the standard reporting periods (see
common:ReportingTimePeriodType) to be used, then it is strongly recommended that
the reporting year start day attribute also be included in the data structure
definition. When the reporting year start day attribute is used, any standard
reporting period values will be assumed to be based on the start day contained
in this attribute. If the reporting year start day attribute is not included and
standard reporting periods are used, these values will be assumed to be based on
a reporting year which begins January 1.

- id - _String_ _optional_. Identifier for the time dimension. Fixed to
  "TIME_PERIOD".
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).lowing dimension to occur in any order.
- conceptIdentity - _String_. Urn reference to a concept where the
  identification of the concept scheme which defines it is contained in another
  context.
- localRepresentation - _Object_. The _localRepresentation_ object has only one
  required property _format_ of type
  _[timeDimensionFormat](#timedimensionformat)_ which defines the representation
  for the time dimension.

??? example

    ```json
    {
        "id": "TIME_PERIOD",
        "conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).TIME_PERIOD",
        "localRepresentation": {
            "format": {
                # timeDimension format object #
            }
        }
    }
    ```

##### timeDimensionFormat

_Object_ _optional_. The timeDimension format only allows time based format and
specifies a default ObservationalTimePeriod representation and facets of a start
and end time.

- endTime - _String_ _optional_. End time for the time dimension.
- startTime - _String_ _optional_. Start time for the time dimension.
- dataType - _String_ _optional_. Any of the following values:
  ObservationalTimePeriod, StandardTimePeriod, BasicTimePeriod,
  GregorianTimePeriod, GregorianYear, GregorianYearMonth, GregorianDay,
  ReportingTimePeriod, ReportingYear, ReportingSemester, ReportingTrimester,
  ReportingQuarter, ReportingMonth, ReportingWeek, ReportingDay, DateTime,
  TimeRange.
- sentinelValues - _Array_ _optional_. When present, the sentinelValues array
  indicates that sentinel values are defined for the text format. Each
  _[sentinelValue](#sentinelvalue)_ object indicates a reserved value in an
  otherwise open value domain that holds a specific meaning.

??? example

    ```json
    {
        "endTime": "2050",
        "startTime": "1960",
        "dataType": "ObservationalTimePeriod",
        "sentinelValues": [
            # sentinelValue object #
        ]
    }
    ```

#### group

_Object_ _optional_. Group describes the structure of a group descriptor in a
data structure definition. A group consist of a of partial key to which
attributes may be attached. The purpose of a group is to specify attributes
values which have the same value based on some common dimensionality. All groups
declared in the data structure must be unique - that is, you may not have
duplicate partial keys. All groups must be given unique identifiers.

- id - _String_. Identifier for the group.
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- groupDimensions - _Array_ _optional_. Contains local references (by ID) to
  dimensions in the key descriptor (DimensionList). Although it is conventional
  to declare dimensions in the same order as they are declared in the ordered
  key, there is no requirement to do so - the ordering of the values of the key
  are taken from the order in which the dimensions are declared. Note that the
  id of a dimension may be inherited from its underlying concept - therefore
  this reference value may actually be the id of the concept.

??? example

    ```json
    {
        "id": "GROUP1",
        "groupDimensions": [
            "CURRENCY",
            "CURRENCY_DENOM"
        ]
    }
    ```

#### measureList

_Object_ _optional_. MeasureList describes the structure of the measure
descriptor for a data structure definition.

- id - _String_ _optional_. Identifier for the measureList. Fixed to
  "MeasureDescriptor".
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- measures - _Array_. The _[measure](#measure)_ object describes the definition
  of a measure, which is the concept that is the value of the phenomenon to be
  measured in a data set. Although this may take its semantic from any concept,
  for a unique measure the common identifier "OBS_VALUE" is recommended.

??? example

    ```json
    {
        "id": "MeasureDescriptor",
        "measures": [
            {
                # measure object #
            }
        ]
    }
    ```

#### measure

_Object_ _optional_. Measure defines the structure of a measure, which is the
concept that is the value of the phenomenon to be measured in a data set. In
addition to the identifying concept and representation, a usage status and max
occurs can be defined. In case of a unique measure, conventionally the use of
the "OBS_VALUE" concept is recommended. A measure can be coded by referencing a
code list from its coded local representation. It can also specify its text
format, which is used as the representation of the measure if a coded
representation is not defined. Neither the coded or uncoded representation are
necessary, since the measure may take these from the referenced concept.

- id - _String_ _optional_. Identifier for the measure.
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- conceptIdentity - _String_. Urn reference to a concept where the
  identification of the concept scheme which defines it is contained in another
  context.
- conceptRoles - _Array_ of *String*s _optional_. ConceptRole references
  (through URNs) the concepts which define roles which this measure serves. If
  the concept from which the measure takes its identity also defines a role the
  concept serves, then the isConceptRole indicator can be set to true on the
  concept identity rather than repeating the reference here.
- localRepresentation - _Object_ _optional_. The
  _[localRepresentation](#localrepresentation)_ object defines the
  representation for the measure.
- usage - Constant _String_ `mandatory` or `optional` _optional_. Indication
  whether reporting a given measure is mandatory or optional. Default:
  `optional`.

??? example

    ```
    {
        "id": "OBS_VALUE",
        "conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).OBS_VALUE",
        "conceptRoles": [
            "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).OBS_VALUE"
        ],
        "localRepresentation": {
            # localRepresentation object #
        },
        "usage": "optional"
    }
    ```

### metadataStructure

_Object_. MetadataStructure provides the details of a metadata structure
definition, which is defined as a collection of metadata concepts and their
structure when used to collect or disseminate reference metadata. A metadata
structure definition performs several functions: it defines a presentational
organization of metadata concepts against which reference metadata may be
reported. The structure of a reference metadata message is derived from this
presentational structure.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

- metadataStructureComponents - _Object_ _optional_. The
  _[dataStructureComponents](#datastructurecomponents)_ object defines the
  grouping of the sets of the components that make up the metadata structure
  definition.

??? example

    ```json
    {
        "id": "DSD1",
        "version": "1.0.0",
        "agencyID": "SDMX",
        "metadadataStructureComponents": {
            # metadataStructureComponent object #
        }
    }
    ```

#### metadataStructureComponents

_Object_. MetadataStructureComponents describes the structure of one set of
components, the metadata attribute list, that make up the metadata structure
definition.

- metadataAttributeList - _Object_ _optional_. The
  _[metadataAttributeList](#metadataattributelist)_ defines the set of metadata
  attributes that can be defined as a hierarchy, for reporting reference
  metadata about a target object. The identification of metadata attributes must
  be unique at any given level of the metadata structure. Although there are XML
  schema constraints to help enforce this, these only apply to explicitly
  assigned identifiers. Identifiers inherited from a concept from which a
  metadata attribute takes its identity cannot be validated against this
  constraint. Therefore, systems processing metadata structure definitions will
  have to perform this check outside of the XML validation.

??? example

    ```json
    {
        "metadataAttributeList": {
            # metadataAttributeList object #
        }
    }
    ```

#### metadataAttributeList

_Object_. MetadataAttributeList describes the structure of a meta data attribute
list. It comprises a set of metadata attributes that can be defined as a
hierarchy.

- id - _String_. The id attribute is provided in this case for completeness.
  However, its value is fixed to "MetadataAttributeDescriptor".
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- metadataAttributes - _Array_ _optional_. The
  _[metadataAttribute](#metadataattribute)_ object defines the a metadata
  attribute, which is the value of an attribute, such as the instance of a coded
  or uncoded attribute in a metadata structure definition.

??? example

    ```json
    {
        "id": "MetadataAttributeDescriptor",
        "metadataAttributes": [
            {
                # metadataattribute object #
            }
        ]
    }
    ```

##### metadataAttribute

_Object_. MetadataAttribute describes the structure of a metadata attribute. The
metadata attribute takes its semantic, and in some cases it representation, from
its concept identity. A metadata attribute may be coded (via the local
representation), uncoded (via the text format), or take no value. In addition to
this value, the metadata attribute may also specify subordinate metadata
attributes. If a metadata attribute only serves the purpose of containing
subordinate metadata attributes, then the isPresentational attribute should be
used. Otherwise, it is assumed to also take a value. If the metadata attribute
does take a value, and a representation is not defined, it will be inherited
from the concept it takes its semantic from. The optional id on the metadata
attribute uniquely identifies it within the metadata structured definition. If
this id is not supplied, its value is assumed to be that of the concept
referenced from the concept identity. Note that a metadata attribute (as
identified by the id attribute) definition must be unique across the entire
metadata structure definition (including target identifier, identifier
component, and report structure ids). A metadata attribute may be used at
different levels, but the content (value and/or child metadata attributes and
their cardinality) of the metadata attribute cannot change.

- id - _String_ _optional_. Identifier for the metadata attribute.
- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- conceptIdentity - _String_. Urn reference to a concept where the
  identification of the concept scheme which defines it is contained in another
  context.
- localRepresentation - _Object_ _optional_. The
  _[localRepresentation](#localrepresentation)_ object defines the
  representation for the attribute. Note that the localRepresentation's
  `minOccurs` and `maxOccurs` properties are prohibited for this component type.
  The number of values that can be reported for a metadata attribute is
  always 1.
- minOccurs - _Non-negative integer_ _optional_. Indicates the minimum number of
  times this metadata attribute must occur within its parent object. If missing
  than there is no lower limit on its occurrences. The default is 1.
- maxOccurs - _Positive integer_/_String_ _optional_. Indicates the maximum
  number of times this metadata attribute can occur within its parent object. If
  set to the string "unbounded" than there is no upper limit on its occurrences.
  The default is 1.
- isPresentational - _Boolean_ _optional_. The isPresentational attribute
  indicates whether the metadata attribute should allow for a value. A value of
  true, meaning the metadata attribute is presentational means that the
  attribute only contains child metadata attributes, and does not contain a
  value. If this attribute is not set to true, and a representation (coded or
  uncoded) is not defined, then the representation of the metadata attribute
  will be inherited from the concept from which it takes its identity. The
  default is false.
- metadataAttributes - _Array_ _optional_. The
  _[metadataAttribute](#metadataattribute)_ object defines the a child metadata
  attribute.

??? example

    ```json
    {
        "id": "META_ATTR",
        "conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=OECD:OECD_CONCEPTS(1.0).META_ATTR",
        "localRepresentation": {
            # localRepresentation object #
        },
        "minOccurs": 2,
        "maxOccurs": 5,
        "isPresentational": true,
        "metadataAttributes": [
            {
                # metadataattribute object #
            }
        ]
    }
    ```

### categoryScheme

_Object_. Describes the structure of a category scheme. A category scheme is the
descriptive information for an arrangement or division of categories into groups
based on characteristics, which the objects have in common. This provides for a
simple, leveled hierarchy or categories.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.
There are no additional parameters. The start, end and parent properties are not
used.

??? example

    ```json
    {
        "id": "TOPICS",
        "version": "1.0.0",
        "agencyID": "SDMX",
        "name": "Topics",
        "names": {
            "en-GB-oed": "Topics",
            "fr": "Thèmes"
        },
        "isPartial": true,
        "categories": [
            {
                "id": "TOPIC1",
                "name": "Topic 1",
                "names": {
                    "en-GB-oed": "Topic 1",
                    "fr": "Thème 1"
                },
                "categories": [
                    {
                        "id": "SUBTOPIC11",
                        "name": "Topic 11",
                        "names": {
                            "en-GB-oed": "Topic 11",
                            "fr": "Thème 11"
                        }
                    }
                ]
            }
        ]
    }
    ```

### conceptScheme

_Object_. Describes the structure of a concept scheme. A concept scheme is the
descriptive information for an arrangement or division of concepts into groups
based on characteristics, which the objects have in common. It contains a
collection of concept definitions, that may be arranged in simple hierarchies.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.

In addition, `conceptScheme`'s _[item](#item)_ artefacts share the following
common object properties:

- coreRepresentation - _Object_ _optional_. The
  _[coreRepresentation](#corerepresentation)_ object defines the core
  representation that are allowed for a concept. The text format allowed for a
  concept is that which is allowed for any non-target object component.
- isoConceptReference - _Object_ _optional_. The
  _[isoConceptReference](#isoconceptreference)_ object provides a reference to
  an ISO 11179 concept.
- parent - _String_ _optional_. Urn reference to a local concept. Parent
  captures the semantic relationships between concepts which occur within a
  single concept scheme. This identifies the concept of which the current
  concept is a qualification (in the ISO 11179 sense) or subclass. The start and
  end properties are not used.

??? example

    ```json
    {
        "id": "CS_BOP",
        "name": "Balance of Payments Concept Scheme",
        "names": {
            "en": "Balance of Payments Concept Scheme",
            "fr": "Schéma des concepts de Balance des paiements"},
        "agencyId": "IMF",
        "version": "1.9.0",
        "concepts": [
            {
                "id": "FREQ",
                "name": "Frequency",
                "names": {
                    "en": "Frequency",
                    "fr": "Fréquence"
                },
                "coreRepresentation": {
                    # coreRepresentation object #
                },
                "isoConceptReference": {
                    # isoConceptReference object #
                },
                "parent": "COMMON_CONCEPTS"
            }
        ]
    }
    ```

#### coreRepresentation

_Object_ _optional_. A core representation for a concept. It is either a
reference to a codelist which enumerates the possible values that can be used as
the representation of this concept, or a text format.

- enumeration - _String_ _optional_. Urn reference to a codelist or valuelist
  which enumerates the possible values that can be used as the representation of
  this concept. This must be a valid SDMX Registry URN (see SDMX Registry
  Specification for details) of a codelist.
- enumerationFormat - _Object_ _optional_. To be used only with a codelist
  reference. The _[enumerationFormat](#enumerationformat)_ object defines a
  restricted version of a text format that only allows facets and text types
  applicable to codes. Although the time facets permit any value, an actual code
  identifier does not support the necessary characters for time. Therefore these
  facets should not contain time in their values.
- format - _Object_ _optional_. As an exclusive alternative to a codelist
  reference the _[format](#format)_ object defines the information for
  describing a full range of data formats and may place restrictions on the
  component's values.
- minOccurs - _Non-negative Integer_ _optional_. Indicates the minimum number of
  values that can be reported for the component. If missing than there is no
  lower limit on its occurrences. The default is 1.
- maxOccurs - _Positive Integer_/_String_ _optional_. Indicates the maximum
  number of values that can be reported for the component. If set to the string
  "unbounded" than there is no upper limit on its occurrences. The default is 1.

??? example

    ```json
    {
        "enumeration": "urn:sdmx:org.sdmx.infomodel.codelist.Codelist=SDMX:CL_FREQ(2.0)",
        "enumerationFormat": {
            # enumerationFormat object #
        }
    }

    {
        "format": {
            # format object #
        }
    }
    ```

#### isoConceptReference

_Object_. Provides a reference to an ISO 11179 concept.

- conceptAgency - _String_.
- conceptID - _String_.
- conceptSchemeID - _String_.

### codelist

_Object_. Defines the structure of a codelist. A codelist is defined as a list
from which some statistical concepts (coded concepts) take their values.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.

Note that the SDMX Information Model does not foresee a `codes` property for
codelist items (`code`), hierarchies being expressed through the `parent`
property for codelist items, which contains the ID of the parent code. However,
for retrieval use cases, implementers can choose to resolve the parent-child
relationships also into recursive `code` properties of `code`. A `code`
describes the structure of a code. A code is defined as a language independent
set of letters, numbers or symbols that represent a concept whose meaning is
described in a natural language. Presentational information not present may be
added through the use of annotations.

In addition, `codelist`'s _[item](#item)_ artefacts share the following common
object properties:

- parent - _String_ _optional_. The ID of the parent code. Parent provides the
  ability to describe simple hierarchies within a single codelist, by
  referencing the ID value of another code in the same codelist. The start and
  end properties are not used.

??? example

    ```json
    {
        "id": "CODELIST1",
        "version": "1.0.0",
        "agencyID": "SDMX",
        "name": "Code list 1",
        "names": {
            "en": "Code list 1",
            "fr": "Liste de codes 1"
        },
        "isPartial": true,
        "codes": [
            {
                "id": "CODE1",
                "name": "Code 1",
                "names": {
                    "en": "Code 1",
                    "fr": "Code 1"
                }
            },
            {
                "id": "CODE11",
                "name": "Code 11",
                "names": {
                    "en": "Code 11",
                    "fr": "Code 11"
                }
                "parent": "CODE1"
            }
        ]
    }
    ```

### geographicCodelist

_Object_. Describes the structure of a geographic codelist. It comprises a set
of GeoFeatureSetCodes, by adding a value in the Code that follows a pattern to
represent a geo feature set.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### geoGridCodelist

_Object_. Describes the structure of a geographic grid code list. These define a
geographical grid composed of cells representing regular squared portions of the
Earth.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### valueList

_Object_. Describes the structure of value list. These represent a closed set of
values the can occur for a dimension, measure, or attribute. These may be
values, or values with names and descriptions (similar to a codelist).

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### hierarchy

_Object_. Describes the structure of a hierarchy. A hierarchy is defined as an
organised collection of codes that may participate in many parent/child
relationships with other codes in the list.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### hierarchyAssociation

_Object_. Describes the structure of a hierarchyAssociation. A
hierarchyAssociation associates a hierarchy with an identifiable object in the
context of another object.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### agencyScheme

_Object_. Defines a specific type of organisation scheme which contains only
maintenance agencies. The agency scheme maintained by a particular maintenance
agency is always provided a fixed identifier and version, and is never final.
Therefore, agencies can be added or removed without have to version the scheme.
Agencies schemes have no hierarchy, meaning that no agency may define a
relationship with another agency in the scheme. In fact, the actual parent
agency for an agency in a scheme is the agency which defines the scheme.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.

The `agencyScheme`'s _[item](#item)_ artefacts are agencies. Agency is an
organisation which maintains structural metadata such as classifications,
concepts, data structures, and metadata structures. In addition, agencies
include the following object properties:

- contacts - _Array_ _optional_. A collection of _[contacts](#contact)_.
  Provides contact information for the agency. The contacts defined for the
  organisation are specific to the agency role the organisation is serving.

See the schema file for more information.

??? example

    ```
    {
        "id": "AGENCIES",
        "version": "1.0.0",
        "agencyID": "SDMX",
        "isExternalReference": false,
        "name": "SDMX Agency Scheme",
        "names": {
            "en": "SDMX Agency Scheme",
            "fr": "Schéma des agences SDMX"
        },
        "links": [
            {
                "href": "https://web-service-root/agencyScheme/SDMX/AGENCIES/1.0",
                "rel": "self",
                "urn": "urn:sdmx:org.sdmx.infomodel.base.AgencyScheme=SDMX:AGENCIES(1.0)"
            }
        ],
        "isPartial": true,
        "agencies": [
            {
                "id": "SDMX",
                "name": "SDMX",
                "names": {
                    "en": "SDMX",
                    "fr": "SDMX"
                },
                "contacts": [
                    {
                        "id": "SDMX",
                        "name": "SDMX",
                        "names": {
                            "en": "SDMX",
                            "fr": "SDMX"
                        },
                        "uris": [
                            "sdmx.org"
                        ]
                    }
                ]
            }
        ]
    }
    ```

### dataProviderScheme

_Object_. Defines a type of organisation scheme which contains only data
providers. The data provider scheme maintained by a particular maintenance
agency is always provided a fixed identifier and version, and is never final.
Therefore, providers can be added or removed without have to version the scheme.
This scheme has no hierarchy, meaning that no organisation may define a
relationship with another organisation in the scheme.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.

See the schema file for more information.

### dataConsumerScheme

_Object_. Defines a type of organisation scheme which contains only data
consumers. The data consumer scheme maintained by a particular maintenance
agency is always provided a fixed identifier and version, and is never final.
Therefore, consumers can be added or removed without have to version the scheme.
This scheme has no hierarchy, meaning that no organisation may define a
relationship with another organisation in the scheme.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.

See the schema file for more information.

### metadataProviderScheme

_Object_. Defines a type of organisation scheme which contains only metadata
providers. The metadata provider scheme maintained by a particular maintenance
agency is always provided a fixed identifier and version, and is never final.
Therefore, providers can be added or removed without have to version the scheme.
This scheme has no hierarchy, meaning that no organisation may define a
relationship with another organisation in the scheme.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.

See the schema file for more information.

### organisationUnitScheme

_Object_. Defines a type of organisation scheme which simply defines
organisations and there parent child relationships. Organisations in this scheme
are assigned no particular role, and may in fact exist within the other type of
organisation schemes as well.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_. See
_[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)_.

See the schema file for more information.

### dataflow

_Object_. Describes the structure of a data flow. A data flow is defined as the
structure of data that will provided for different reference periods. If this
type is not referenced externally, then a reference to a data structure
definition must be provided.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

In addition, `dataflow` has the following property:

- structure - _String_ _optional_. Urn reference to the data structure
  definition which defines the structure of all data for this flow.

??? example

    ```
    {
        "id": "EXR",
        "version": "1.0.0",
        "agencyID": "ECB",
        "isExternalReference": false,
        "name": "Exchange Rates",
        "names": {
            "en": "Exchange Rates",
            "fr": "Taux de change"
        },
        "links": [
            {
                "href": "/dataflow/ECB/EXR/1.0",
                "rel": "self",
                "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)",
            }
        ],
        "structure": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)"
    }
    ```

### metadataflow

_Object_. Describes the structure of a metadata flow. A dataflow is defined as
the structure of reference metadata that will be provided for different
reference periods. If this type is not referenced externally, then a reference
to a metadata structure definition must be provided.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### reportingTaxonomy

_Object_. Describes the structure of a reporting taxonomy, which is a scheme
which defines the composition structure of a data report where each component
can be described by an independent structure or structure usage description.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### provisionAgreement

_Object_. Describes the structure of a provision agreement. A provision
agreement defines an agreement for a data provider to report data against a
flow.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### metadataProvisionAgreement

_Object_. Describes the structure of a metadata provision agreement. A metadata
provision agreement defines an agreement for a metadata provider to report
reference metadata against a flow.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### structureMap

_Object_. Describes the structure of a structureMap. StructureMap allows mapping
between data structures or dataflows.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### representationMap

_Object_. Describes the structure of a representationMap. RepresentationMap
allows mapping between representations.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### conceptSchemeMap

_Object_. Describes the structure of a conceptSchemeMap. ConceptSchemeMap allows
mapping between concepts in different concept schemes.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### categorySchemeMap

_Object_. Describes the structure of a categorySchemeMap. CategorySchemeMap
allows mapping between categories in different category schemes.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### organisationSchemeMap

_Object_. Describes the structure of a organisationSchemeMap.
OrganisationSchemeMap allows mapping between organisations in different
organisation schemes.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### reportingTaxonomyMap

_Object_. Describes the structure of a reportingTaxonomyMap.
ReportingTaxonomyMap allows mapping between reporting categories in different
reporting taxonomies.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### process

_Object_. Describes the structure of a process, which is a scheme which defines
or documents the operations performed on data in order to validate data or to
derive new information according to a given set of rules. Processes occur in
order, and will continue in order unless a transition dictates another step
should occur.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### categorisation

_Object_. Defines the structure for a categorisation. A source object is
referenced via an object reference and the target category is referenced via the
target category.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

In addition, `categorisation` has the following properties:

- source - _String_ _optional_. Source is a urn reference to an object to be
  categorized.
- target - _String_ _optional_. Target is a urn reference to the category that
  the referenced object is to be mapped to.

??? example

    ```json
    {
        "id": "53A341E8-D48B-767E-D5FF-E2E3E0E2BB19",
        "version": "1.0.0",
        "agencyID": "ECB",
        "isExternalReference": false,
        "name": "Categorise: DATAFLOWECB:EXR(1.0)",
        "names": {
            "en": "Categorise: DATAFLOWECB:EXR(1.0)",
            "fr": "Catégoriser: DATAFLOWECB:EXR(1.0)"
        },
        "links": [
            {
                "href": "/categorisation/ECB/53A341E8-D48B-767E-D5FF-E2E3E0E2BB19/1.0",
                "rel": "self",
                "urn": "urn:sdmx:org.sdmx.infomodel.categoryscheme.Categorisation=ECB:53A341E8-D48B-767E-D5FF-E2E3E0E2BB19(1.0)"
            }
        ],
        "source": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)",
        "target": "urn:sdmx:org.sdmx.infomodel.categoryscheme.CategoryScheme=ECB:MOBILE_NAVI(1.0).07"
    }
    ```

### dataConstraint

_Object_. DataConstraint specifies a sub set of the definition of the allowable
or available content of a data set in terms of the content or in terms of the
set of key combinations. The inclusion of a key or region in a constraint is
determined by first processing the included key sets, and then removing those
keys defined in the excluded key sets. If no included key sets are defined, then
it is assumed that all possible keys or regions are included, and any excluded
key or regions are removed from this complete set.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

In addition, `dataConstraint` has the following properties:

- role - _String_. The type attribute indicates whether this constraint states
  what data is actually present for the constraint attachment ("Actual"), or if
  it defines what content is allowed ("Allowed"). Actual data constraints cannot
  be managed (retrieved or uploaded) through standard structure queries since
  they are to be generated dynamically through data availability queries
  according to the real current data availability. Actual data constraints
  should thus not be (semantically) versioned.
- constraintAttachment - _Object_ _optional_. The
  _[constraintAttachment](#constraintattachment)_ object describes the
  collection of constrainable artefacts that the constraint is attached to.
- cubeRegions - _Array_ _optional_. A list of of _[cubeRegion](#cuberegion)_
  objects. CubeRegion describes a set of dimension values which define a region
  and attributes which relate to the region for the purpose of describing a
  constraint.
- dataKeySets - _Array_ _optional_. A list of of _[dataKeySet](#datakeyset)_
  objects. DataKeySet defines a collection of full or partial data keys.
- releaseCalendar - _Object_ _optional_. The
  _[releaseCalendar](#releasecalendar)_ defines dates on which the constrained
  data is to be made available.

??? example

    ```json
    {
        "role": "Allowed",
        "id": "EXR_CONSTRAINTS",
        "version": "1.0.0",
        "agencyID": "ECB",
        "isExternalReference": false,
        "name": "Constraints for the EXR dataflow",
        "names": {
            "en": "Constraints for the EXR dataflow",
            "fr": "Constraintes pour le dataflow EXR"
        },
        "links": [
            {
                "href": "/constraint/ECB/EXR_CONSTRAINTS/1.0",
                "rel": "self",
                "urn": "urn:sdmx:org.sdmx.infomodel.registry.ContentConstraint=ECB:EXR_CONSTRAINTS(1.0)"
            }
        ],
        "constraintAttachment": {
            # constraintAttachment object #
        },
        "cubeRegions": [
            {
                # cubeRegion object #
            }
        ],
        "dataKeySets": [
            {
                # dataKeySet object #
            }
        ],
        "releaseCalendar": {
            # releaseCalendar object #
        }
    }
    ```

### metadataConstraint

_Object_. MetadataConstraint specifies a sub set of the definition of the
allowable content of a metadata set.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

In addition, `metadataConstraint` has the following properties:

- role - _String_. The role attribute is fixed to "Allowed" and indicates that
  this constraint defines what content is allowed.
- constraintAttachment - _Object_ _optional_. The
  _[constraintAttachment](#constraintattachment)_ object describes the
  collection of constrainable artefacts that the constraint is attached to.
- metadataTargetRegions - _Array_ _optional_. A list of of
  _[metadataTargetRegion](#metadatatargetregion)_ objects which describes the
  values allowed for metadata attributes.
- releaseCalendar - _Object_ _optional_. The
  _[releaseCalendar](#releasecalendar)_ defines dates on which the constrained
  data is to be made available.

??? example

    ```json
    {
        "role": "Allowed",
        "id": "EXR_CONSTRAINTS",
        "version": "1.0.0",
        "agencyID": "ECB",
        "isExternalReference": false,
        "name": "Constraints for the EXR dataflow",
        "names": {
            "en": "Constraints for the EXR dataflow",
            "fr": "Constraintes pour le dataflow EXR"
        },
        "links": [
            {
                "href": "/constraint/ECB/EXR_CONSTRAINTS/1.0",
                "rel": "self",
                "urn": "urn:sdmx:org.sdmx.infomodel.registry.ContentConstraint=ECB:EXR_CONSTRAINTS(1.0)"
            }
        ],
        "constraintAttachment": {
            # constraintAttachment object #
        },
        "metadataTargetRegions": [
            {
                # metadataTargetRegion object #
            }
        ],
        "releaseCalendar": {
            # releaseCalendar object #
        }
    }
    ```

#### constraintAttachment

_Object_. ConstraintAttachment describes a collection of references to
constrainable artefacts.

constraintAttachment properties for DataConstraints:

- dataProvider - _String_ _optional_. DataProvider is a URN reference to a data
  provider to which the constraint is attached. If this is used, then only the
  release calendar is relevant.
- dataStructures - _Array_ _optional_ of *string*s. URN references to data
  structure definitions to which the constraint is attached. A constraint which
  is attached to more than one data structure must only express key sets and/or
  cube regions where the identifiers of the dimensions are common across all
  structures to which the constraint is attached.
- dataflows - _Array_ _optional_ of *string*s. Urn references to data flows to
  which the constraint is attached. A constraint can be attached to more than
  one dataflow, and the dataflows do not necessarily have to be usages of the
  same data structure. However, a constraint which is attached to more than one
  data structure must only express key sets and/or cube regions where the
  identifiers of the dimensions are common across all structures to which the
  constraint is attached.
- provisionAgreements - _Array_ _optional_ of *string*s. Urn references to
  provision agreements to which the constraint is attached. A constraint can be
  attached to more than one data provision agreement, and the data provision
  agreements do not necessarily have to be references structure usages based on
  the same structure. However, a constraint which is attached to more than one
  data provision agreement must only express key sets and/or cube/target regions
  where the identifier of the components are common across all structures to
  which the constraint is attached.

constraintAttachment properties for MetadataConstraints:

- metadataProvider - _String_ _optional_. MetadataProvider is a URN reference to
  a metadata provider to which the constraint is attached. If this is used, then
  only the release calendar is relevant.
- metadataSets - _Array_ _optional_. URN references to metadata sets to which
  the constraint is attached.
- metadataStructures - _Array_ _optional_ of *string*s. URN references to
  metadata structure definitions to which the constraint is attached. A
  constraint which is attached to more than one metadata structure must only
  express key sets and/or target regions where the identifiers of the target
  objects are common across all structures to which the constraint is attached.
- metadataflows - _Array_ _optional_ of *string*s. Urn references to metadata
  flows to which the constraint is attached. A constraint can be attached to
  more than one metadataflow, and the metadataflows do not necessarily have to
  be usages of the same metadata structure. However, a constraint which is
  attached to more than one metadata structure must only express key sets and/or
  target regions where the identifiers of the target objects are common across
  all structures to which the constraint is attached.
- metadataProvisionAgreements - _Array_ _optional_ of *string*s. Urn references
  to metadata provision agreements to which the constraint is attached. A
  constraint can be attached to more than one metadata provision agreement, and
  the metadata provision agreements do not necessarily have to be references
  structure usages based on the same structure. However, a constraint which is
  attached to more than one metadata provision agreement must only express key
  sets and/or cube/target regions where the identifier of the components are
  common across all structures to which the constraint is attached.

constraintAttachment properties for any constraint:

- simpleDataSources - _Array_ _optional_ of *string*s. URLs of SDMX-ML data or
  metadata messages.
- queryableDataSources - _Object_ _optional_. The
  _[queryableDataSource](#queryabledatasource)_ object describes a queryable
  data source to which the constraint is attached. Used only with one of the
  dataStructures, dataflows, dataProvisionAgreements, metadataStructures,
  metadataflows and metadataProvisionAgreements properties.

??? example

    ```json
    {
        "dataProvider": "urn:sdmx:org.sdmx.infomodel...."
    }

    {
        "dataStructures": [
            "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)"
        ],
        "queryableDataSources": [
            {
                # queryableDataSource object #
            }
        ]
    }

    {
        "dataflows": [
            "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)"
        ],
        "queryableDataSources": [
            {
                # queryableDataSource object #
            }
        ]
    }

    {
        "provisionAgreements": [
            "urn:sdmx:org.sdmx.infomodel...."
        ],
        "queryableDataSources": [
            {
                # queryableDataSource object #
            }
        ]
    }

    {
        "metadataProvider": "urn:sdmx:org.sdmx.infomodel...."
    }

    {
        "metadataSets": [
            "urn:sdmx:org.sdmx.infomodel....", "urn:sdmx:org.sdmx.infomodel...."
        ]
    }

    {
        "metadataStructures": [
            "urn:sdmx:org.sdmx.infomodel.metadatastructure.MetadataStructure=ECB:ECB_EXR1_M(1.0)"
        ],
        "queryableDataSources": [
            {
                # queryableDataSource object #
            }
        ]
    }

    {
        "metadataflows": [
            "urn:sdmx:org.sdmx.infomodel.metadatastructure.Metadataflow=ECB:EXR_M(1.0)"
        ],
        "queryableDataSources": [
            {
                # queryableDataSource object #
            }
        ]
    }

    {
        "metadataProvisionAgreements": [
            "urn:sdmx:org.sdmx.infomodel...."
        ],
        "queryableDataSources": [
            {
                # queryableDataSource object #
            }
        ]
    }

    {
        "simpleDataSources": [
            "/data/EXR/M..EUR.SP00.A"
        ]
    }
    ```

##### queryableDataSource

_Object_. QueryableDataSource describes a data source which accepts an standard
SDMX Query message and responds appropriately.

- isRESTDatasource - _Boolean_.
- isWebServiceDatasource - _Boolean_.
- dataURL - _String_. DataURL contains the URL of the data source.
- wadlURL - _String_ _optional_. WADLURL provides the location of a WADL
  instance on the internet which describes the REST protocol of the queryable
  data source.
- wsdlURL - _String_ _optional_. WSDLURL provides the location of a WSDL
  instance on the internet which describes the queryable data source.

??? example

    ```json
    {
        "isRESTDatasource": true,
        "isWebServiceDatasource": true,
        "dataURL": "/data/EXR/M..EUR.SP00.A"
    }
    ```

#### cubeRegion

_Object_. CubeRegion defines the structure of a data cube region. This is based
on the abstract RegionType and simply refines the key and attribute values to
conform with what is applicable for dimensions and attributes, respectively. See
the documentation of the base type for more details on how a region is defined.

- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- include - _Boolean_ _optional_. Default: `true`. The include attribute
  indicates that the region is to be included or excluded within the context in
  which it is defined. For example, if the regions is defined as part of a
  content constraint, the exclude flag would mean the data identified by the
  region is not present.
- components - _Array_ _optional_ of _[ComponentValueSet](#componentvalueset)_
  objects containing a reference to a component (data attribute, metadata
  attribute, or measure) and providing a collection of values for the referenced
  component. This serves to state that for the key which defines the region, the
  components that are specified here have or do not have (depending on the
  include attribute of the value set) the values provided. It is possible to
  provide a component reference without specifying values, for the purpose of
  stating the component is absent (include = false) or present with an unbounded
  set of values. As opposed to key components, which are assumed to be wild
  carded if absent, no assumptions are made about the absence of a component.
  Only components which are explicitly stated to be present or absent from the
  region will be know. All unstated components for the set cannot be assumed to
  absent or present.
  
- keyValues - _Array_ _optional_ of _[CubeRegionKey](#cuberegionkey)_ objects
  containing a reference to a component which disambiguates the data (i.e. a
  dimension) and providing a collection of values for the component. The
  collection of values can be flagged as being inclusive or exclusive to the
  region being defined. Any key component that is not included is assumed to be
  wild carded, which is to say that the cube includes all possible values for
  the un-referenced key components. Further, this assumption applies to the
  values of the components as well. The values for any given component can only
  be sub-setted in the region by explicit inclusion or exclusion. For example, a
  dimension X which has the possible values of 1, 2, 3 is assumed to have all of
  these values if a key value is not defined. If a key value is defined with an
  inclusion attribute of true and the values of 1 and 2, the only the values of
  1 and 2 for dimension X are included in the definition of the region. If the
  key value is defined with an inclusion attribute of false and the value of 1,
  then the values of 2 and 3 for dimension X are included in the definition of
  the region. Note that any given key component must only be referenced once in
  the region.

??? example

    ```json
    {
        "include": true,
        "components": [
            {
                # ComponentValueSet object #
            }
        ],
        "keyValues": [
            {
                # CubeRegionKey object #
            }
        ]
    }
    ```

##### ComponentValueSet

_Object_. ComponentValueSet defines the structure for providing values for a
data attributes, measures, or metadata attributes. If no values are provided,
the component is implied to include/excluded from the region in which it is
defined, with no regard to the value of the component. Note that for metadata
attributes which occur within other metadata attributes, a nested identifier can
be provided. For example, a value of CONTACT.ADDRESS.STREET refers to the
metadata attribute with the identifier STREET which exists in the ADDRESS
metadata attribute in the CONTACT metadata attribute, which is defined at the
root of the report structure.

- id - _String_.
- include - _Boolean_ _optional_. The include attribute indicates whether the
  values provided for the referenced component are to be included are excluded
  from the region in which they are defined.
- removePrefix - _Boolean_ _optional_. The removePrefix attribute indicates
  whether codes should keep or not the prefix, as defined in the extension of
  codelist.
- timeRange - _Object_ _optional_. A _[TimeRangeValue](#timerangevalue)_ object.
- values - Non-empty _array_ _optional_ of *String*s and
  _[SimpleComponentValue](#simplecomponentvalue)_ objects. Only one of timeRange
  or values properties is allowed.

??? example

    ```json
    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "values": {
            "NRP0", "NN00", "NRD0", "NRU1"
            # SimpleComponentValue object #
        }
    }

    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "timeRange": {
            # TimeRangeValue object #
        }
    }
    ```

###### TimeRangeValue

_Object_. TimeRangeValue allows a time period value to be expressed as a range.
It can be expressed as the period before a period, after a period, or between
two periods. Each of these properties can specify their inclusion in regards to
the range.

- afterPeriod - _Object_ _optional_. A _[TimePeriodRange](#timeperiodrange)_
  object. AfterPeriod is the period after which the period is meant to cover.
  This date may be inclusive or exclusive in the range.
- beforePeriod - _Object_ _optional_. A _[TimePeriodRange](#timeperiodrange)_
  object. BeforePeriod is the period before which the period is meant to cover.
  This date may be inclusive or exclusive in the range.
- endPeriod - _Object_ _optional_. A _[TimePeriodRange](#timeperiodrange)_
  object. EndPeriod is the end period of the range. This date may be inclusive
  or exclusive in the range.
- startPeriod - _Object_ _optional_. A _[TimePeriodRange](#timeperiodrange)_
  object. StartPeriod is the start date or the range that the queried date must
  occur within. This date may be inclusive or exclusive in the range.

??? example

    ```json
    {
        "afterPeriod": {
            # TimePeriodRange object #
        },
        "beforePeriod": {
            # TimePeriodRange object #
        },
        "endPeriod": {
            # TimePeriodRange object #
        },
        "startPeriod": {
            # TimePeriodRange object #
        }
    }
    ```

###### TimePeriodRange

_Object_. TimePeriodRange defines a time period, and indicates whether it is
inclusive in a range.

- period - _String_ _optional_. Specifies a distinct time period or point in
  time in SDMX. The time period can either be a Gregorian calendar period, a
  standard reporting period, a distinct point in time, or a time range with a
  specific date and duration.
- isInclusive - _Boolean_ _optional_.

??? example

    ```json
    {
        "period": "2018",
        "isInclusive": true
    }
    ```

###### SimpleComponentValue

_Object_. SimpleComponentValue contains a simple value for a component, and if
that value is from a code list, the ability to indicate that child codes in a
simple hierarchy are part of the value set of the component for the region.

- value - _String_.
- lang - _String_ _optional_. IETF Language Tag according to
  [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for
  specifying locals in HTTP - _String_. The localised name.
- cascadeValues - _Boolean_ _optional_ or constant _string_ `excluderoot`.
  CascadeValues identifies if the value should be cascaded.
- validFrom - _String_ _optional_. A timestamp from which the value is valid.
  Values must follow the ISO 8601 syntax for combined dates and times, including
  time zone.
- validTo - _String_ _optional_. A timestamp from which the value is superseded.
  Values must follow the ISO 8601 syntax for combined dates and times, including
  time zone.

??? example

    ```json
    {
        "value": "NRP0",
        "lang": "en",
        "cascadeValues": "excluderoot",
        "validFrom": "2021-09-01",
        "validTo":"2021-09-30"
    }
    ```

##### CubeRegionKey

_Object_. CubeRegionKey provides a set of values for a dimension for the purpose
of defining a data cube region. A set of distinct value can be provided, or if
this dimension is represented as time, and time range can be specified.

- id - _String_.
- include - _Boolean_ _optional_. The include attribute indicates whether the
  values provided for the referenced component are to be included are excluded
  from the region in which they are defined.
- removePrefix - _Boolean_ _optional_. The removePrefix attribute indicates
  whether codes should keep or not the prefix, as defined in the extension of
  codelist.
- validFrom - _String_ _optional_. A timestamp from which the set of values is
  valid. Values must follow the ISO 8601 syntax for combined dates and times,
  including time zone.
- validTo - _String_ _optional_. A timestamp from which the set of values is
  superceded. Values must follow the ISO 8601 syntax for combined dates and
  times, including time zone.
- timeRange - _Object_ _optional_. A _[TimeRangeValue](#timerangevalue)_ object.
- values - Non-empty _array_ _optional_ of *String*s and
  _[SimpleComponentValue](#simplecomponentvalue)_ objects. Only one of timeRange
  or values properties is allowed.

??? example

    ```json
    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "validFrom": "2021-09-01",
        "validTo":"2021-09-30",
        "values": {
            "NRP0", "NN00", "NRD0", "NRU1"
            # SimpleComponentValue object #
        }
    }

    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "validFrom": "2021-09-01",
        "validTo":"2021-09-30",
        "timeRange": {
            # TimeRangeValue object #
        }
    }
    ```

#### dataKeySet

_Object_. dataKeySet defines a collection of full or partial data keys
(dimension values).

- isIncluded - _Boolean_.
- keys - Non-empty _array_ of _[dataKey](#datakey)_ objects. Data Key contains a
  set of dimension values which identify a full set of data.

??? example

    ```json
    {
        "isIncluded": true,
        "keys": [
            {
                # DataKey object #
            }
        ]
    }
    ```

##### dataKey

_Object_. DataKey is a region which defines a distinct full or partial data key.
The key consists of a set of values, each referencing a dimension and providing
a single value for that dimension. The purpose of the key is to define a subset
of a data set (i.e. the observed value and data attribute) which have the
dimension values provided in this definition. Any dimension not stated
explicitly in this key is assumed to be wild carded, thus allowing for the
definition of partial data keys.

- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- include - _Boolean_.
- validFrom - _String_ _optional_. A timestamp from which the region is valid.
  Values must follow the ISO 8601 syntax for combined dates and times, including
  time zone.
- validTo - _String_ _optional_. A timestamp from which the region is
  superseded. Values must follow the ISO 8601 syntax for combined dates and
  times, including time zone.
- keyValues - Non-empty _array_ of _[dataKeyValue](#datakeyvalue)_ objects.
- components - Non-empty _array_ of
  _[dataComponentValueSet](#datacomponentvalueset)_ objects.

??? example

    ```json
    {
        "include": "true",
        "validFrom": "2021-09-01",
        "validTo":"2021-09-30",
        "keyValues": [
            {
                # dataKeyValue object #
            }
        ],
        "components": [
            {
                # dataComponentValueSet object #
            }
        ]
    }
    ```

###### dataKeyValue

_Object_. DataKeyValue provides a dimension value for the purpose of defining a
distinct data key. Only a single value can be provided for the dimension.

- id - _String_.
- include - _Boolean_ _optional_. The include attribute indicates whether the
  values provided for the referenced component are to be included are excluded
  from the region in which they are defined.
- removePrefix - _Boolean_ _optional_. The removePrefix attribute indicates
  whether codes should keep or not the prefix, as defined in the extension of
  codelist.
- value - _String_.

??? example

    ```json
    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "value": "NRP0"
    }
    ```

###### dataComponentValueSet

_Object_. dataComponentValueSet defines the structure for providing values for a
data attributes, measures, or metadata attributes. If no values are provided,
the component is implied to include/excluded from the region in which it is
defined, with no regard to the value of the component. Note that for metadata
attributes which occur within other metadata attributes, a nested identifier can
be provided. For example, a value of CONTACT.ADDRESS.STREET refers to the
metadata attribute with the identifier STREET which exists in the ADDRESS
metadata attribute in the CONTACT metadata attribute, which is defined at the
root of the report structure.

- id - _String_.
- include - _Boolean_ _optional_. The include attribute indicates whether the
  values provided for the referenced component are to be included are excluded
  from the region in which they are defined.
- removePrefix - _Boolean_ _optional_. The removePrefix attribute indicates
  whether codes should keep or not the prefix, as defined in the extension of
  codelist.
- timeRange - _Object_ _optional_. A _[TimeRangeValue](#timerangevalue)_ object.
- values - Non-empty _array_ _optional_ of *String*s and
  _[datacomponentvalue](#datacomponentvalue)_ objects. Only one of timeRange or
  values properties is allowed.

??? example

    ```json
    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "values": {
            "DIM"
            # DataComponentValue object #
        }
    }

    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "timeRange": {
            # TimeRangeValue object #
        }
    }
    ```

###### DataComponentValue

_Object_. DataComponentValue contains a simple value for a component, and if
that value is from a code list, the ability to indicate that child codes in a
simple hierarchy are part of the value set of the component for the region.

- cascadeValues - _Boolean_ _optional_ or constant _string_ `excluderoot`.
  CascadeValues identifies if the value should be cascaded.
- lang - _String_ _optional_. IETF Language Tag according to
  [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for
  specifying locals in HTTP - _String_. The localised name.
- value - _String_.

??? example

    ```json
    {
        "cascadeValues": "excluderoot",
        "lang": "en",
        "value": "NRP0"
    }
    ```

#### metadataTargetRegion

_Object_. metadataTargetRegion defines the structure of a metadata target
region. A metadata target region must define the report structure and the
metadata target from that structure on which the region is based. This type is
based on the abstract RegionType and simply refines the key and attribute values
to conform with what is applicable for target objects and metadata attributes,
respectively. See the documentation of the base type for more details on how a
region is defined.

- annotations - _Array_ _optional_. Provides a list of annotation objects. See
  the section [annotation](#annotation).
- links - _Array_ _optional_. A collection of links to additional resources. See
  the section [link](#link).
- include - _Boolean_ _optional_. The include attribute indicates that the
  region is to be included or excluded within the context in which it is
  defined. For example, if the regions is defined as part of a content
  constraint, the exclude flag would mean the data identified by the region is
  not present.
- components - _Array_ _optional_ of
  _[MetadataAttributeValueSet](#metadataattributevalueset)_ objects containing a
  reference to a component (data attribute, metadata attribute, or measure) and
  provides a collection of values for the referenced component. This serves to
  state that for the key which defines the region, the components that are
  specified here have or do not have (depending on the include attribute of the
  value set) the values provided. It is possible to provide a component
  reference without specifying values, for the purpose of stating the component
  is absent (include = false) or present with an unbounded set of values. As
  opposed to key components, which are assumed to be wild carded if absent, no
  assumptions are made about the absence of a component. Only components which
  are explicitly stated to be present or absent from the region will be know.
  All unstated components for the set cannot be assumed to absent or present.
- validFrom - _String_ _optional_. A timestamp from which the region is valid.
  Values must follow the ISO 8601 syntax for combined dates and times, including
  time zone.
- validTo - _String_ _optional_. A timestamp from which the region is
  superseded. Values must follow the ISO 8601 syntax for combined dates and
  times, including time zone.

??? example

    ```json
    {
        "include": true,
        "validFrom": "2021-09-01",
        "validTo":"2021-09-30",
        "components": [
            {
                # MetadataAttributeValueSet object #
            }
        ]
    }
    ```

##### MetadataAttributeValueSet

_Object_. MetadataAttributeValueSet defines the structure for providing values
for a metadata attribute. If no values are provided, the attribute is implied to
include/excluded from the region in which it is defined, with no regard to the
value of the metadata attribute. Note that for metadata attributes which occur
within other metadata attributes, a nested identifier can be provided. For
example, a value of CONTACT.ADDRESS.STREET refers to the metadata attribute with
the identifier STREET which exists in the ADDRESS metadata attribute in the
CONTACT metadata attribute, which is defined at the root of the report
structure.

- id - _String_.
- include - _Boolean_ _optional_. The include attribute indicates whether the
  values provided for the referenced component are to be included are excluded
  from the region in which they are defined.
- removePrefix - _Boolean_ _optional_. The removePrefix attribute indicates
  whether codes should keep or not the prefix, as defined in the extension of
  codelist.
- timeRange - _Object_ _optional_. A _[TimeRangeValue](#timerangevalue)_ object.
- values - Non-empty _array_ _optional_ of *String*s and
  _[SimpleComponentValue](#simplecomponentvalue)_ objects. Only one of timeRange
  or values properties is allowed.

??? example

    ```json
    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "values": {
            "NRP0", "NN00", "NRD0", "NRU1"
            # SimpleComponentValue object #
        }
    }

    {
        "id": "EXR_TYPE",
        "include": true,
        "removePrefix": false,
        "timeRange": {
            # TimeRangeValue object #
        }
    }
    ```

#### releaseCalendar

_Object_. The ReleaseCalendar describes information about the timing of releases
of the constrained data. All of these values use the standard "P7D" - style
format.

- offset - _String_. Offset is the interval between January first and the first
  release of data within the year.
- periodicity - _String_. Periodicity is the period between releases of the data
  set.
- tolerance - _String_. Tolerance is the period after which the release of data
  may be deemed late.

??? example

    ```json
    {
        "offset": "30",
        "periodicity": "12",
        "tolerance": "2019"
    }
    ```

### customTypeScheme

_Object_. CustomTypeScheme provides the details of a custom type scheme, in
which user defined operators are described.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### vtlMappingScheme

_Object_. VtlMappingScheme provides the details of a VTL mapping scheme, in
which VTL mappings are described.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### namePersonalisationScheme

_Object_. NamePersonalisationScheme provides the details of a name
personalisation scheme, in which name personalisations are described.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### rulesetScheme

_Object_. RulesetScheme provides the details of a ruleset scheme, in which
rulesets are described.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### transformationScheme

_Object_. TransformationScheme provides the details of a transformation scheme,
in which transformations are described.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

### userDefinedOperatorScheme

_Object_. UserDefinedOperatorScheme provides the details of a user defined
operator scheme, in which user defined operators are described.

See _[Common SDMX artefact properties](#common-sdmx-artefact-properties)_.

See the schema file for more information.

## error

_Object_ _optional_. Used to provide a error message in addition to RESTful web
services HTTP error status codes. The following pieces of information are to be
provided:

- code - _Number_. Provides a code number for the error message. Code numbers
  are defined in the SDMX 2.1 Web Services Guidelines.
- title - _String_ _optional_. A short, human-readable (best-language-match)
  summary of the problem that SHOULD NOT change from occurrence to occurrence of
  the problem, except for purposes of localization.
- titles - _Object_ _optional_. A list of short, human-readable localised
  summaries (see _[names](#names)_) of the problem that SHOULD NOT change from
  occurrence to occurrence of the problem, except for purposes of localization.
- detail - _String_ _optional_. A human-readable (best-language-match)
  explanation specific to this occurrence of the problem. Like title, this
  field’s value can be localized. It is fully customizable by the service
  providers and should provide enough detail to ease understanding the reasons
  of the error.
- details - _Object_ _optional_. A list of human-readable localised explanations
  (see _[names](#names)_) specific to this occurrence of the problem. Like
  titles, this field’s value can be localized. It is fully customizable by the
  service providers and should provide enough detail to ease understanding the
  reasons of the error.
- links - _Array_ _optional_. _Links_ field is an array of _[link](#link)_
  objects. If appropriate, a collection of links to additional external
  resources for the error.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    {
        "code": 150,
        "title": "Invalid number of items in the item parameter",
        "titles": {
            "en": "Invalid number of items in the item parameter",
            "fr": "Nombre invalide d'items dans le paramètre 'item'"
        }
    }
    ```
