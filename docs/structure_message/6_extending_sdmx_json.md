# Extending SDMX-JSON

The objects defined in SDMX-JSON are "open", i.e. they can be extended with
properties not defined in this specification. Providers of SDMX-JSON messages
are therefore welcome to add support for features not covered in this
specification. Whenever appropriate, providers who opt to do so are invited to
inform us, so that future versions of SDMX-JSON may integrate these extensions,
thereby improving interoperability.

The snippet below shows an example of an `error` object, extended with a
`wsCustomErrorCode`:

```json
"errors": [
    {
        "code": 150,
        "title": "Invalid number of items in the item parameter",
        "titles": {
            "en": "Invalid number of items in the item parameter",
            "fr": "Nombre invalide d'items dans le paramètre 'item'"
        }
        "wsCustomErrorCode": 39272
    }
]
```
