# Extending SDMX-JSON

The objects defined in SDMX-JSON are "open", i.e. they can be extended with
properties not defined in this specification. Providers of SDMX-JSON messages
are therefore welcome to add support for features not (yet) covered in this
specification. Whenever appropriate, providers who opt to do so are invited to
inform us, so that future versions of SDMX-JSON may integrate these extensions,
thereby improving interoperability.

However, in order to allow JSON validators to properly check for and recognize
syntax errors instead of interpreting them as extensions, all extension keywords
must be prefixed with "x-". Any keyword that is neither a standard keyword nor
begins with "x-" is in violation of the specification and should cause
validation of the SDMX-JSON document to fail.

The snippet below shows an example of an `error` object, extended with a
`x-CustomErrorCode`:

```json
"errors": [
    {
        "code": 400,
        "title": "Invalid parameter",
        "titles": {
            "en": "Invalid parameter",
            "fr": "Paramètre invalide"
        },
        "x-CustomErrorCode": 39272
    }
]
```
