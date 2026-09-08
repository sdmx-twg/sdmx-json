# Introduction to SDMX-JSON Structure Message 2.2.0

See the SDMX-JSON Data Message docs for a brief introduction of the SDMX
information model. For additional information on the SDMX information model,
please refer to the [SDMX documentation](../../index.md).

Samples, tools and other SDMX-JSON resources are available in the public
[Github repository](https://github.com/sdmx-twg/sdmx-json).

Before we start, let's clarify a few more things about this guide:

- New fields may be introduced in later versions. Therefore consuming
  applications should tolerate the addition of new fields with ease.
- The ordering of properties in objects is undefined. The properties may appear
  in any order and consuming applications should not rely on any specific
  ordering. It is safe to consider a nulled property and the absence of a
  property as the same thing.
- Not all properties appear in all contexts. For example response with error
  messages may not contain a data property.