# Introduction to SDMX-JSON Structure Message 2.1.0

See the SDMX-JSON Data Message docs for a brief introduction of the SDMX information model. For additional information on the SDMX information model, please refer to the [SDMX documentation](http://sdmx.org/?page_id=10).

Samples, tools and other SDMX-JSON resources are available in the public Github repository <https://github.com/sdmx-twg/sdmx-json>.

Before we start, let's clarify a few more things about this guide:

- New fields may be introduced in later versions. Therefore consuming applications should tolerate the addition of new fields with ease.
- The ordering of properties in objects is undefined. The properties may appear in any order and consuming applications should not rely on any specific ordering. It is safe to consider a nulled property and the absence of a property as the same thing.
- Not all properties appear in all contexts. For example response with error messages may not contain a data property.

# Field Guide to SDMX-JSON Structure Message 2.1.0 Objects (aligned with SDMX 3.1)

## message

Message is the top level object and it contains the requested information ("data") as well as the meta-information describing the (technical) context of the message and, possibly, error information.

* $schema - *String* *optional*. Contains the URL to the schema allowing to validate the message. This also allows identifying the version of SDMX-JSON format used in this message. **Providing the link to the SDMX-JSON schema is recommended.**
* meta - *Object* *optional*. A *[meta](#meta)* object that contains non-standard meta-information and basic technical information about the message, such as when it was prepared and who has sent it.
* data - *Object* *optional*. *[Data](#data)* contains the message's “primary data”.
* errors - *Array* *optional* of *[statusInformation](#statusinformation)* objects providing - when appropriate - more detail to the HTTP status codes in the RESTful SDMX web service responses.

The properties data and errors CAN coexist in the same message.

Example:

	{
		"$schema": "https://json.sdmx.org/2.1/sdmx-json-data-schema.json",
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

## meta

*Object* *optional*. Used to include non-standard meta-information and basic technical information 
about the message, such as when it was prepared and who has sent it.
Any members MAY be specified within `meta` objects.
* schema - *String* *optional*. Deprecated and replaced by the `$schema` property at the root level, which allows for automated validations.
* id - *String*. Unique string that identifies the message for further references.
* test - *Boolean* *optional*. Indicates whether the message is for test purposes or not. False for normal messages.
* prepared - *String*. A date or date-time indicating when the message was prepared. Values must follow the ISO 8601 syntax for dates or combined dates and times, including time zone.
* contentLanguages - *Array* *optional*. Array of strings containing the identifier of all languages used anywhere in the message for localized elements, and thus the languages of the intended audience, representing in an array format the same information than the http Content-Language response header, e.g. "en, fr-fr". See IETF Language Tags: https://tools.ietf.org/html/rfc5646#section-2.1. The array's first element indicates the main language used in the message for localized elements. **The usage of this property is recommended.**
* name - *String* *optional*. Human-readable (best-language-match) name for the transmission.
* names - *Object* *optional*. Human-readable localised *[names](#names)* for the transmission.
* sender - *Object*. *[Sender](#sender)* contains information about the party that is transmitting the message.
* receiver - *Object* *optional*. *[Receiver](#receiver)* contains information about the party that is receiving the message. This can be useful if the WS requires authentication.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional external resources for the header.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

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
* name - *String* *nullable*. A human-readable (best-language-match) name of the sender.
* names - *Object* *optional*. A list of human-readable localised *[names](#names)* of the sender.
* contacts - *Array* *optional*. A collection of *[contacts](#contact)*. Provides contact information for the party in regard to the transmission of the message.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

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

#### names

*Object* containing all appropriate localised names, one per object property:

* One or more of: IETF Language Tag according to [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for specifying locals in HTTP - *String*. The localised name.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	{ 
		"en": "This is an English name",
		"en-GB": "This is a British name",
		"fr": "C'est un nom français"
	}

#### contact

*Object*. A collection of contact details. Each object in the collection 
may contain the following field:

* id - *String*. Identifier for the resource.
* name - *String* *optional*. Human-readable (best-language-match) name of the contact.
* names - *Object* *optional*. Human-readable localised *[names](#names)* of the contact.
* department - *String* *optional*. Human-readable (best-language-match) name of the organisational structure for the contact.
* departments - *Object* *optional*. Human-readable localised *[names](#names)* of the organisational structure for the contact.
* role - *String* *optional*. Human-readable (best-language-match) name of the responsibility of the contact.
* roles - *Object* *optional*. Human-readable localised *[names](#names)* of the responsibility of the contact.
* telephones - *Array* *optional*. An array of telephone numbers for the contact.
* faxes - *Array* *optional*. An array of fax numbers for the contact person.
* uris - *Array* *optional*. An array of uris. Each uri holds an information URL for the contact.
* emails - *Array* *optional*. An array of email addresses for the contact person.
* x400s - *Array* *optional*. An array of X.400 addresses for the contact person.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

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

### receiver

*Object* *optional*. Information about the party that is receiving the message. 
This can be useful if the WS requires authentication. Receiver contains the 
same fields as [sender](#sender).

### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

## data

*Object* *optional*. Header contains the message's “primary data”.

* *[Artefact type]* - *Array* *optional*. This field is an array of objects of one of the corresponding SDMX Information Model artefact types: *dataStructure*, *metadataStructure*, *categoryScheme*, *conceptScheme*, *codelist*, *geographicCodelists*, *geoGridCodelists*, *valueLists*, *hierarchy*, *hierarchyAssociation*, *agencyScheme*, *dataProviderScheme*, *dataProviderScheme*, *metadataProviderScheme*, *organisationUnitScheme*, *dataflow*, *metadataflow*, *reportingTaxonomy*, *provisionAgreement*, *metadataProvisionAgreement*, *structureMap*, *representationMap*, *conceptSchemeMap*, *categorySchemeMap*, *organisationSchemeMap*, *reportingTaxonomyMap*, *process*, *categorisation*, *dataConstraint*, *metadataConstraint*, *customTypeScheme*, *vtlMappingScheme*, *namePersonalisationScheme*, *rulesetScheme*, *transformationScheme* and *userDefinedOperatorScheme*. Each of the corresponding object properties is allowed at maximum one time. Contains the requested structural information according to the definition of this artefact. For more information, please see:

    * *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*
    * *[Common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*

    * *[dataStructures](#datastructure)*
    * *[metadataStructures](#metadatastructure)*
    * *[categorySchemes](#categoryscheme)*
    * *[conceptSchemes](#conceptscheme)*
    * *[codelists](#codelist)*
    * *[geographicCodelists](#geographiccodelist)*
    * *[geoGridCodelists](#geogridcodelist)*
    * *[valueLists](#valuelist)*
    * *[hierarchies](#hierarchy)*
    * *[hierarchyAssociations](#hierarchyassociation)*
    * *[agencySchemes](#agencyscheme)*
    * *[dataProviderSchemes](#dataproviderscheme)*
    * *[dataConsumerSchemes](#dataconsumerscheme)*
    * *[metadataProviderSchemes](#metadataproviderscheme)*
    * *[organisationUnitSchemes](#organisationunitscheme)*
    * *[dataflows](#dataflow)*
    * *[metadataflows](#metadataflow)*
    * *[reportingTaxonomies](#reportingtaxonomy)*
    * *[provisionAgreements](#provisionagreement)*
    * *[metadataProvisionAgreements](#metadataprovisionagreement)*
    * *[structureMaps](#structuremap)*
    * *[representationMaps](#representationmap)*
    * *[conceptSchemeMaps](#conceptschememap)*
    * *[categorySchemeMaps](#categoryschememap)*
    * *[organisationSchemeMaps](#organisationschememap)*
    * *[reportingTaxonomyMaps](#reportingtaxonomymap)*
    * *[processes](#process)*
    * *[categorisations](#categorisation)*
    * *[dataConstraints](#dataconstraint)*
    * *[availabilityConstraints](#availabilityconstraint)*
    * *[metadataConstraints](#metadataconstraint)*
    * *[customTypeSchemes](#customtypescheme)*
    * *[vtlMappingSchemes](#vtlmappingscheme)*
    * *[namePersonalisationSchemes](#namepersonalisationscheme)*
    * *[rulesetSchemes](#rulesetscheme)*
    * *[transformationSchemes](#transformationscheme)*
    * *[userDefinedOperatorSchemes](#userdefinedoperatorscheme)*

Example:

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

### Common SDMX artefact properties

All SDMX artefact types share the following common object properties:

* id - *String*. Identifier for the resource.
* agencyID - *String* *optional*. ID of the agency maintaining this resource.
* version - *String* *optional*. Version of this resource. Can be a legacy version with two version parts (e.g. '1.0') for fully mutable artefacts, or a semantic version according to SDMX versioning rules with 3 parts (e.g. '1.0.0') for immutable artefacts or with 4 parts (e.g. '1.0.0-draft') for artefacts allowed to change within a certain scope.
* name - *String*. Human-readable (best-language-match) name of the resource.
* names - *Object* *optional*. Human-readable localised *[names](#names)* of the resource.
* description - *String* *optional*. Human-readable (best-language-match) description of the resource.
* descriptions - *Object* *optional*. Human-readable localised descriptions (see *[names](#names)*) of the resource.
* validFrom - *String* *optional*. A timestamp from which the version is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the version is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* isPartialLanguage - *Boolean* *optional*. Default: `false`. Set to `true` if the structure doesn't contain the complete set of all available languages, e.g., when obtained as a response to a GET query that requested specific languages through the HTTP header “Accept-Language”. 
* isExternalReference - *Boolean* *optional*. If set to “true” it indicates that the content of the resource is held externally.
* annotations - *Array* *optional*. Provides a list of annotation objects.
* links - *Array* *optional*. A collection of links to additional resources for the resource. See the section *[link](#link)*. **It is recommended to systematically include as the first link a self-referencing hyperlink (link with "rel"="self") to indicate the URL address of the resource, and its URN.**

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Note about handling actions:

Structural metadata (artefacts) are maintainable. Thus, when interacting with SDMX Rest web services, the HTTP action verbs GET, PUT and POST are used to indicate the intended action per web request. Consequently, different actions cannot be bundled and executed with "transactional ACIDity". 

Example:

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
		"isPartialLanguage": true,
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

#### annotation

*Object* *optional*. Provides all information about an annotation. 

* id - *String* *optional*. ID provides a non-standard identification of an annotation. It can be used to disambiguate annotations.
* title - *String* *optional*. Provides a non-localised title for the annotation.
* type - *String* *optional*. Type is used to distinguish between annotations designed to support various uses. The types are not enumerated, and these can be freely specified by the creator of the annotations. The definitions and use of annotation types should be documented by their creator.
* value - *String* *optional*. Provides a non-localised value text for the annotation.
* text - *String* *optional*. A human-readable (best-language-match) text of the annotation.
* texts - *Object* *optional*. A list of human-readable localised texts (see *[names](#names)*) of the annotation.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a link to an additional external resource which may contain or supplement the annotation.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

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

### Common properties of SDMX artefacts of base type "ItemScheme"

All SDMX artefacts of base type "ItemScheme" (CategoryScheme, ConceptScheme, Codelist, GeographicCodelist, GeoGridCodelist, AgencyScheme, DataProviderScheme, MetadataProviderSchemes, DataConsumerScheme, OrganisationUnitScheme, ReportingTaxonomy, CustomTypeScheme, VtlMappingScheme, NamePersonalisationScheme, RulesetScheme, UserDefinedOperatorScheme) share the *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

In addition, they share the following common object properties:

* isPartial - *Boolean* *optional*. If set to true, it indicates that the resource contains only a sub-set of items.
* categories/concepts/codes/geoFeatureSetCodes/geoGridCodes/agencies/dataProviders/dataConsumers/metadataProviders/organisationUnits/reportingCategories/
customTypes/vtlMappings/namePersonalisations/rulesets/transformations/userDefinedOperators - *Array* *optional*. Provides a list of *[items](#item)* if the resource inherits from the ItemScheme. **Note that the order of items is significant. In the use case of a submission of a partial list is is necessary to include preceding and succeeding items to allow determining the correct positioniong of the submitted items.**

Example:

	{
		"id": "MY_DOMAINS",
		"isPartial": false,
		"categories": [
			{
				# item object #
			}
		]
	}

#### item

*Object* *optional*. Abtract generic item within the ItemScheme (if the resource is a CategoryScheme, ConceptScheme, Codelist, GeographicCodelist, GeoGridCodelist, AgencyScheme, DataProviderScheme, MetadataProviderSchemes, DataConsumerScheme, OrganisationUnitScheme, ReportingTaxonomy, CustomTypeScheme, VtlMappingScheme, NamePersonalisationScheme, RulesetScheme or UserDefinedOperatorScheme). 

* id - *String*. Identifier for the item.
* name - *String*. Human-readable (best-language-match) name of the item.
* names - *Object* *optional*. Human-readable localised *[names](#names)* of the item.
* description - *String* *optional*. Human-readable (best-language-match) description of the item. The description is typically longer than the text provided for the name field.
* descriptions - *Object* *optional*. Human-readable localised descriptions (see *[names](#names)*) of the item. A descriptions is typically longer than the text provided for the name field.
* parent - *String* *optional*. Contains the ID or the URN for the parent of the item (which is itself an item) enabling the reconstruction of the ordered item hierarchy. Prohibited in certain item scheme types.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources for the item. See the section [link](#link).
* categories/concepts/codes/geoFeatureSetCodes/geoGridCodes/agencies/dataProviders/dataConsumers/metadataProviders/organisationUnits/reportingCategories/
customTypes/vtlMappings/namePersonalisations/rulesets/transformations/userDefinedOperators - *Array* *optional*. Provides a list of child items of the item. **Note that the order of items is significant. In the use case of a submission of a partial list is is necessary to include preceding and succeeding items to allow determining the correct positioniong of the submitted items.**

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Examples:

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

### dataStructure

*Object*. Describes the structure of a data structure definition. A data structure definition is defined as a collection of metadata concepts, their structure and usage when used to collect or disseminate data.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

* dataStructureComponents - *Object* *optional*. The *[dataStructureComponents](#dataStructureComponents)* object defines the grouping of the sets of structural metadata concepts that have a defined structural role in the data structure definition, like dimensions and time dimension, measures, attributes and relationships with external metadata attributes. Note that for any component or group defined in a data structure definition, its id must be unique. This applies to the identifiers explicitly defined by the components as well as those inherited from the concept identity of a component. For example, if two dimensions take their identity from concepts with same identity (regardless of whether the concepts exist in different schemes) one of the dimensions must be provided a different explicit identifier. Although there are XML schema constraints to help enforce this, these only apply to explicitly assigned identifiers. Identifiers inherited from a concept from which a component takes its identity cannot be validated against this constraint. Therefore, systems processing data structure definitions will have to perform this check outside of the XML validation. There are also three reserved identifiers in a data structure definition: TIME_PERIOD, REPORTING_PERIOD_START_DAY and REPORTING_PERIOD_END_DAY. These identifiers may not be used outside of their respective defintions (TimeDimension and Attribute). This applies to both the explicit identifier that can be assigned to the components or groups as well as an identifier inherited by a component from its concept identity. For example, if an ordinary dimension (i.e. not the time dimension) takes its concept identity from a concept with the identifier TIME_PERIOD, that dimension must provide a different explicit identifier.
* metadata - *String* *optional*. The URN of a a metadata structure definition. A data structure definition may be related to a metadata structure definition in order to use its metadata attributes as part of the data. Note that the referenced metadata set cannot contain nested metadata attributes, as these are not supported in the data. By default all metadata attributes can be associated at any level of the data. However, a metadata attribute usage can be used to provide a specific attribute relationshp for a given metadata attribute.
* evolvingStructure  - *boolean* *optional*.  An evolving data structure indicates that new Dimensions may be added under a minor version update e.g. 1.0.0 to 1.1.0.  Evolving Data Structures can only be used by Dataflows if they include a DimensionConstraint to fix the Dimensions to the subset required by the Dataflow.

Example:

	{
		"id": "DSD1",
		"version": "1.0.0",
		"agencyID": "SDMX",
		"dataStructureComponents": {
			# dataStructureComponents object #
		},
		"evolvingStructure" false,
		"metadata": "urn:sdmx:org.sdmx.infomodel.metadatastructuredefinition.MetadataStructureDefinition=OECD:METADATA(1.0.0)"
	}

#### dataStructureComponents

*Object* *optional*. DataStructureComponents describes the structure of the grouping to the sets of structural concepts that have a defined structural role in the data structure definition. At a minimum at least one dimension must be defined.

* attributeList - *Object* *optional*. The *[attributeList](#attributeList)* object is a collection of structural concepts that define the attributes of the data structure definition. Attributes can relate to one or more measures, and be reported at the level of dataflow, several or all dimensions (observation).
* dimensionList - *Object*. The *[dimensionList](#dimensionList)* object is an ordered set of structural concepts that, combined, classify a statistical series, such as a time series, and whose values, when combined (the key) in an instance such as a data set, uniquely identify a specific series.
* groups - *Array* *optional*. Array of *[group](#group)* objects that are sets of structural concepts (and possibly their values) that define a partial key derived from the key descriptor in a data structure definition. 
* measureList - *Object* *optional*. The *[measureList](#measureList)* object is a collection of structural concepts that define the measures of the data structure definition. 

Example:

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

#### attributeList

*Object* *optional*. AttributeList describes the attributes in the data structure definition.

* id - *String*. Identifier for the attributeList. It is provided only for completeness. However, its value is fixed to "AttributeDescriptor".
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* attributes - *Array* *optional*. The *[attribute](#attribute)* object describes the definition of a data attribute, which is defined as a characteristic of an object or entity. The attribute list may contain the specialized data attribute that states the month and day at which the reporting year begins or ends, and which provides important context to the time dimension when the value of the time dimension is one of the standard reporting periods. This attribute provides a reference point from which the actual calendar dates covered by these periods can be determined. If this attribute does not occur in a data set, then the reporting year start/end day will be assumed to be January 1/December 31. This attribute can be recognised by its identifier "REPORTING_PERIOD_START_DAY" or "REPORTING_PERIOD_END_DAY".
* metadataAttributeUsages - *Array* *optional*. The *[metadataAttributeUsage](#metadataattributeusage)* object refines the details of how a metadata attribute from the metadata structure referenced from the data structure is used. By default, metadata attributes can be expressed at any level of the data. This allows an attribute relationship to be defined in order restrict the reporing of a metadata attribute to a specific part of the data.


Example: 

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
					
##### attribute

*Object* *optional*. Attribute describes the structure of a data attribute, which is defined as a characteristic of an object or entity. The attribute takes its semantic, and in some cases it representation, from its concept identity. An attribute can be coded by referencing a code list from its coded local representation. It can also specify its data format, which is used as the representation of the attribute if a coded representation is not defined. Neither the coded or uncoded representation are necessary, since the attribute may take these from the referenced concept. An attribute specifies its relationship with other data structure components and is given an assignment status. These two properties dictate where in a data message the attribute will be attached, and whether or not the attribute will be required to be given a value. A set of roles defined in concept scheme can be assigned to the attribute.

* id - *String* *optional*. Identifier for the attribute.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* usage - Constant *String* `mandatory` or `optional` *optional*. Indication whether reporting a given attribute is mandatory or optional. Default: `optional`, except for REPORTING_YEAR_START_DAY and REPORTING_YEAR_END_DAY attributes, for which the default is `mandatory`. 
* attributeRelationship - *Object*. The *[attributeRelationship](#attributeRelationship)* object describes how the value of this attribute varies with the values of other components. These relationships will be used to determine the attachment level of the attribute in the various data formats. 				
* measureRelationship - Non-empty *Array* of *String*s *optional*. This means that the value of the attribute **applies** only to some of the measure components, of which the array contains the ID(s). This is only for informational and presentational purposes. If the `measureRelationship` is not present then the attribute values apply to all measures.
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context. The reporting year start and end day attributes take their semantics from their respective concept identity (usually the REPORTING_YEAR_START_DAY and REPORTING_YEAR_END_DAY concepts), yet always have a fixed identifier (REPORTING_YEAR_START_DAY and REPORTING_YEAR_END_DAY).  
* conceptRoles - *Array* of *String*s *optional*. ConceptRole references (through URNs) the concepts which define roles which this attribute serves.
* localRepresentation - *Object* *optional*. The *[localRepresentation](#localRepresentation)* object defines the representation for the attribute. The representation for the reporting year start or end day attribute that has the conceptRole with concept ID "REPORTING_PERIOD_START_DAY"/"REPORTING_PERIOD_END_DAY" and states the month and day at which the reporting year begins or ends, does not allow for enumerated values and its text format is fixed to be a day and month in the ISO 8601 format of '--MM-DD'.

Example:

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
	
	Reporting year start day attribute:

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

###### attributeRelationship

*Object* *optional*. AttributeRelationship defines the structure for stating the relationship between an attribute and other data structure definition components.

* dataflow - Empty *Object* *optional*. This means that the value of the attribute does **not depend** on any dimension component, and therefore that the value of the attribute can **vary** only per dataflow. It is the data modeller's responsibility to design or use non-overlapping dataflows that do not have observations in common, otherwise the integrity of dataflow-specific attribute values is not assured by the model, e.g. when querying those data through its DSD. The level at which the unique attribute value will be presented in data messages depends on the data format and which dimensions are referenced.
* dimensions - Non-empty *Array* of *String*s *optional*. This means that the value of the attribute **depends** on only **some** of the dimension components, of which the array contains the ID(s), and therefore that the value of the attribute can **vary** only with each partial key combination of those dimensions (e.g., a group, a time series or a section). The level at which the attribute values will be presented in data messages depends on the data format and which dimensions are referenced.
* areDimensionsOptional - Non-empty *Array* of *Boolean*s *optional*. This is used to indicate those dimensions to which the attributes might optionally not be attached. The array structure must follow that for `dimensions`.
* group - *String* *optional*. Identifier of a local GroupKey Descriptor. This is used as a convenience to referencing all of the dimension defined by the referenced group. The level at which the attribute values will be presented in data messages depends on the data format and which dimensions are referenced. If the group (level) is available in the data format used then the attribute values should be presented at that group level.
* observation - Empty *Object* *optional*. This means that the value of the attribute **depends** on **all** of the dimension components, and therefore that the value of the attribute can **vary** with each observation. An attribute with this relationship will its values always have presented at observation level.

As with any other attribute, this relationship should be carefully selected also for the reporting year start or end day attribute as it will determine what type of data the data structure definition will allow. For example, if an attribute relationship of `dataflow` is specified, this will mean that the data sets for this dataflow can only contain data with standard reporting periods where the all reporting periods have the same start day. In this case, data reported as standard reporting periods from two entities with different fiscal year start days could not be contained in the same data set.

Examples:

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

###### localRepresentation

*Object* *optional*. LocalRepresentation defines the representation for the attribute. A data attribute can be text (including XHTML and multi-lingual values), a simple value, or an enumerated value.

* enumeration - *String* *optional*. Urn reference to an item scheme (such as a codelist) or a value list. Dimensions cannot reference a value list.
* enumerationFormat - *Object* *optional*. The *[enumerationFormat](#enumerationformat)* object defines a restricted version of a *[format](#format)* that only allows facets and text types applicable to items (codes) in the referenced scheme. Although the time facets permit any value, an actual code identifier does not support the necessary characters for time. Therefore these facets should not contain time in their values.
* format - *Object* *optional*. As an exclusive alternative to an item scheme reference the *[format](#format)* object defines the information for describing a full range of data formats and may place restrictions on the component's values. 
* minOccurs - *Non-negative Integer* *optional*. Indicates the minimum number of values that can be reported for the component. If missing than there is no lower limit on its occurrences. The default is 1.
* maxOccurs - *Positive Integer*/*String* *optional*. Indicates the maximum number of values that can be reported for the component. If set to the string "unbounded" than there is no upper limit on its occurrences. The default is 1.

The representation for the reporting year start/end day attribute that has the identifier "REPORTING_PERIOD_START_DAY"/"REPORTING_PERIOD_END_DAY" and states the month and day at which the reporting year begins/ends, does not allow for enumerated values and its text format is fixed to be a day and month in the ISO 8601 format of '--MM-DD'. Its *format* has thus only one single property *dataType* fixed to "MonthDay". This type exists solely for the purpose of fixing the representation of the reporting year start/end day attribute. Its maxOccurs property is a *Positive Integer* and is fixed to 1.

Examples:

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

##### enumerationFormat

*Object* *optional*. A restricted version of the *[format](#format)* that only allows facets and text types applicable to codes. Although the time facets permit any value, an actual code identifier does not support the necessary characters for time. Therefore these facets should not contain time in their values.

* dataType - *String* *optional*. Describes the type of data format allowed for the representation of the component. Only the following dataTypes are supported: "String",
 "Alpha", "AlphaNumeric", "Numeric", "BigInteger", "Integer", "Long", "Short", "Boolean", "URI", "Count", "InclusiveValueRange", "ExclusiveValueRange", "Incremental", "ObservationalTimePeriod", "StandardTimePeriod", "BasicTimePeriod", "GregorianTimePeriod", "GregorianYear", "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod", "ReportingYear", "ReportingSemester", "ReportingTrimester", "ReportingQuarter", "ReportingMonth", "ReportingWeek", "ReportingDay", "Month", "MonthDay", "Day" and "Duration". Time `dimensions` (having the id and role "TIME_PERIOD") only support the types "ObservationalTimePeriod", "StandardTimePeriod", "BasicTimePeriod", "GregorianTimePeriod", "GregorianYear", "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod", "ReportingYear", "ReportingSemester", "ReportingTrimester", "ReportingQuarter", "ReportingMonth", "ReportingWeek" and "ReportingDay". The default data type is "String" except for time `dimensions`, which takes "ObservationalTimePeriod" as default.
* isSequence - *Boolean* *optional*. Indicates whether the values are intended to be ordered, and it may work in combination with the interval, startValue, and endValue attributes or the timeInterval, startTime, and endTime, attributes. If this attribute holds a value of 'true', a start value or time and a numeric or time interval must supplied. If an end value is not given, then the sequence continues indefinitely.
* interval - *Integer* *optional*. Specifies the permitted interval (increment) in a sequence. In order for this to be used, the isSequence attribute must have a value of 'true'.
* startValue - *Number* *optional*. Is used in conjunction with the isSequence and interval attributes (which must be set in order to use this attribute). This attribute is used for a numeric sequence, and indicates the starting point of the sequence. This value is mandatory for a numeric sequence to be expressed.
* endValue - *Number* *optional*.  Is used in conjunction with the isSequence and interval attributes (which must be set in order to use this attribute). This attribute is used for a numeric sequence, and indicates that ending point (if any) of the sequence.
* timeInterval - *String* *optional*. Indicates the permitted duration in a time sequence. It complies with the time duration specification of the ISO 8601 standard. In order for this to be used, the isSequence attribute must have a value of 'true'.
* startTime - *String* *optional*. Is used in conjunction with the isSequence and timeInterval attributes (which must be set in order to use this attribute). This attribute is used for a time sequence, and indicates the start time of the sequence. This value is mandatory for a time sequence to be expressed. It must be a valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* endTime - *String* *optional*. Is used in conjunction with the isSequence and timeInterval attributes (which must be set in order to use this attribute). This attribute is used for a time sequence, and indicates that ending point (if any) of the sequence. It must be a valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* minLength - *Positive integer* *optional*. Specifies the minimum length of the value in characters.
* maxLength - *Positive integer* *optional*. Specifies the maximum length of the value in characters.
* minValue - *Number* *optional*. Is used for inclusive and exclusive ranges, indicating what the lower bound of the range is. If this is used with an inclusive range, a valid value will be greater than or equal to the value specified here. By default, the minValue is assumed to be inclusive.
* maxValue - *Number* *optional*. Is used for inclusive and exclusive ranges, indicating what the upper bound of the range is. If this is used with an inclusive range, a valid value will be less than or equal to the value specified here. By default, the maxValue is assumed to be inclusive.
* pattern - *String* *optional*. Holds any standard regular expression.

Example:

	{
		"dataType": "String",
		"pattern": "^[0-9][0-9]$"
	}

###### format

*Object* *optional*. Format defines the information for describing a range of data formats restricted to the representations allowed for all components except for target objects.

* dataType - *String* *optional*. Describes the type of data format allowed for the representation of the component. Only the following dataTypes are supported: "String",
 "Alpha", "AlphaNumeric", "Numeric", "BigInteger", "Integer", "Long", "Short", "Decimal", "Float", "Double", "Boolean", "URI", "Count", "InclusiveValueRange", "ExclusiveValueRange", "Incremental", "ObservationalTimePeriod", "StandardTimePeriod", "BasicTimePeriod", "GregorianTimePeriod", "GregorianYear", "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod", "ReportingYear", "ReportingSemester", "ReportingTrimester", "ReportingQuarter", "ReportingMonth", "ReportingWeek", "ReportingDay", "DateTime", "TimeRange", "Month", "MonthDay", "Day", "Time", "Duration", "GeospatialInformation" and "XHTML". `Dimensions` do not support the type "XHTML". Time `dimensions` (having the id and role "TIME_PERIOD") only support the types "ObservationalTimePeriod", "StandardTimePeriod", "BasicTimePeriod", "GregorianTimePeriod", "GregorianYear", "GregorianYearMonth", "GregorianDay", "ReportingTimePeriod", "ReportingYear", "ReportingSemester", "ReportingTrimester", "ReportingQuarter", "ReportingMonth", "ReportingWeek", "ReportingDay", "DateTime", "TimeRange". The default data type is "String" except for time `dimensions`, which takes "ObservationalTimePeriod" as default.
* isSequence - *Boolean* *optional*. Indicates whether the values are intended to be ordered, and it may work in combination with the interval, startValue, and endValue attributes or the timeInterval, startTime, and endTime, attributes. If this attribute holds a value of 'true', a start value or time and a numeric or time interval must supplied. If an end value is not given, then the sequence continues indefinitely.
* interval - *Integer* *optional*. Specifies the permitted interval (increment) in a sequence. In order for this to be used, the isSequence attribute must have a value of 'true'.
* startValue - *Number* *optional*. Is used in conjunction with the isSequence and interval attributes (which must be set in order to use this attribute). This attribute is used for a numeric sequence, and indicates the starting point of the sequence. This value is mandatory for a numeric sequence to be expressed.
* endValue - *Number* *optional*.  Is used in conjunction with the isSequence and interval attributes (which must be set in order to use this attribute). This attribute is used for a numeric sequence, and indicates that ending point (if any) of the sequence.
* timeInterval - *String* *optional*. Indicates the permitted duration in a time sequence. It complies with the time duration specification of the ISO 8601 standard. In order for this to be used, the isSequence attribute must have a value of 'true'.
* startTime - *String* *optional*. Is used in conjunction with the isSequence and timeInterval attributes (which must be set in order to use this attribute). This attribute is used for a time sequence, and indicates the start time of the sequence. This value is mandatory for a time sequence to be expressed. It must be a valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* endTime - *String* *optional*. Is used in conjunction with the isSequence and timeInterval attributes (which must be set in order to use this attribute). This attribute is used for a time sequence, and indicates that ending point (if any) of the sequence. It must be a valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* minLength - *Positive integer* *optional*. Specifies the minimum length of the value in characters.
* maxLength - *Positive integer* *optional*. Specifies the maximum length of the value in characters.
* minValue - *Number* *optional*. Is used for inclusive and exclusive ranges, indicating what the lower bound of the range is. If this is used with an inclusive range, a valid value will be greater than or equal to the value specified here. By default, the minValue is assumed to be inclusive.
* maxValue - *Number* *optional*. Is used for inclusive and exclusive ranges, indicating what the upper bound of the range is. If this is used with an inclusive range, a valid value will be less than or equal to the value specified here. By default, the maxValue is assumed to be inclusive.
* decimals - *Positive integer* *optional*. Indicates the number of characters allowed after the decimal separator.
* pattern - *String* *optional*. Holds any standard regular expression.
* isMultiLingual - *Boolean* *optional*. **Only for `measures` and `attributes`.** This indicates for a text format of type "String" or "XHTML", whether the uncoded component value should allow for multiple values in different languages. The default is `false`. 
* sentinelValues - *Array* of *Object*s *optional*. When present, the sentinelValues array indicates that sentinel values are defined for the data format. Each *[sentinelValue](#sentinelvalue)* object indicates a reserved value in an otherwise open value domain that holds a specific meaning. For example, a value of -1 can be defined to indicate a non-applicable value.

Example:

	{
		"dataType": "String",
		"maxLength": 1050,
		"pattern": "^[A-Za-z][A-Za-z0-9_-]*$",
		"isMultilingual": true,
		"sentinelValues": [
			# sentinelValue object #
		]
	}

###### sentinelValue

*Object*. It defines a reserved value (within the value domain of the data format) along with its meaning.

* value - *Number* or *String*. The sentinel value being described.
* name - *String*. Human-readable (best-language-match) name (or meaning) of the sentinel value.
* names - *Object* *optional*. Human-readable localised *[names](#names)* (or meanings) of the sentinel value.
* description - *String* *optional*. Human-readable (best-language-match) description for the sentinel value.
* descriptions - *Object* *optional*. Human-readable localised descriptions (see *[names](#names)*) for the sentinel value.

Example:

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

##### metadataAttributeUsage

*Object*. MetadataAttributeUsage defines how a metadata attribute is used in a data structure. This is a local reference to a metadata attribute from the metadata structure referenced by this data structure. An attribute relationship can be defined in order to describe the relationship of the metadata attribute to the data structure components.

* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* metadataAttributeReference - *String*. MetadataAttributeReference is a local (nested ID) reference to a metadata attribute defined in the metadata structure referenced by this data structure.
* attributeRelationship - *Object*. The *[attributeRelationship](#attributeRelationship)* object defines the relationship between the referenced metadata attribute and the components of the data structure.

Example:

	{
		"metadataAttributeReference": "ATTR1.ATTR1-1",
		"attributeRelationship": {
			# attributeRelationship object #
		}
	}

#### dimensionList

*Object* *optional*. DimensionList describes the key descriptor for a data structure definition. The order of the declaration of child dimensions is significant: it is used to describe the order in which they will appear in data formats for which key values are supplied in an ordered fashion (exclusive of the time dimension, which is not represented as a member of the ordered key). Any data structure definition which uses the time dimension should also declare a frequency dimension, conventionally the first dimension in the key (the set of ordered non-time dimensions). It is not necessary to assign a time dimension, as data can be organised in any fashion required.

* id - *String* *optional*. Identifier for the dimensionList. However, this value is fixed to "DimensionDescriptor".
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* dimensions - *Array* *optional* of *[dimension](#dimension)* objects that describe the structure of a dimension, which is defined as a statistical concept used (most probably together with other statistical concepts) to identify a statistical series, such as a time series, e.g. a statistical concept indicating certain economic activity or a geographical reference area. The list must not include the time dimension.
* timeDimension - *Object* *optional*. The *[timeDimension](#timeDimension)* object describes a special dimension which designates the period in time in which the data identified by the full series key applies.

Example:

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

##### dimension

*Object* *optional*. Dimension describes the structure of an ordinary dimension, which is defined as a statistical concept used (most probably together with other statistical concepts) to identify a statistical series, such as a time series, e.g. a statistical concept indicating certain economic activity or a geographical reference area. The dimension takes its semantic, and in some cases it representation, from its concept identity. A dimension can be coded by referencing a code list from its coded local representation. It can also specify its text format, which is used as the representation of the dimension if a coded representation is not defined. Neither the coded or uncoded representation are necessary, since the dimension may take these from the referenced concept.

* id - *String* *optional*. Identifier for the dimension. If not provided, the id is taken from the identifying concept.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* position - *Integer* *optional*. Positive integer (minimum: 0). The order of the dimensions in the key descriptor (DimensionList element) defines the order of the dimensions in the data structure, starting at 0. This position attribute explicitly specifies the position of the dimension in the data structure. It is optional and if specified must be consistent with the position of the dimension in the key descriptor.
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* conceptRoles - *Array* of *String*s *optional*. ConceptRole references concepts (through URNs) which define roles which this dimension serves.
* localRepresentation - *Object* *optional*. The *[localRepresentation](#localRepresentation)* object defines the representation for the dimension. Note that for dimensions the maxOccurs property must be 1, thus cannot be changed. Also the isMultiLingual format property cannot be set to true for dimensions.

Example:

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

##### timeDimension

*Object* *optional*. TimeDimension describes the structure of a time dimension. The time dimension takes its semantic from its concept identity (usually the TIME_PERIOD concept), yet is always has a fixed identifier (TIME_PERIOD). The time dimension always has a fixed text format, which specifies that its format is always the in the value set of the observational time period (see common:ObservationalTimePeriodType). It is possible that the format may be a sub-set of the observational time period value set. For example, it is possible to state that the representation might always be a calendar year. See the enumerations of the dataType attribute in the localRepresentation/format for more details of the possible sub-sets. It is also possible to facet this representation with start and end dates. The purpose of such facts is to restrict the value of the time dimension to occur within the specified range. If the time dimension is expected to allow for the standard reporting periods (see common:ReportingTimePeriodType) to be used, then it is strongly recommended that the reporting year start day attribute also be included in the data structure definition. When the reporting year start day attribute is used, any standard reporting period values will be assumed to be based on the start day contained in this attribute. If the reporting year start day attribute is not included and standard reporting periods are used, these values will be assumed to be based on a reporting year which begins January 1.

* id - *String* *optional*. Identifier for the time dimension. Fixed to "TIME_PERIOD".
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).lowing dimension to occur in any order.
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* localRepresentation - *Object* *optional*. The *localRepresentation* object has only one property *format* of type *[timeDimensionFormat](#timedimensionformat)*, which is required and which defines the representation for the time dimension. If omitted then the `localRepresentation` is of text format type `ObservationalTimePeriod`.

Example:

	{
		"id": "TIME_PERIOD",
		"conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).TIME_PERIOD",
		"localRepresentation": {
			"format": {
				# timeDimension format object #
			}
		}
	}

##### timeDimensionFormat

*Object* *optional*. The timeDimension format only allows time based format and specifies a default ObservationalTimePeriod representation and facets of a start and end time.

* endTime - *String* *optional*. End time for the time dimension.
* startTime - *String* *optional*. Start time for the time dimension.
* dataType - *String*. Any of the following values: ObservationalTimePeriod, StandardTimePeriod, BasicTimePeriod, GregorianTimePeriod, GregorianYear, GregorianYearMonth, GregorianDay, ReportingTimePeriod, ReportingYear, ReportingSemester, ReportingTrimester, ReportingQuarter, ReportingMonth, ReportingWeek, ReportingDay, DateTime, TimeRange.
* sentinelValues - *Array* *optional*. When present, the sentinelValues array indicates that sentinel values are defined for the text format. Each *[sentinelValue](#sentinelvalue)* object indicates a reserved value in an otherwise open value domain that holds a specific meaning.

Example:

	{
		"endTime": "2050",
		"startTime": "1960",
		"dataType": "ObservationalTimePeriod",
		"sentinelValues": [
			# sentinelValue object #
		]
	}

#### group

*Object* *optional*. Group describes the structure of a group descriptor in a data structure definition. A group consist of a of partial key to which attributes may be attached. The purpose of a group is to specify attributes values which have the same value based on some common dimensionality. All groups declared in the data structure must be unique - that is, you may not have duplicate partial keys. All groups must be given unique identifiers.

* id - *String*. Identifier for the group.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* groupDimensions - *Array* *optional*. Contains local references (by ID) to dimensions in the key descriptor (DimensionList). Although it is conventional to declare dimensions in the same order as they are declared in the ordered key, there is no requirement to do so - the ordering of the values of the key are taken from the order in which the dimensions are declared.

Examples:

	{
		"id": "GROUP1",
		"groupDimensions": [
			"CURRENCY",
			"CURRENCY_DENOM"
		]
	}

#### measureList

*Object* *optional*. MeasureList describes the structure of the measure descriptor for a data structure definition.

* id - *String* *optional*. Identifier for the measureList. Fixed to "MeasureDescriptor".
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* measures - *Array*. The *[measure](#measure)* object describes the definition of a measure, which is the concept that is the value of the phenomenon to be measured in a data set. Although this may take its semantic from any concept, for a unique measure the common identifier "OBS_VALUE" is recommended.

Example:

	{
		"id": "MeasureDescriptor",
		"measures": [
			{
				# measure object #
			}
		]
	}
		
#### measure

*Object* *optional*. Measure defines the structure of a measure, which is the concept that is the value of the phenomenon to be measured in a data set. In addition to the identifying concept and representation, a usage status and max occurs can be defined. In case of a unique measure, conventionally the use of the "OBS_VALUE" concept is recommended. A measure can be coded by referencing a code list from its coded local representation. It can also specify its text format, which is used as the representation of the measure if a coded representation is not defined. Neither the coded or uncoded representation are necessary, since the measure may take these from the referenced concept.

* id - *String* *optional*. Identifier for the measure.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* conceptRoles - *Array* of *String*s *optional*. ConceptRole references (through URNs) the concepts which define roles which this measure serves.
* localRepresentation - *Object* *optional*. The *[localRepresentation](#localRepresentation)* object defines the representation for the measure.
* usage - Constant *String* `mandatory` or `optional` *optional*. Indication whether reporting a given measure is mandatory or optional. Default: `optional`. 

Example:

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

### metadataStructure

*Object*. MetadataStructure provides the details of a metadata structure definition, which is defined as a collection of metadata concepts and their structure when used to collect or disseminate reference metadata. A metadata structure definition performs several functions: it defines a presentational organization of metadata concepts against which reference metadata may be reported. The structure of a reference metadata message is derived from this presentational structure.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

* metadataStructureComponents - *Object* *optional*. The *[dataStructureComponents](#dataStructureComponents)* object defines the grouping of the sets of the components that make up the metadata structure definition.


Example:

	{
		"id": "DSD1",
		"version": "1.0.0",
		"agencyID": "SDMX",
		"metadadataStructureComponents": {
			# metadataStructureComponent object #
		}
	}

#### metadataStructureComponents

*Object*. MetadataStructureComponents describes the structure of one set of components, the metadata attribute list, that make up the metadata structure definition.

* metadataAttributeList - *Object* *optional*. The *[metadataAttributeList](#metadataattributelist)* defines the set of metadata attributes that can be defined as a hierarchy, for reporting reference metadata about a target object. The identification of metadata attributes must be unique at any given level of the metadata structure. Although there are XML schema constraints to help enforce this, these only apply to explicitly assigned identifiers. Identifiers inherited from a concept from which a metadata attribute takes its identity cannot be validated against this constraint. Therefore, systems processing metadata structure definitions will have to perform this check outside of the XML validation.

Example:

	{
		"metadataAttributeList": {
			# metadataAttributeList object #
		}
	}

#### metadataAttributeList

*Object*. MetadataAttributeList describes the structure of a meta data attribute list. It comprises a set of metadata attributes that can be defined as a hierarchy.

* id - *String*. The id attribute is provided in this case for completeness. However, its value is fixed to "MetadataAttributeDescriptor".
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* metadataAttributes - *Array* *optional*. The *[metadataAttribute](#metadataattribute)* object defines the a metadata attribute, which is the value of an attribute, such as the instance of a coded or uncoded attribute in a metadata structure definition.


Example: 

	{
		"id": "MetadataAttributeDescriptor",
		"metadataAttributes": [
			{
				# metadataattribute object #
			}
		]
	}
					
##### metadataAttribute

*Object*. MetadataAttribute describes the structure of a metadata attribute. The metadata attribute takes its semantic, and in some cases it representation, from its concept identity. A metadata attribute may be coded (via the local representation), uncoded (via the text format), or take no value. In addition to this value, the metadata attribute may also specify subordinate metadata attributes. If a metadata attribute only serves the purpose of containing subordinate metadata attributes, then the isPresentational attribute should be used. Otherwise, it is assumed to also take a value. If the metadata attribute does take a value, and a representation is not defined, it will be inherited from the concept it takes its semantic from. The optional id on the metadata attribute uniquely identifies it within the metadata structured definition. If this id is not supplied, its value is assumed to be that of the concept referenced from the concept identity. Note that a metadata attribute (as identified by the id attribute) definition  must be unique across the entire metadata structure definition (including target identifier, identifier component, and report structure ids). A metadata attribute may be used at different levels, but the content (value and/or child metadata attributes and their cardinality) of the metadata attribute cannot change.

* id - *String* *optional*. Identifier for the metadata attribute.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* localRepresentation - *Object* *optional*. The *[localRepresentation](#localRepresentation)* object defines the representation for the attribute. Note that the localRepresentation's `minOccurs` and `maxOccurs` properties are prohibited for this component type. The number of values that can be reported for a metadata attribute is always 1.
* minOccurs - *Non-negative integer* *optional*. Indicates the minimum number of times this metadata attribute must occur within its parent object. If missing than there is no lower limit on its occurrences. The default is 1.
* maxOccurs - *Positive integer*/*String* *optional*. Indicates the maximum number of times this metadata attribute can occur within its parent object. If set to the string "unbounded" than there is no upper limit on its occurrences. The default is 1.
* isPresentational - *Boolean* *optional*. The isPresentational attribute indicates whether the metadata attribute should allow for a value. A value of true, meaning the metadata attribute is presentational means that the attribute only contains child metadata attributes, and does not contain a value. If this attribute is not set to true, and a representation (coded or uncoded) is not defined, then the representation of the metadata attribute will be inherited from the concept from which it takes its identity. The default is false.
* metadataAttributes - *Array* *optional*. The *[metadataAttribute](#metadataattribute)* object defines the a child metadata attribute.

Example:

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


### categoryScheme

*Object*. Describes the structure of a category scheme. A category scheme is the descriptive information for an arrangement or division of categories into groups based on characteristics, which the objects have in common. This provides for a simple, leveled hierarchy or categories.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.
There are no additional parameters.
The start, end and parent properties are not used.

Example:

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


### conceptScheme

*Object*. Describes the structure of a concept scheme. A concept scheme is the descriptive information for an arrangement or division of concepts into groups based on characteristics, which the objects have in common. It contains a collection of concept definitions, that may be arranged in simple hierarchies.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

In addition, `conceptScheme`'s *[item](#item)* artefacts share the following common object properties:

* coreRepresentation - *Object* *optional*. The *[coreRepresentation](#coreRepresentation)* object defines the core representation that are allowed for a concept. The text format allowed for a concept is that which is allowed for any non-target object component.
* isoConceptReference - *Object* *optional*. The *[isoConceptReference](#isoConceptReference)* object provides a reference to an ISO 11179 concept.
* parent - *String* *optional*. ID of a local concept. Parent captures the semantic relationships between concepts which occur within a single concept scheme. This identifies the concept of which the current concept is a qualification (in the ISO 11179 sense) or subclass.
The start and end properties are not used.

Example:

	{
		"id": "CS_EXAMPLE",
		"name": "Concept scheme example",
		"names": {
			"en": "Concept scheme example",
			"fr": "Exemple de concept scheme"},
		"agencyId": "ABC",
		"version": "1.9.0",
		"concepts": [
			{
				"id": "A",
				"name": "Concept A",
				"names": {
					"en": "Concept A",
					"fr": "Concept A"
				},
				"coreRepresentation": {
					# coreRepresentation object #
				},
				"isoConceptReference": {
					# isoConceptReference object #
				}
			},
			{
				"id": "A1",
				"name": "Concept A1",
				"names": {
					"en": "Concept A1",
					"fr": "Concept A1"
				},
				"parent": "A"
			}

		]
	}

#### coreRepresentation

*Object* *optional*. A core representation for a concept. It is either a reference to a codelist which enumerates the possible values that can be used as the representation of this concept, or a text format.

* enumeration - *String* *optional*. Urn reference to a codelist or valuelist which enumerates the possible values that can be used as the representation of this concept. This must be a valid SDMX Registry URN (see SDMX Registry Specification for details) of a codelist.
* enumerationFormat - *Object* *optional*. To be used only with a codelist reference. The *[enumerationFormat](#enumerationformat)* object defines a restricted version of a text format that only allows facets and text types applicable to codes. Although the time facets permit any value, an actual code identifier does not support the necessary characters for time. Therefore these facets should not contain time in their values.
* format - *Object* *optional*. As an exclusive alternative to a codelist reference the *[format](#format)* object defines the information for describing a full range of data formats and may place restrictions on the component's values.
* minOccurs - *Non-negative Integer* *optional*. Indicates the minimum number of values that can be reported for the component. If missing than there is no lower limit on its occurrences. The default is 1.
* maxOccurs - *Positive Integer*/*String* *optional*. Indicates the maximum number of values that can be reported for the component. If set to the string "unbounded" than there is no upper limit on its occurrences. The default is 1.

Examples:

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

#### isoConceptReference

*Object*. Provides a reference to an ISO 11179 concept.

* conceptAgency - *String*.
* conceptID - *String*.
* conceptSchemeID - *String*.

### codelist

*Object*. Defines the structure of a codelist. A codelist is defined as a list from which some statistical concepts (coded concepts) take their values.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

Note that the SDMX Information Model does not foresee a `codes` property for codelist items (`code`), hierarchies being expressed through the `parent` property for codelist items, which contains the ID of the parent code. However, for retrieval use cases, implementers can choose to resolve the parent-child relationships also into recursive `code` properties of `code`. A `code` describes the structure of a code. A code is defined as a language independent set of letters, numbers or symbols that represent a concept whose meaning is described in a natural language. Presentational information not present may be added through the use of annotations.

In addition, `codelist`'s *[item](#item)* artefacts share the following common object properties:

* parent - *String* *optional*. The ID of the parent code. Parent provides the ability to describe simple hierarchies within a single codelist, by referencing the ID value of another code in the same codelist.
The start and end properties are not used.

Example:

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


### geographicCodelist

*Object*. Describes the structure of a geographic codelist. It comprises a set of GeoFeatureSetCodes, by adding a value in the Code that follows a pattern to represent a geo feature set.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.


### geoGridCodelist

*Object*. Describes the structure of a geographic grid code list. These define a geographical grid composed of cells representing regular squared portions of the Earth.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.


### valueList

*Object*. Describes the structure of value list. These represent a closed set of values the can occur for a dimension, measure, or attribute. These may be values, or values with names and descriptions (similar to a codelist).

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.


### hierarchy

*Object*. Describes the structure of a hierarchy. A hierarchy is defined as an organised collection of codes that may participate in many parent/child relationships with other codes in the list.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### hierarchyAssociation

*Object*. Describes the structure of a hierarchyAssociation. A hierarchyAssociation associates a hiearchy with an identifiable object in the context of another object.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### agencyScheme

*Object*. Defines a specific type of organisation scheme which contains only maintenance agencies. The agency scheme maintained by a particular maintenance agency is always provided a fixed identifier and version, and is never final. Therefore, agencies can be added or removed without have to version the scheme. Agencies schemes have no hierarchy, meaning that no agency may define a relationship with another agency in the scheme. In fact, the actual parent agency for an agency in a scheme is the agency which defines the scheme.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

The `agencyScheme`'s *[item](#item)* artefacts are agencies. Agency is an organisation which maintains structural metadata such as classifications, concepts, data structures, and metadata structures. In addition, agencies include the following object properties:

* contacts - *Array* *optional*. A collection of *[contacts](#contact)*. Provides contact information for the agency. The contacts defined for the organisation are specific to the agency role the organisation is serving.

See the schema file for more information.

Example:

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

### dataProviderScheme

*Object*. Defines a type of organisation scheme which contains only data providers. The data provider scheme maintained by a particular maintenance agency is always provided a fixed identifier and version, and is never final. Therefore, providers can be added or removed without have to version the scheme. This scheme has no hierarchy, meaning that no organisation may define a relationship with another organisation in the scheme.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

See the schema file for more information.

### dataConsumerScheme

*Object*. Defines a type of organisation scheme which contains only data consumers. The data consumer scheme maintained by a particular maintenance agency is always provided a fixed identifier and version, and is never final. Therefore, consumers can be added or removed without have to version the scheme. This scheme has no hierarchy, meaning that no organisation may define a relationship with another organisation in the scheme.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

See the schema file for more information.

### metadataProviderScheme

*Object*. Defines a type of organisation scheme which contains only metadata providers. The metadata provider scheme maintained by a particular maintenance agency is always provided a fixed identifier and version, and is never final. Therefore, providers can be added or removed without have to version the scheme. This scheme has no hierarchy, meaning that no organisation may define a relationship with another organisation in the scheme.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

See the schema file for more information.

### organisationUnitScheme

*Object*. Defines a type of organisation scheme which simply defines organisations and there parent child relationships. Organisations in this scheme are assigned no particular role, and may in fact exist within the other type of organisation schemes as well.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

See the schema file for more information.

### dataflow

*Object*. Describes the structure of a data flow. A data flow is defined as the structure of data that will provided for different reference periods. If this type is not referenced externally, then a reference to a data structure definition must be provided.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

In addition, `dataflow` has the following property:

* structure - *String* *optional*. Urn reference to the data structure definition which defines the structure of all data for this flow.
* dimensionConstraint -  *Array* *optional* of *string*s. ID references to a fixed list of `dimensions` (by ID) to which a `dataflow` may be constrained. This is a required property if the `dataStructure` defines itself as an evolving structure, indicating that it can change dimensionality under a minor version change, and if the `dataflow` references that `dataStructure` using a wildcarded minor version number. New minor DSD version can so still be used by this `dataflow` even if that DSD version defines new additional `dimensions`. `Dimensions` not listed should not be used in `datasets` for this `dataflow`. The `timeDimension` is not to be listed, as it is always to be used when defined in the DSD, and it cannot be added to a DSD without increasing its major version.

Example:

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
		"structure": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
		"dimensionConstraint": ["FREQ", "CUR", "CURR_DEMOM"]
	}

### metadataflow

*Object*. Describes the structure of a metadata flow. A dataflow is defined as the structure of reference metadata that will be provided for different reference periods. If this type is not referenced externally, then a reference to a metadata structure definition must be provided.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### reportingTaxonomy

*Object*.  Describes the structure of a reporting taxonomy, which is a scheme which defines the composition structure of a data report where each component can be described by an independent structure or structure usage description.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### provisionAgreement

*Object*. Describes the structure of a provision agreement. A provision agreement defines an agreement for a data provider to report data against a flow.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### metadataProvisionAgreement

*Object*. Describes the structure of a metadata provision agreement. A metadata provision agreement defines an agreement for a metadata provider to report reference metadata against a flow.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### structureMap

*Object*. Describes the structure of a structureMap. StructureMap allows mapping between data structures or dataflows.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### representationMap

*Object*. Describes the structure of a representationMap. RepresentationMap allows mapping between representations.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### conceptSchemeMap

*Object*. Describes the structure of a conceptSchemeMap. ConceptSchemeMap allows mapping between concepts in different concept schemes.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### categorySchemeMap

*Object*. Describes the structure of a categorySchemeMap. CategorySchemeMap allows mapping between categories in different category schemes.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### organisationSchemeMap

*Object*. Describes the structure of a organisationSchemeMap. OrganisationSchemeMap allows mapping between organisations in different organisation schemes.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### reportingTaxonomyMap

*Object*. Describes the structure of a reportingTaxonomyMap. ReportingTaxonomyMap allows mapping between reporting categories in different reporting taxonomies.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### process

*Object*. Describes the structure of a process, which is a scheme which defines or documents the operations performed on data in order to validate data or to derive new information according to a given set of rules. Processes occur in order, and will continue in order unless a transition dictates another step should occur.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### categorisation

*Object*. Defines the structure for a categorisation. A source object is referenced via an object reference and the target category is referenced via the target category.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

In addition, `categorisation` has the following properties:

* source - *String* *optional*. Source is a urn reference to an object to be categorized.
* target - *String* *optional*. Target is a urn reference to the category that the referenced object is to be mapped to.

Example:

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

### dataConstraint

*Object*. A data constraint contains the allowed values for the referencing artefact. These values are expressed either as sets of keys (DataKeySets) or sets of component values (CubeRegion) constructed from a data structure definition. Data constraints can be used, e.g., for validation or for defining a partial code list.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

In addition, `dataConstraint` has the following properties:

* constraintAttachment - *Object* *optional*. The *[constraintAttachment](#constraintAttachment)* object describes the collection of constrainable artefacts that the constraint is attached to.
* cubeRegions - *Array* *optional*. A list of of *[cubeRegion](#cubeRegion)* objects. CubeRegion describes a set of dimension values which define a region and attributes which relate to the region for the purpose of describing a constraint.
* dataKeySets - *Array* *optional*. A list of of *[dataKeySet](#dataKeySet)* objects. DataKeySet defines a collection of full or partial data keys.

Example: 

	{
		"id": "EXR_CONSTRAINTS",
		"version": "1.0.0",
		"agencyID": "ECB",
		"isExternalReference": false,
		"name": "Data constraints for the EXR dataflow",
		"names": {
			"en": "Data constraints for the EXR dataflow",
			"fr": "Constraintes de données pour le dataflow EXR"
		},
		"links": [
			{
				"href": "/constraint/ECB/EXR_CONSTRAINTS/1.0",
				"rel": "self",
				"urn": "urn:sdmx:org.sdmx.infomodel.registry.DataConstraint=ECB:EXR_CONSTRAINTS(1.0)"
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
		]
	}

### availabilityConstraint

*Object*. This type of constraint contains the actual data currently present for the referenced object and is used to express data availability by listing a set of component values (CubeRegion) constructed from a data structure definition. Availability constraints should not be (semantically) versioned.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

In addition, `availabilityConstraint` has the following properties:

* constraintAttachment - *Object* *optional*. The *[constraintAttachment](#constraintAttachment)* object describes the collection of constrainable artefacts that the constraint is attached to.
* cubeRegions - *Array* *optional*. A list of of *[cubeRegion](#cubeRegion)* objects. CubeRegion describes a set of dimension values which define a region and attributes which relate to the region for the purpose of describing a constraint.

Example: 

	{
		"id": "EXR_CONSTRAINTS",
		"version": "1.0.0",
		"agencyID": "ECB",
		"isExternalReference": false,
		"name": "Availability constraints for the EXR dataflow",
		"names": {
			"en": "Availability constraints for the EXR dataflow",
			"fr": "Constraintes de disponibilité pour le dataflow EXR"
		},
		"links": [
			{
				"href": "/constraint/ECB/EXR_CONSTRAINTS/1.0",
				"rel": "self",
				"urn": "urn:sdmx:org.sdmx.infomodel.registry.AvailabilityConstraint=ECB:EXR_CONSTRAINTS(1.0)"
			}
		],
		"constraintAttachment": {
			# constraintAttachment object #
		},
		"cubeRegions": [
			{
				# cubeRegion object #
			}
		]
	}

### metadataConstraint

*Object*. MetadataConstraint specifies a sub set of the definition of the allowable content of a metadata set.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

In addition, `metadataConstraint` has the following properties:

* constraintAttachment - *Object* *optional*. The *[constraintAttachment](#constraintAttachment)* object describes the collection of constrainable artefacts that the constraint is attached to.
* metadataTargetRegions - *Array* *optional*. A list of of *[metadataTargetRegion](#metadataTargetRegion)* objects which describes the values allowed for metadata attributes.

Example: 

	{
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
		]
	}

#### constraintAttachment

*Object*. ConstraintAttachment describes a collection of references to constrainable artefacts.

constraintAttachment properties for DataConstraints:  

* dataProvider - *String* *optional*. DataProvider is a URN reference to a data provider to which the constraint is attached.
* dataStructures - *Array* *optional* of *string*s. URN references to data structure definitions to which the constraint is attached. A constraint which is attached to more than one data structure must only express key sets and/or cube regions where the identifiers of the dimensions are common across all structures to which the constraint is attached. 
* dataflows - *Array* *optional* of *string*s. URN references to data flows to which the constraint is attached. A constraint can be attached to more than one dataflow, and the dataflows do not necessarily have to be usages of the same data structure. However, a constraint which is attached to more than one data structure must only express key sets and/or cube regions where the identifiers of the dimensions are common across all structures to which the constraint is attached. 
* provisionAgreements - *Array* *optional* of *string*s. URN references to provision agreements to which the constraint is attached. A constraint can be attached to more than one data provision agreement, and the data provision agreements do not necessarily have to be references structure usages based on the same structure. However, a constraint which is attached to more than one data provision agreement must only express key sets and/or cube/target regions where the identifier of the components are common across all structures to which the constraint is attached. 

constraintAttachment properties for MetadataConstraints:  

* metadataProvider - *String* *optional*. MetadataProvider is a URN reference to a metadata provider to which the constraint is attached.
* metadataStructures - *Array* *optional* of *string*s. URN references to metadata structure definitions to which the constraint is attached. A constraint which is attached to more than one metadata structure must only express key sets and/or target regions where the identifiers of the target objects are common across all structures to which the constraint is attached. 
* metadataflows - *Array* *optional* of *string*s. URN references to metadata flows to which the constraint is attached. A constraint can be attached to more than one metadataflow, and the metadataflows do not necessarily have to be usages of the same metadata structure. However, a constraint which is attached to more than one metadata structure must only express key sets and/or target regions where the identifiers of the target objects are common across all structures to which the constraint is attached. 
* metadataProvisionAgreements - *Array* *optional* of *string*s. URN references to metadata provision agreements to which the constraint is attached. A constraint can be attached to more than one metadata provision agreement, and the metadata provision agreements do not necessarily have to be references structure usages based on the same structure. However, a constraint which is attached to more than one metadata provision agreement must only express key sets and/or cube/target regions where the identifier of the components are common across all structures to which the constraint is attached. 

Examples:

	{
		"dataProvider": "urn:sdmx:org.sdmx.infomodel...."
	}
	
	{
		"dataStructures": [
			"urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)"
		]
	}
	
	{
		"dataflows": [
			"urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)"
		]
	}
	
	{
		"provisionAgreements": [
			"urn:sdmx:org.sdmx.infomodel...."
		]
	}

	{
		"metadataProvider": "urn:sdmx:org.sdmx.infomodel...."
	}
	
	
	{
		"metadataStructures": [
			"urn:sdmx:org.sdmx.infomodel.metadatastructure.MetadataStructure=ECB:ECB_EXR1_M(1.0)"
		]
	}
	
	{
		"metadataflows": [
			"urn:sdmx:org.sdmx.infomodel.metadatastructure.Metadataflow=ECB:EXR_M(1.0)"
		]
	}
	
	{
		"metadataProvisionAgreements": [
			"urn:sdmx:org.sdmx.infomodel...."
		]
	}


#### cubeRegion

*Object*. CubeRegion defines the structure of a data cube region. This is based on the abstract RegionType and simply refines the key and attribute values to conform with what is applicable for dimensions and attributes, respectively. See the documentation of the base type for more details on how a region is defined.

* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* include - *Boolean* *optional*. Default: `true`. The include attribute indicates that the region is to be included or excluded within the context in which it is defined. For example, if the regions is defined as part of a content constraint, the exclude flag would mean the data identified by the region is not present.
* components - *Array* *optional* of *[ComponentValueSet](#ComponentValueSet)* objects containing a reference to a component (data attribute, metadata attribute, or measure) and providing a collection of values for the referenced component. This serves to state that for the key which defines the region, the components that are specified here have or do not have (depending on the include attribute of the value set) the values provided. It is possible to provide a component reference without specifying values, for the purpose of stating the component is absent (include = false) or present with an unbounded set of values. As opposed to key components, which are assumed to be wild carded if absent, no assumptions are made about the absence of a component. Only components which are explicitly stated to be present or absent from the region will be know. All unstated components for the set cannot be assumed to absent or present.
* keyValues - *Array* *optional* of *[CubeRegionKey](#CubeRegionKey)* objects containing a reference to a component which disambiguates the data (i.e. a dimension) and providing a collection of values for the component. The collection of values can be flagged as being inclusive or exclusive to the region being defined. Any key component that is not included is assumed to be wild carded, which is to say that the cube includes all possible values for the un-referenced key components. Further, this assumption applies to the values of the components as well. The values for any given component can only be sub-setted in the region by explicit inclusion or exclusion. For example, a dimension X which has the possible values of 1, 2, 3 is assumed to have all of these values if a key value is not defined. If a key value is defined with an inclusion attribute of true and the values of 1 and 2, the only the values of 1 and 2 for dimension X are included in the definition of the region. If the key value is defined with an inclusion attribute of false and the value of 1, then the values of 2 and 3 for dimension X are included in the definition of the region. Note that any given key component must only be referenced once in the region. 

Example:

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
		
##### ComponentValueSet

*Object*. ComponentValueSet defines the structure for providing values for a data attributes, measures, or metadata attributes. If no values are provided, the component is implied to include/excluded from the region in which it is defined, with no regard to the value of the component. Note that for metadata attributes which occur within other metadata attributes, a nested identifier can be provided. For example, a value of CONTACT.ADDRESS.STREET refers to the metadata attribute with the identifier STREET which exists in the ADDRESS metadata attribute in the CONTACT metadata attribute, which is defined at the root of the report structure. 

* id - *String*. 
* include - *Boolean* *optional*. The include attribute indicates whether the values provided for the referenced component are to be included are excluded from the region in which they are defined.
* removePrefix - *Boolean* *optional*. The removePrefix attribute indicates whether codes should keep or not the prefix, as defined in the extension of codelist.
* timeRange - *Object* *optional*. A *[TimeRangeValue](#TimeRangeValue)* object.
* values - Non-empty *array* *optional* of *String*s and *[SimpleComponentValue](#SimpleComponentValue)* objects.
Only one of timeRange or values properties is allowed.


Example:

	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"values": [
			"NRP0", "NN00", "NRD0", "NRU1", 
			# SimpleComponentValue object #
		]
	}
	
	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"timeRange": {
			# TimeRangeValue object #
		}
	}

###### TimeRangeValue

*Object*. TimeRangeValue allows a time period value to be expressed as a range. It can be expressed as the period before a period, after a period, or between two periods. Each of these properties can specify their inclusion in regards to the range.

* afterPeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. AfterPeriod is the period after which the period is meant to cover. This date may be inclusive or exclusive in the range.
* beforePeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. BeforePeriod is the period before which the period is meant to cover. This date may be inclusive or exclusive in the range.
* endPeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. EndPeriod is the end period of the range. This date may be inclusive or exclusive in the range.
* startPeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. StartPeriod is the start date or the range that the queried date must occur within. This date may be inclusive or exclusive in the range.
* validFrom - *String* *optional*. A timestamp from which the time range is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the time range is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.

Example:

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
		},
		"validFrom": "2021-09-01",
		"validTo":"2021-09-30"
	}

###### TimePeriodRange

*Object*. TimePeriodRange defines a time period, and indicates whether it is inclusive in a range.

* period - *String* *optional*. Specifies a distinct time period or point in time in SDMX. The time period can either be a Gregorian calendar period, a standard reporting period, a distinct point in time, or a time range with a specific date and duration.
* isInclusive - *Boolean* *optional*.

Example:

	{
		"period": "2018",
		"isInclusive": true
	}

###### SimpleComponentValue

*Object*. SimpleComponentValue contains a simple value for a component, and if that value is from a code list, the ability to indicate that child codes in a simple hierarchy are part of the value set of the component for the region.

* value - *String*. 
* lang - *String* *optional*. IETF Language Tag according to [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for specifying locals in HTTP - *String*. The localised name.
* cascadeValues - *Boolean* *optional* or constant *string* `excluderoot`. CascadeValues identifies if the value should be cascaded.
* validFrom - *String* *optional*. A timestamp from which the value is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the value is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.

Example:

	{
		"value": "NRP0",
		"lang": "en",
		"cascadeValues": "excluderoot",
		"validFrom": "2021-09-01",
		"validTo":"2021-09-30"
	}

##### CubeRegionKey

*Object*. CubeRegionKey provids a set of values for a dimension for the purpose of defining a data cube region. A set of distinct value can be provided, or if this dimension is represented as time, and time range can be specified. 

* id - *String*. 
* include - *Boolean* *optional*. The include attribute indicates whether the values provided for the referenced component are to be included are excluded from the region in which they are defined.
* removePrefix - *Boolean* *optional*. The removePrefix attribute indicates whether codes should keep or not the prefix, as defined in the extension of codelist.
* validFrom - *String* *optional*. A timestamp from which the set of values is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the set of values is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* timeRange - *Object* *optional*. A *[TimeRangeValue](#TimeRangeValue)* object.
* values - Non-empty *array* *optional* of *String*s and *[SimpleComponentValue](#SimpleComponentValue)* objects (but not allowing for a language-specification).
Only one of timeRange or values properties is allowed.


Example:

	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"validFrom": "2021-09-01",
		"validTo":"2021-09-30",
		"values": [
			"NRP0", "NN00", "NRD0", "NRU1",
			# SimpleComponentValue object #
		]
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

#### dataKeySet

*Object*. dataKeySet defines a collection of full or partial data keys (dimension values).

* isIncluded - *Boolean*.
* keys - Non-empty *array* of *[dataKey](#dataKey)* objects. Data Key contains a set of dimension values which identify a full set of data.

Example:

	{
		"isIncluded": true,
		"keys": [
			{
				# DataKey object #
			}
		]
	}

##### dataKey

*Object*. DataKey is a region which defines a distinct full or partial data key. The key consists of a set of values, each referencing a dimension and providing a single value for that dimension. The purpose of the key is to define a subset of a data set (i.e. the observed value and data attribute) which have the dimension values provided in this definition. Any dimension not stated explicitly in this key is assumed to be wild carded, thus allowing for the definition of partial data keys.

* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* include - *Boolean*. Fixed to `true`.
* validFrom - *String* *optional*. A timestamp from which the region is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the region is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* keyValues - Non-empty *array* of *[dataKeyValue](#dataKeyValue)* objects.
* components - Non-empty *array* of *[dataComponentValueSet](#dataComponentValueSet)* objects.

Example:

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

###### dataKeyValue

*Object*. DataKeyValue provides dimension value(s) for the purpose of defining one or more data keys. 

* id - *String*. 
* include - *Boolean* *optional*. Fixed to `true`.
* removePrefix - *Boolean* *optional*. The removePrefix attribute indicates whether codes should keep or not the prefix, as defined in the extension of codelist.
* value - *String*. Deprecated. For backward-compatibility, used when only one dimension value is specified.
* values - Non-empty *array* of *String*s for dimension values.

Examples:

	{
		"id": "EXR_TYPE",
		"include": true,
		"value": "NRP0"
	}

	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"values": ["NRP0"]
	}


###### dataComponentValueSet

*Object*. dataComponentValueSet defines the structure for providing values for a data attributes, measures, or metadata attributes. If no values are provided, the component is implied to include/excluded from the region in which it is defined, with no regard to the value of the component. Note that for metadata attributes which occur within other metadata attributes, a nested identifier can be provided. For example, a value of CONTACT.ADDRESS.STREET refers to the metadata attribute with the identifier STREET which exists in the ADDRESS metadata attribute in the CONTACT metadata attribute, which is defined at the root of the report structure.

* id - *String*. 
* include - *Boolean* *optional*. The include attribute indicates whether the values provided for the referenced component are to be included are excluded from the region in which they are defined.
* removePrefix - *Boolean* *optional*. The removePrefix attribute indicates whether codes should keep or not the prefix, as defined in the extension of codelist.
* timeRange - *Object* *optional*. A *[TimeRangeValue](#TimeRangeValue)* object.
* values - Non-empty *array* *optional* of *String*s and *[DataComponentValue](#DataComponentValue)* objects.
Only one of timeRange or values properties is allowed.

Example:

	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"values": [
			"DIM",
			# DataComponentValue object #
		]
	}
	
	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"timeRange": {
			# TimeRangeValue object #
		}
	}	

###### DataComponentValue

*Object*. DataComponentValue contains a simple value for a component, and if that value is from a code list, the ability to indicate that child codes in a simple hierarchy are part of the value set of the component for the region.

* cascadeValues - *Boolean* *optional* or constant *string* `excluderoot`. CascadeValues identifies if the value should be cascaded.
* lang - *String* *optional*. IETF Language Tag according to [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for specifying locals in HTTP - *String*. The localised name.
* value - *String*. 

Example:

	{
		"cascadeValues": "excluderoot",
		"lang": "en",
		"value": "NRP0"
	}

#### metadataTargetRegion

*Object*. metadataTargetRegion defines the structure of a metadata target region. A metadata target region must define the report structure and the metadata target from that structure on which the region is based. This type is based on the abstract RegionType and simply refines the key and attribute values to conform with what is applicable for target objects and metadata attributes, respectively. See the documentation of the base type for more details on how a region is defined.

* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* include - *Boolean* *optional*. The include attribute indicates that the region is to be included or excluded within the context in which it is defined. For example, if the regions is defined as part of a content constraint, the exclude flag would mean the data identified by the region is not present.
* components - *Array* *optional* of *[MetadataAttributeValueSet](#MetadataAttributeValueSet)* objects containing a reference to a component (data attribute, metadata attribute, or measure) and provides a collection of values for the referenced component. This serves to state that for the key which defines the region, the components that are specified here have or do not have (depending on the include attribute of the value set) the values provided. It is possible to provide a component reference without specifying values, for the purpose of stating the component is absent (include = false) or present with an unbounded set of values. As opposed to key components, which are assumed to be wild carded if absent, no assumptions are made about the absence of a component. Only components which are explicitly stated to be present or absent from the region will be know. All unstated components for the set cannot be assumed to absent or present.
* validFrom - *String* *optional*. A timestamp from which the region is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the region is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.

Example:

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
		
##### MetadataAttributeValueSet

*Object*. MetadataAttributeValueSet defines the structure for providing values for a metadata attribute. If no values are provided, the attribute is implied to include/excluded from the region in which it is defined, with no regard to the value of the metadata attribute. Note that for metadata attributes which occur within other metadata attributes, a nested identifier can be provided. For example, a value of CONTACT.ADDRESS.STREET refers to the metadata attribute with the identifier STREET which exists in the ADDRESS metadata attribute in the CONTACT metadata attribute, which is defined at the root of the report structure. 

* id - *String*. 
* include - *Boolean* *optional*. The include attribute indicates whether the values provided for the referenced component are to be included are excluded from the region in which they are defined.
* removePrefix - *Boolean* *optional*. The removePrefix attribute indicates whether codes should keep or not the prefix, as defined in the extension of codelist.
* timeRange - *Object* *optional*. A *[TimeRangeValue](#TimeRangeValue)* object.
* values - Non-empty *array* *optional* of *String*s and *[SimpleComponentValue](#SimpleComponentValue)* objects.
Only one of timeRange or values properties is allowed.


Example:

	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"values": [
			"NRP0", "NN00", "NRD0", "NRU1", 
			# SimpleComponentValue object #
		]
	}
	
	{
		"id": "EXR_TYPE",
		"include": true,
		"removePrefix": false,
		"timeRange": {
			# TimeRangeValue object #
		}
	}

### customTypeScheme

*Object*. CustomTypeScheme provides the details of a custom type scheme, in which user defined operators are described.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### vtlMappingScheme

*Object*. VtlMappingScheme provides the details of a VTL mapping scheme, in which VTL mappings are described.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### namePersonalisationScheme

*Object*. NamePersonalisationScheme provides the details of a name personalisation scheme, in which name personalisations are described.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### rulesetScheme

*Object*. RulesetScheme provides the details of a ruleset scheme, in which rulesets are described.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### transformationScheme

*Object*. TransformationScheme provides the details of a transformation scheme, in which transformations are described.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### userDefinedOperatorScheme

*Object*. UserDefinedOperatorScheme provides the details of a user defined operator scheme, in which user defined operators are described.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

## statusInformation

*Object* *optional*. Used to provide status information with more detail to the HTTP status codes in the RESTful SDMX web service responses. The following pieces of information should be provided:

* code - *Number*. Provides an HTTP status code number for the status information if appropriate. See [RESTful SDMX web service error handling and status information](https://github.com/sdmx-twg/sdmx-rest/blob/master/doc/status.md).
* title - *String* *optional*. A short, human-readable (best-language-match) summary of the situation that SHOULD NOT change from occurrence to occurrence of the status, except for purposes of localization.
* titles - *Object* *optional*. A list of short, human-readable localised summaries (see *[names](#names)*) of the status that SHOULD NOT change from occurrence to occurrence of the status, except for purposes of localization.
* detail - *String* *optional*. A human-readable (best-language-match) explanation specific to this occurrence of the status. Like title, this field’s value can be localized. It is fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the status.
* details - *Object* *optional*. A list of human-readable localised explanations (see *[names](#names)*) specific to this occurrence of the status. Like titles, this field’s value can be localized. It is fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the status.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional external resources for the status information.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	{
		"code": 400,
		"title": "Invalid number of items in the item parameter",
		"titles": {
			"en": "Invalid number of items in the item parameter",
			"fr": "Nombre invalide d'items dans le paramètre 'item'"
		}
	}

# Linking mechanism

## link

*Object* *optional*. A link to an external resource.

* href - *String* *optional* only if `urn` is present. Absolute or relative URL of the external resource.
* rel - *String*. Relationship of the object to the external resource. See semantics below.
* urn - *String* *optional* only if `href` is present. The `urn` holds a valid SDMX Registry URN (see SDMX Registry Specification for details).
* uri - *String* *optional*. The `uri` attribute holds a URI that contains a link to additional information about the resource, such as a web page. This `uri` is not an SDMX resource.
* title - *String* *optional*. A human-readable (best-language-match) description of the target link.
* titles - *Object* *optional*. A list of human-readable localised descriptions (see *[names](#names)*) of the target link.
* type - *String* *optional*. A hint about the type of representation returned by the link.
* hreflang - *String* *optional*. The natural language of the external link, the same as used in the HTTP Accept-Language request header.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Examples:

	{
		"href": "https://registry.sdmx.org/ws/rest/datastructure/ECB/ECB_EXR1",
		"rel": "self",
		"uri": "http://www.xyz.org/pdf/0123456789"
	}

	{
		"href": "https://registry.sdmx.org/ws/rest/dataflow/ECB.DISS/BSI_PUB/1.0",
		"rel": "dataflow",
		"urn": "urn:sdmx:org.sdmx.infomodel.datastructure.dataflow=ECB.DISS:BSI_PUB(1.0)"
	}

	{
		"href": "https://registry.sdmx.org/FusionRegistry/ws/rest/provisionagreement/ESTAT/PA_NAMAIN_IDC_N",
		"rel": "provisionagreement"
	}

	{
		"href": "https://registry.sdmx.org/help.html",
		"rel": "help",
		"title": "Documentation about the SDMX Global Registry",
		"titles": {
			"en": "Documentation about the SDMX Global Registry",
			"fr": "Documentation concernant le 'SDMX Global Registry'"
		},
		"type": "text/html",
		"hreflang": "en"
	}

Collections of links can be attached to various elements in SDMX-JSON.

Similarily with standards such as HTML5 and Atom, link elements in SDMX-JSON *must* define a *URL* (the `href` attribute) and a *semantic* (the `rel` attribute). This allows clients to follow the links they care about and ignore the ones whose semantic they are not interested in. In addition, links in SDMX-JSON *may* define a `title` (a human-friendly description of the target link) and a `type` (a hint about the type of representation returned by the link). Please refer to the [list of Media Types and Subtypes](http://www.iana.org/assignments/media-types/media-types.xhtml) assigned and listed by the IANA for additional information about expected values for the `type` attribute.

SDMX-JSON offers a list of predefined semantics, but implementers are free to extend it. The list of predefined semantics comes from the list of SDMX artefacts that can be returned by SDMX RESTful web services, semantics defined in [RFC5988](https://tools.ietf.org/rfc/rfc5988.txt) and additional items deemed to be useful in the context of statistical data dissemination. These semantics are:

  - SDMX artefacts: dataStructure, metadataStructure, categoryScheme, conceptScheme, codelist, hierarchicalCodelist, organisationScheme, agencyScheme, dataProviderScheme, dataConsumerScheme, organisationUnitScheme, dataflow, metadataflow, reportingTaxonomy, provisionAgreement, structureSet, process, categorisation, contentconstraint, attachmentconstraint, category, concept, code, organisation, agency, dataProvider, dataConsumer, organisationUnit, reportingCategory, Data
  - RFC5988: alternate, copyright, glossary, help, index.
  - Miscellaneous: calendar (link to a release calendar), source (information about the source of data), request (the SDMX RESTful query that triggered the SDMX-JSON response).

The *URL* captured in the `href` attribute can be *absolute* or *relative*. **It is recommended to use absolute URLs in case the SDMX-JSON message is archived.**

# Localised text elements

**Localised best-language-match text strings (static properties matched through "Lookup"):**

The first best language match according to the user’s preferred language choices expressed through the HTTP content negotiation (Accept-Language header parameter) or the system-default language, if there is no preferred language or no language match, is to be provided for each localised text element. In any case, the message does not indicate the returned language per localised text element.

This language matching type is called "Lookup", see <https://tools.ietf.org/html/rfc4647#section-3.4>.

Example:

	"name": "Frequency"

**Localised text objects (variable properties matched through "Filtering"):**

Optionally, all available language matches according to the user’s preferred language choices expressed through the HTTP content negotiation (Accept-Language header parameter) can be provided for each localised text element. 

This language matching type is called "Filtering", see <https://tools.ietf.org/html/rfc4647#section-3.3>.

Example:

	"names": {
		"en": "Frequency",
		"fr": "Fréquence"
	}

In case that there is no language match for a particular localisable element, it is optional to:
- return the element in a system-default language or alternatively to not return the element
- indicate available alternative languages for the element's maintainable artefact through links to underlying localised resources

The localised text object must be present and complete whenever the user’s preferred language choice is `*` (wildcard for any language). This mechanism allows transmitting of structures with their complete content (all localised elements and the knowledge about the locale) to a registry or to other databases, thus in contexts where all language versions are required or where the information on the language used is required.

**It is recommended to indicate all languages used anywhere in the message for localised elements through http Content-Language response header (languages of the intended audience) and/or through a “contentLanguages” property in the meta tag.** The main language used can be indicated through the “lang” property in the meta tag.

**In case one or more specific languages were requested through the HTTP header “Accept-Language" in a GET query and the response message might not contain the complete set of all available languages, then in the response message the SDMX artefact's property `isPartialLanguage` is to be set to `true`.** Submitting such a partial metadataset to update an SDMX storage system will only add or update the included languages but not change other languages. 

# Security Considerations

This document defines a response format for SDMX RESTful Web Services in JSON
and it raises no new security considerations. SDMX Web Services Guidelines
includes the security considerations associated with its usage.


# Extending SDMX-JSON

The objects defined in SDMX-JSON are "open", i.e. they can be extended with properties not defined in this specification. Providers of SDMX-JSON messages are therefore welcome to add support for features not (yet) covered in this specification. Whenever appropriate, providers who opt to do so are invited to inform us, so that future versions of SDMX-JSON may integrate these extensions, thereby improving interoperability.

However, in order to allow JSON validators to properly check for and recognize syntax errors instead of interpreting them as extensions, all extension keywords must be prefixed with "x-". Any keyword that is neither a standard keyword nor begins with "x-" is in violation of the specification and should cause validation of the SDMX-JSON document to fail.

The snippet below shows an example of an `error` object, extended with a `x-CustomErrorCode`:

```json
	"errors": [
		{
			"code": 400,
			"title": "Invalid number of items in the item parameter",
			"titles": {
				"en": "Invalid number of items in the item parameter",
				"fr": "Nombre invalide d'items dans le paramètre 'item'"
			},
			"x-CustomErrorCode": 39272
		}
	]
```
