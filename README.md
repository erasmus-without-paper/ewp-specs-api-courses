Courses API
===========

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This document describes the **Courses API**. This API can be implemented by any
HEI, even it is does not take part in EWP mobility process. It allows other
HEIs to access information on courses conducted in this institution.


Request method
--------------

 * Requests MUST be made with either HTTP GET or HTTP POST method. Servers MUST
   support both these methods. Servers SHOULD reject all other request methods.

 * Clients are advised to use POST when passing large number of parameters
   (servers MAY set a limit on their GET query string length).


Request parameters
------------------

Parameters MUST be provided in the regular `application/x-www-form-urlencoded`
format.


### `hei_id` (required)

ID of the institution at which the courses (referenced in `course_id`
parameters) are conducted. This parameter MUST be required by the server even
if it covers only a single institution.


### `course_id` (repeatable, required)

A list of course identifiers (no more than `<max-course-ids>` items) - IDs of
courses the client wants to retrieve information on.

This parameter is *repeatable*, so the request MAY contain multiple occurrences
of it. The server is REQUIRED to process all of them.

Server implementers provide their own chosen value of `<max-course-ids>` via
their manifest entry (see [manifest-entry.xsd](manifest-entry.xsd)). Clients
SHOULD parse this value (or assume it's equal to `1`).


### `snapshot_at_date` (optional)

A date, in the `YYYY-mm-dd` format. If given, the server MAY attempt to
generate a course description *as it would have looked like at this date*.

When we were designing this API, it was brought to our attention that, in many
computer systems, some attributes of courses may change in time. Some of these
changes are important (e.g. the number of ECTS credits awarded), while other
describe just unimportant details (e.g. bibliography used). This parameter is
a simple way of allowing the client the access to this history (other, more
comprehensive ways, might be designed in the future).

*A note for client developers:* You should be aware, that we do not require the
server developers to support this parameter. Many computer systems may return
only the "freshest" version of the Course description, regardless of the date
passed.

*A note for server developers:* While it is not required to support this
parameter, it is REQUIRED to clearly state *if* you support it (or support it
only partially), in your manifest entry. See [manifest-entry.xsd]
(manifest-entry.xsd) for details.


Permissions
-----------

 * All requests from the EWP Network MUST be allowed to access this API.

 * Additionally, implementers MAY allow this API to be accessed by
   **anonymous** external clients too (without the need of using any client
   certificate).


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply.

 * Invalid (or unknown) `hei_id` values MUST result in a HTTP 400 error
   response.

 * Invalid (unknown) `course_id` values MUST be **ignored**. Servers MUST
   return a valid (HTTP 200) XML response in such cases, but the response will
   simply not contain the information on the unknown `course_id` values. If
   all values are unknown, servers MUST respond with an empty `<response>`
   element. This requirement is true even when `<max-course-ids>` is `1`.

 * If the length of `course_id` list is greater than `<max-course-ids>`,
   servers MUST respond with HTTP 400.


Response
--------

Servers MUST respond with a valid XML document described by the [response.xsd]
(response.xsd) schema. See the schema annotations for further information.


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[echo]: https://github.com/erasmus-without-paper/ewp-specs-api-echo
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
[institutions-api]: https://github.com/erasmus-without-paper/ewp-specs-api-institutions
