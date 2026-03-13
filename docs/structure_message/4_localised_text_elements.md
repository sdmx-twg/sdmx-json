# Localised text elements

**Localised best-language-match text strings (static properties matched through
"Lookup"):**

The first best language match according to the user’s preferred language choices
expressed through the HTTP content negotiation (Accept-Language header
parameter) or the system-default language, if there is no preferred language or
no language match, is to be provided for each localised text element. In any
case, the message does not indicate the returned language per localised text
element.

This language matching type is called "Lookup", see
<https://tools.ietf.org/html/rfc4647#section-3.4>.

??? example

    ```json
    "name": "Frequency"
    ```

**Localised text objects (variable properties matched through "Filtering"):**

All available language matches according to the user’s preferred language
choices expressed through the HTTP content negotiation (Accept-Language header
parameter) is to be provided for each localised text element.

This language matching type is called "Filtering", see
<https://tools.ietf.org/html/rfc4647#section-3.3>.

??? example

    ```json
    "names": {
        "en": "Frequency",
        "fr": "Fréquence"
    }
    ```

In case that there is no language match for a particular localisable element, it
is optional to:

- return the element in a system-default language or alternatively to not return
  the element
- indicate available alternative languages for the element's maintainable
  artefact through links to underlying localised resources

The localised text object must be present and complete whenever the user’s
preferred language choice is `*` (wildcard for any language). This mechanism
allows transmitting of structures with their complete content (all localised
elements and the knowledge about the locale) to a registry or to other
databases, thus in contexts where all language versions are required or where
the information on the language used is required.

**It is recommended to indicate all languages used anywhere in the message for
localised elements through http Content-Language response header (languages of
the intended audience) and/or through a “contentLanguages” property in the meta
tag.** The main language used can be indicated through the “lang” property in
the meta tag.

**In case one or more specific languages were requested through the HTTP header
“Accept-Language" in a GET query and the response message might not contain the
complete set of all available languages, then in the response message the SDMX
artefact's property `isPartialLanguage` is to be set to `true`.** Submitting
such a partial metadataset to update an SDMX storage system will only add or
update the included languages but not change other languages.
