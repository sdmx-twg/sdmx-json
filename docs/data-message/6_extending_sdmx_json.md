# Extending SDMX-JSON

The objects defined in SDMX-JSON are "open", i.e. they can be extended by implementers with properties not defined in this specification. Providers of SDMX-JSON messages are therefore welcome to add support for features not covered in this specification. Whenever appropriate, providers who opt to do so are invited to inform us, so that future versions of SDMX-JSON may integrate these extensions, thereby improving interoperability.

The snippet below shows an example of an `error` object, extended with a `wsCustomErrorCode`:

```json
	"errors": [
		{
			"code": 150,
			"title": "Invalid number of dimensions in the key parameter",
			"titles": { "en": "Invalid number of dimensions in the key parameter" },
			"wsCustomErrorCode": 39272
		}
	]
```