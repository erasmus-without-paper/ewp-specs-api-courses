Release notes
=============

This document describes all the changes made to the *Courses API* document,
starting from its first beta draft version.


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
