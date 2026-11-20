# Field Guide to SDMX-JSON 2.2 Registry Message Objects (aligned with SDMX 3.2.0)

## message

Message is the top level object and it contains the requested information
(data and reference metadata registrations) as well as the meta-information
describing the (technical) context of the message and, possibly, status
information.

- $schema - *String* *optional*. Contains the URL to the schema allowing to
  validate the message. This also allows identifying the version of SDMX-JSON
  format used in this message. **Providing the link to the SDMX-JSON schema is
  recommended.**
- meta - *Object*. A *meta* object that contains non-standard meta-information
  and basic technical information about the message, such as when it was
  prepared and who has sent it. It is the same as in the other SDMX-JSON
  messages, see *[meta](../metadata_message/1_field_guide.md#meta)*.
- data - *Object* *optional*. *[Data](#data)* contains the message's “primary
  data”, i.e. the registrations.
- errors - *Array* *optional* of *statusInformation* objects providing - when
  appropriate - more detail to the HTTP status codes in the RESTful SDMX web
  service responses, as in the other SDMX-JSON messages (see
  *[statusInformation](../metadata_message/1_field_guide.md#statusinformation)*).

The properties data and errors CAN coexist in the same message.

??? example

    ```json
    {
      "$schema": "https://json.sdmx.org/2.2/sdmx-json-registry-schema.json",
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

## data

*Object* *optional*. Contains the message's “primary data”.

- dataRegistrations - *Array* *optional*. A collection of
  *[data registrations](#dataregistration)*.
- metadataRegistrations - *Array* *optional*. A collection of
  *[metadata registrations](#metadataregistration)*.

## dataRegistration

*Object*. A data registration provides the information on where the registered
data can be retrieved. The registered data must be associated with a provision
agreement, a dataflow, or a data structure definition: exactly one of the three
properties below is provided. If possible, the provision agreement should be
specified. Only in cases where this is not possible should the dataflow or data
structure definition be used.

- id - *String* *optional*. Registry assigned identification for the
  registration. It must be provided in a response message (unless an error
  occurred while submitting a new registration), and should be included when
  updating or deleting a registration.
- lastUpdated - *String* *optional*. Timestamp for the last time the data
  source was updated. Values must follow the ISO 8601 syntax for combined dates
  and times, including time zone.
- provisionAgreement - *String* *optional*. URN of the provision agreement that
  the data are registered against.
- dataflow - *String* *optional*. URN of the dataflow that the data are
  registered against.
- dataStructure - *String* *optional*. URN of the data structure definition that
  the data are registered against.
- datasource - *Object*. The *[data source](#datasource)* from which the
  registered data can be retrieved.

??? example

    ```json
    {
      "id": "REG_EXR_1",
      "lastUpdated": "2026-11-01T08:30:00Z",
      "provisionAgreement": "urn:sdmx:org.sdmx.infomodel.registry.ProvisionAgreement=ECB:PA_EXR(1.0.0)",
      "datasource": {
        "sdmxApis": [
          {
            "entryPoint": "https://sdmx.example.org/rest/v2",
            "sdmxApiVersion": "2.2"
          }
        ]
      }
    }
    ```

### datasource

*Object*. Specifies where the registered data can be retrieved: either one or
more files, or one or more SDMX REST APIs, but not both.

- files - *Array* *optional*. One or more *[files](#file)*. Each file must have a
  distinct url. When several files are provided, for example the same content
  as SDMX-CSV and as SDMX-JSON, each must have a format, and the formats must be
  distinct.
- sdmxApis - *Array* *optional*. One or more web services compliant with the
  *[SDMX REST API](#sdmxapi)*. Each must have a distinct combination of entry
  point and SDMX REST API version, for example the same service for two
  versions of the API.

??? example

    ```json
    "datasource": {
      "files": [
        {
          "url": "https://sdmx.example.org/files/icp-2026-10.csv",
          "format": "application/vnd.sdmx.data+csv;version=2.1.0"
        },
        {
          "url": "https://sdmx.example.org/files/icp-2026-10.json",
          "format": "application/vnd.sdmx.data+json;version=2.1.0"
        }
      ]
    }
    ```

### file

*Object*. Describes a static file, in terms of its URL and its format.

- url - *String*. The URL of the file.
- format - *String* *optional*. The SDMX media type of the file, using the same
  syntax as the SDMX REST API HTTP Accept header, in the form
  `application/vnd.sdmx.data+{format};version={major.minor.patch}`, where
  format is one of `json`, `xml`, `csv` or `parquet` (e.g.
  `application/vnd.sdmx.data+csv;version=2.1.0`). For reference metadata files,
  the form is `application/vnd.sdmx.metadata+{format};version={major.minor.patch}`,
  where format is one of `json`, `xml` or `csv`. The format is optional for
  compatibility with existing registrations, but required when several files
  are provided.

### sdmxApi

*Object*. Describes a web service compliant with the SDMX REST API.

- entryPoint - *String*. The URL of the entry point of the SDMX REST web
  service (e.g. `https://sdmx.example.org/rest`). It cannot be empty.
- sdmxApiVersion - *String*. The version of the SDMX REST API supported at this
  entry point, which determines how queries are formulated, expressed as
  major.minor, i.e. the code of a release of the SDMX REST API without the
  leading "v" and the patch number (e.g. `1.5`, `2.1` or `2.2`, from the
  releases v1.5.0, v2.1.0 and v2.2.2).
- specificationURL - *String* *optional*. The URL of a machine-readable
  description of the service, such as an OpenAPI specification.

??? example

    ```json
    "sdmxApis": [
      {
        "entryPoint": "https://sdmx.example.org/rest/v1",
        "sdmxApiVersion": "1.5"
      },
      {
        "entryPoint": "https://sdmx.example.org/rest/v2",
        "sdmxApiVersion": "2.2",
        "specificationURL": "https://sdmx.example.org/rest/v2/openapi.json"
      }
    ]
    ```

## metadataRegistration

*Object*. A metadata registration provides the information on where the
registered reference metadata can be retrieved. The registered metadata must be
associated with a metadata provision agreement, a metadataflow, or a metadata
structure definition: exactly one of the three properties below is provided. If
possible, the metadata provision agreement should be specified. Only in cases
where this is not possible should the metadataflow or metadata structure
definition be used.

- id - *String* *optional*. Registry assigned identification for the
  registration, as for *[data registrations](#dataregistration)*.
- lastUpdated - *String* *optional*. Timestamp for the last time the metadata
  source was updated. Values must follow the ISO 8601 syntax for combined dates
  and times, including time zone.
- metadataProvisionAgreement - *String* *optional*. URN of the metadata
  provision agreement that the reference metadata are registered against.
- metadataflow - *String* *optional*. URN of the metadataflow that the reference
  metadata are registered against.
- metadataStructure - *String* *optional*. URN of the metadata structure
  definition that the reference metadata are registered against.
- metadataSource - *Object*. Where the registered reference metadata can be
  retrieved, with the same properties as the *[data source](#datasource)*:
  either one or more *[files](#file)* (with reference metadata media types), or
  one or more *[SDMX REST APIs](#sdmxapi)*.

??? example

    ```json
    {
      "id": "REG_QUALITY_1",
      "lastUpdated": "2026-11-10T09:00:00Z",
      "metadataProvisionAgreement": "urn:sdmx:org.sdmx.infomodel.registry.MetadataProvisionAgreement=ECB:QUALITY_ECB(1.0.0)",
      "metadataSource": {
        "files": [
          {
            "url": "https://sdmx.example.org/files/quality-2026.json",
            "format": "application/vnd.sdmx.metadata+json;version=2.1.0"
          }
        ]
      }
    }
    ```
