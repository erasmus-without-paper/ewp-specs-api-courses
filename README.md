Courses API
===========

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This document describes the **Courses API**. This API is not directly related
to EWP mobility APIs, but it might help with building user-friendly mobility
client software. It allows other HEIs to access information on courses and
other learning opportunities conducted in this institution.


Learning Opportunities
----------------------

This API is named "Courses API", but what it *really* serves are entities
called "Learning Opportunities".

### Types

`Course` is just one of many types of Learning Opportunities. Other types
include entities such as `Degree programme`, `Module` or `Class`. This model
has been adopted from the [EMREX][emrex] and ELMO projects (you will see many
similarities to EMREX in the XSDs too).

Server implementers MAY choose to expose any number of Learning Opportunities
via the Courses API (and [Simple Course Replication API][replication-api]). As
far as we know, in case of some server implementations, this will be a natural
way to go, because all types of Learning Opportunities are kept in a single
database table there (see below). Client implementers should always take notice
of the `<type>` element returned.


### Specification vs instance

Learning opportunities are models with two entities: LO specifications and LO
instances.

 * LO specification (LOS) is a abstract specification of a course/programme. A
   template of sorts. It has no students, no grades, no starting nor ending
   dates.

 * LO instance (LOI) describes a "real" instance of an LO. It is conducted in
   specific time, it has its students, etc.

This API allows to access both - LOSes and LOIs - but all LOIs are accessible
via LOS parents. If a LOS doesn't has a LOI child, then it means that this LOS
has not been conducted in the searched timespan.


<a name='unique-identifiers'></a>

### LO identifiers: `los_id` and `loi_id`

**Please read it carefully. Hasty decisions might cost you in the future!**

There are major differences in a way learning opportunities are modeled in
various computer systems:

 * Some partners keep all learning opportunities in a single database table,
   with the same primary key. In such databases, **it is not possible**, for
   example, for a `Course` and a `Degree Programme`, to have the same value of
   primary key.

 * Other partners keep them in separate tables (e.g. three tables: for
   programmes, courses and classes). In such databases, **is it possible** that
   there exists a `Course`, and a `Degree Programme` with exactly the same
   primary key.

In EWP, we have decided to adopt the former strategy (as EMREX had). **This
means that partners using the latter strategy (multiple tables) might need to
perform some additional actions in order to avoid ID conflicts:**

 * One way of dealing with the problem would be to force unique IDs across
   multiple tables. This way, whenever you receive an ID, you will be able to
   find out which entity it refers to (by searching multiple tables). This
   approach however is often bad for performance reasons (and most databases
   don't support it natively). Also, you might be required to update your
   existing primary keys, which is bad in itself.

 * Another way would be to use UUIDs as `los_id`. You won't need to update your
   primary keys, but you will need to keep another set of unique keys, which
   is also not so good for performance.

 * Fastest approach (albeit a bit more complex) is to use "dynamic IDs". You
   do not change your database in any way, but you are required to publish
   unique, dynamically generated IDs in your EWP responses. Clients will treat
   such "dynamic IDs" as regular IDs. Later, whenever a client asks about the
   details of such ID, you must be able to dynamically map it back into a real
   row in a real table.

   **Example:** if you have separate tables for degree programmes and for
   courses, and you have a course with a primary key value of `123`, then,
   whenever you refer to this course in EWP APIs, you might use `C123`
   (instead of just `123`). Later, if one of the clients asks you about `P123`,
   you will know that he is after a `Degree Programme`, not a `Course`, and you
   will know which of your tables to query for `123`.

   Note, that letters `C` and `P` are just examples. From the client's point of
   view, they are part of the ID, and they have no special meaning. It is up to
   you to decide on this meaning. **Once you decide on one schema, you will
   need to use it consistently across all EWP APIs.**

Same is true for `loi_id` values (which are unique identifiers of specific
LOIs).

Note, that you SHOULD make this decision now, even if you only expose a single
type of learning opportunity via this API. If you don't, then you will have
problems expanding to other types in the future.

Also, IDs of your LOSes and LOIs generally **shouldn't change**.


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

ID of the institution at which the LOSes (referenced in `los_id` parameters)
are conducted. This parameter MUST be required by the server even if the server
covers only a single institution.


### `los_id` (repeatable, required)

A list of LOS identifiers (no more than `<max-los-ids>` items) - IDs of LOSes
the client wants to retrieve information on.

This parameter is *repeatable*, so the request MAY contain multiple occurrences
of it. The server is REQUIRED to process all of them.

Server implementers provide their own chosen value of `<max-los-ids>` via
their manifest entry (see [manifest-entry.xsd](manifest-entry.xsd)). Clients
SHOULD parse this value (or assume it's equal to `1`).


### `lois_before` (optional)

A date in the `YYYY-mm-dd` format. It given, then the client states that it is
interested only in the LOIs conducted before this date. If given, server SHOULD
include only such `<learningOpportunityInstance>` elements for which their
`<end>` date is before this given date.

By default (if neither `lois_before` nor `lois_after` is given), server
includes all LOIs.


### `lois_after` (optional)

A date in the `YYYY-mm-dd` format. It given, then the client states that it is
interested only in the LOIs conducted after this date. If given, server SHOULD
include only such `<learningOpportunityInstance>` elements for which their
`<start>` date is after this given date.

By default (if neither `lois_before` nor `lois_after` is given), server
includes all LOIs.


### `los_at_date` (optional)

A date, in the `YYYY-mm-dd` format. If given, the server MAY attempt to
generate a LOS description *as it would have looked like at this date*.

When we were designing this API, it was brought to our attention that, in many
computer systems, some attributes of LOSes may change in time. Some of these
changes are important (e.g. the number of ECTS credits awarded), while other
describe just unimportant details (e.g. bibliography used). This parameter is
a simple, although somewhat "crude" way of allowing the client to access this
history (other, more comprehensive ways, might be designed in the future).

*A note for client developers:* You should be aware, that we do not require the
server developers to support this parameter. Many computer systems may return
only the "freshest" version of the LO specification, regardless of the date
passed.

*A note for server developers:* While it is not required to support this
parameter, it is recommended to describe somewhere if (and how) you do (for
example, in your manifest entry's `<admin-notes>` element). This might help
future API designers to propose more comprehensive APIs.


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

 * Invalid (unknown) `los_id` values MUST be **ignored**. Servers MUST
   return a valid (HTTP 200) XML response in such cases, but the response will
   simply not contain the information on the unknown `los_id` values. If
   all values are unknown, servers MUST respond with an empty `<response>`
   element. This requirement is true even when `<max-los-ids>` is `1`.

 * If the length of `los_id` list is greater than `<max-los-ids>`,
   servers MUST respond with HTTP 400.


Response
--------

Servers MUST respond with a valid XML document described by the [response.xsd]
(response.xsd) schema. See the schema annotations for further information.


Data model entities involved in the response
--------------------------------------------

 * Learning Opportunity Specification
 * Learning Opportunity Instance
 * Academic Term


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[echo]: https://github.com/erasmus-without-paper/ewp-specs-api-echo
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
[institutions-api]: https://github.com/erasmus-without-paper/ewp-specs-api-institutions
[replication-api]: https://github.com/erasmus-without-paper/ewp-specs-api-course-replication
[emrex]: http://emrex.eu/
