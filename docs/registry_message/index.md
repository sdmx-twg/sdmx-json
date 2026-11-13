# SDMX-JSON Registry Message

As one of the standard SDMX exchange formats, the SDMX-JSON Registry Message is a
JSON (JavaScript Object Notation) based message format for exchanging data
registrations, i.e. the information on where the data registered against a
provision agreement can be retrieved.

The SDMX-JSON Registry Message format 2.2.0 supports the SDMX 3.2 Information
Model (see the Data Provisioning section) and works together with the SDMX
RESTful Web Services API for registration queries (media type
`application/vnd.sdmx.registry+json;version=2.2.0`). It follows the same
paradigm as the other SDMX-JSON messages: a `meta` object with the technical
information about the message, a `data` object with the registrations, and
optional `errors`.

See:
- docs: for the descriptive documentation
- samples: for message samples
- schemas: for the normative JSON schema usable for validation purposes
