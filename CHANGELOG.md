Release notes
=============

This document describes all the changes made to the *Courses API* document,
starting from its first beta draft version.


0.5.0
-----

* Academic Term data type is now in `stable-v1` namespace.

* Added an optional `<ounit-id>` link ([more information]
  (https://github.com/erasmus-without-paper/ewp-specs-api-courses/issues/11)).

* Introduced limits on LOS and LOI IDs ([more information]
  (https://github.com/erasmus-without-paper/general-issues/issues/23)).

* `minOccurs` and `maxOccurs` are now provided explicitly ([why?]
  (https://github.com/erasmus-without-paper/general-issues/issues/22)).


0.4.0
-----

* New `los_code` parameter was added to allow requesters searching for LOSes
  by their codes (the server is now REQUIRED to support this parameter).
  Providing `los_id` parameter is no longer required, if the requester provides
  the `los_code` parameter instead. This is a part of a larger change in all
  APIs described [here]
  (https://github.com/erasmus-without-paper/general-issues/issues/21).

* New optional element: `<eqfLevelProvided>`. See [this issue]
  (https://github.com/erasmus-without-paper/ewp-specs-api-courses/issues/8).


0.3.0
-----

Major changes in the format of LOS and LOI IDs:

* New `LosID` and `LoiID` simpleTypes were introduced.
* EWP now requires partners to use a predefined set of prefixes for these IDs.
* New `<los-code>` elements were introduced to support servers in providing
  separate surrogate and natural/business keys for all LOSes.

For discussion and reasoning, see [this thread]
(https://github.com/erasmus-without-paper/ewp-specs-api-mobilities/issues/9).


0.2.1
-----

* Add `<gradingScheme>` to `response-example.xml` too.


0.2.0
-----

* `<gradingScheme>` element has been introduced ([issue link]
  (https://github.com/erasmus-without-paper/ewp-specs-api-courses/issues/5)).


0.1.0
-----

Initial release.
