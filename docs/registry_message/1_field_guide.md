# Field Guide to SDMX-JSON 2.2 Registry Message Objects (aligned with SDMX 3.2.0)

## message

Message is the top level object and it contains the requested information
(data registrations) as well as the meta-information describing the (technical)
context of the message and, possibly, status information.

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

- registrations - *Array* *optional*. A collection of
  *[registrations](#registration)*.

## registration

*Object*. A data registration provides the information on where the data
registered against a provision agreement can be retrieved.

- id - *String* *optional*. Registry assigned identification for the
  registration. It must be provided in a response message (unless an error
  occurred while submitting a new registration), and should be included when
  updating or deleting a registration.
- lastUpdated - *String* *optional*. Timestamp for the last time the data
  source was updated. Values must follow the ISO 8601 syntax for combined dates
  and times, including time zone.
- provisionAgreement - *String*. URN of the provision agreement that the data
  are registered against.
- datasource - *Object*. The *[data source](#datasource)* from which the
  registered data can be retrieved.

??? example

    ```json
    {
      "id": "REG_EXR_1",
      "lastUpdated": "2026-11-01T08:30:00Z",
      "provisionAgreement": "urn:sdmx:org.sdmx.infomodel.registry.ProvisionAgreement=ECB:PA_EXR(1.0.0)",
      "datasource": {
        "sdmxApi": {
          "entryPoint": "https://sdmx.example.org/rest",
          "sdmxApiVersion": "2.2"
        }
      }
    }
    ```

### datasource

*Object*. Specifies the data source. A file, an SDMX REST API, or both must be
specified.

- file - *String* *optional*. The URL of an SDMX data message accessible as a
  file (simple data source).
- sdmxApi - *Object* *optional*. A web service compliant with the
  *[SDMX REST API](#sdmxapi)*, from which the registered data can be queried
  (queryable data source).

### sdmxApi

*Object*. Describes a web service compliant with the SDMX REST API.

- entryPoint - *String*. The URL of the entry point of the SDMX REST web
  service (e.g. `https://sdmx.example.org/rest`). It cannot be empty.
- sdmxApiVersion - *String*. The version of the SDMX REST API implemented by
  the entry point, which determines the syntax of the queries. It is given by
  the code of a release of the SDMX REST API, written as `major.minor`, without
  the leading "v" and the patch number (e.g. `1.5`, `2.1` or `2.2`, from the
  releases v1.5.0, v2.1.0 and v2.2.2).

The same properties describe an SDMX REST web service in the VTL query
extraction of the structure message.

??? example

    ```json
    "datasource": {
      "file": "https://sdmx.example.org/files/icp-2026-10.json",
      "sdmxApi": {
        "entryPoint": "https://sdmx.example.org/rest/v1",
        "sdmxApiVersion": "1.5"
      }
    }
    ```
