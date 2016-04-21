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

Parameters MUST be provided either in a query string (for GET requests), or in
the `application/x-www-form-urlencoded` format (for POST requests).


### `course_id` (repeatable, required)

A list of course identifiers (max 100 items) - IDs of courses the client wants
to retrieve information on.

This parameter is *repeatable*, so the request MAY contain multiple occurrences
of it. The server is REQUIRED to process all of them.


Permissions
-----------

All requests from the EWP Network MUST be allowed access to this API.
Additionally, servers MAY allow this API to be accessed by external clients
(without proper client certificate). Consult the [Echo API][echo] specs for
details on handling unprivileged requests.


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply.

 * Invalid (unknown) `course_id` values MUST be ignored. Servers MUST return
   a valid (HTTP 200) XML response in such cases, but the response will simply
   not contain the information on the unknown `course_id` values. (If all
   values are unknown, servers MUST respond with an empty envelope.)

 * If the length of `course_id` list is greater than 100, servers MAY respond
   with HTTP 400. Clients SHOULD split such large requests into a couple of
   smaller ones.


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
