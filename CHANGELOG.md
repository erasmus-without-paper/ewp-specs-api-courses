Release notes
=============

This document describes all the changes made to the *Courses API* document,
starting from its first beta draft version.


0.6.1
-----

* Make `los-code`s explicitly recommended.
* Fix some misspellings.
* Fix schemaLocations.
* Explicitly declare that this version still requires the use of
  [Version 1](https://github.com/erasmus-without-paper/ewp-specs-sec-intro/tree/stable-v1)
  of the *Authentication and Security* document. You can find more information
  on the planned process of updating security requirements
  [here](https://github.com/erasmus-without-paper/ewp-specs-sec-intro/issues/1).


0.6.0
-----

* Changed XML namespaces (as part of
  [this issue](https://github.com/erasmus-without-paper/ewp-specs-api-iias/issues/22)).
  Since this version, Courses API is in the `stable-v1` XML namespace.

  This does not means that Courses API is stable. It can still change in
  backward-incompatible ways, until version `1.0.0` is released.

* The `url` is not `maxOccurs="unbounded"`, and it may have an `xml:lang`
  attribute. See [here](https://github.com/erasmus-without-paper/ewp-specs-api-courses/issues/13).


0.5.1
-----

* Fixed annotations. The `<languageOfInstruction>` element is of `xs:language`
  type, so it's not "ISO 639-1 code" (as it has been mentioned in the
  annotations), but rather "BCP 47 code". (This is different, because BCP 47
  codes may contain various subtags.)


0.5.0
-----

* Academic Term data type is now in `stable-v1` namespace.

* Added an optional `<ounit-id>` link
  ([more information](https://github.com/erasmus-without-paper/ewp-specs-api-courses/issues/11)).

* Introduced limits on LOS and LOI IDs
  ([more information](https://github.com/erasmus-without-paper/general-issues/issues/23)).

* `minOccurs` and `maxOccurs` are now provided explicitly
  ([why?](https://github.com/erasmus-without-paper/general-issues/issues/22)).


0.4.0
-----

* New `los_code` parameter was added to allow requesters searching for LOSes
  by their codes (the server is now REQUIRED to support this parameter).
  Providing `los_id` parameter is no longer required, if the requester provides
  the `los_code` parameter instead. This is a part of a larger change in all
  APIs described
  [here](https://github.com/erasmus-without-paper/general-issues/issues/21).

* New optional element: `<eqfLevelProvided>`. See
  [this issue](https://github.com/erasmus-without-paper/ewp-specs-api-courses/issues/8).


0.3.0
-----

Major changes in the format of LOS and LOI IDs:

* New `LosID` and `LoiID` simpleTypes were introduced.
* EWP now requires partners to use a predefined set of prefixes for these IDs.
* New `<los-code>` elements were introduced to support servers in providing
  separate surrogate and natural/business keys for all LOSes.

For discussion and reasoning, see
[this thread](https://github.com/erasmus-without-paper/ewp-specs-api-mobilities/issues/9).


0.2.1
-----

* Add `<gradingScheme>` to `response-example.xml` too.


0.2.0
-----

* `<gradingScheme>` element has been introduced
  ([issue link](https://github.com/erasmus-without-paper/ewp-specs-api-courses/issues/5)).


0.1.0
-----

Initial release.
