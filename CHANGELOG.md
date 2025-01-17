# CHANGELOG

## v1.7.0

July 2022.

* Replaces `error_logger` with `logger`.

* Turns repeated reconnection errors into log level `notice`.

* Parser optimizations.

* Enables performance tuning of the received packets handling.
  The socket is set to `{active, N}` with N = 10 by default.
  This is configurable by including `{active, N}` in the socket options or
  in the TLS options when TLS is used.

  Note that `{active, N}` with TLS requires OTP 21.3 or later.
  When using OTP below 21.3 the option needs to be set to `{active, true}`.

## v1.6.0

July 2022.

* Adds sentinel support.

* Obfuscates username and password in the state, to prevent them from appearing
  verbatim in logs and stack traces. They can be provided as 0-ary functions
  returning the actual secret when applied.

* Fix a crash that happens when reconnect fails twice with two different
  reasons. This bug was introduced in v1.5.0.

## v1.5.1

May 2022.

* Solves a bug introduced in v1.5.0 causing the reconnect process to survive
  if the client is stopped while attempting to reconnect.

* Drops the spawned process used for reconnecting. Instead, the reconnect
  attempts are scheduled using timers.

  The option `reconnect_sleep` now applies to the time between a
  successful connect and the first reconnect attemt, if the connection is lost
  just after connecting. However, there is no delay before reconnecting
  if the connection has been up for at least `reconnect_sleep` milliseconds.

## v1.5.0

May 2022.

* No delay before the first reconnect attempt.

* eredis_sub: Automatic re-subscribe on reconnect. Messages on the
  form `{subscribed, Channel, Pid}` are sent to the controlling
  process in this case and need to be acked.

* eredis_sub: TLS, custom TCP options, AUTH with username, SELECT
  database and registered name options added.

## v1.4.1

June 2021.

* Restore support for rebar 2.
* Optimize parser.

## v1.4.0

June 2021.

* Support for named connection processes.
* Support AUTH with username.

## v1.3.3

March 2021.

* Include correct files in published package on hex.pm.

## v1.3.2

February 2021.

* Official release

## v1.3.1-nordix

February 2021.

* Fix build problems with Mix and other non-Rebar tools

## v1.3.0-nordix

December 2020 in the Nordix fork by Björn Svensson and Viktor Söderqvist.

* TLS support
* Correction regarding chunked error responses
* All socket errors, including send and setopts errors, trigger reconnect
* Explicitly close socket before reconnect, preventing dangling ports
* Terminate reconnect loop when client terminates (fixes wooga/eredis#124)
* Try all addresses from `inet:getaddrs/2` instead of just one when connecting
* Stray messages don't crash the connection process
* Improved tests and coverage
* CI builds using Github Actions
* Elvis code style and Dialyzer corrections

## v1.2.0

2018 in the original `wooga/eredis` repo.

* Unix domain socket support (PR wooga/eredis#108 by Igor Slepchin)
* Reset parser state on disconnect (PR wooga/eredis#106 by Ivan Baidakou)
* Non-blocking init AKA async connect (PR wooga/eredis#105 by Valery Meleshkin)
* Some improved tests

## v1.1.0

* Merged a ton of of old and neglected pull requests. Thanks to
  patient contributors:
  * Emil Falk
  * Evgeny Khramtsov
  * Kevin Wilson
  * Luis Rascão
  * Аверьянов Илья (savonarola on github)
  * ololoru
  * Giacomo Olgeni

* Removed rebar binary, made everything a bit more rebar3 & mix
  friendly.


## v1.0.8

* Fixed include directive to work with rebar 2.5.1. Thanks to Feng Hao
  for the patch.

## v1.0.7

* If an eredis_sub_client needs to reconnect to Redis, the controlling
  process is now notified with the message `{eredis_reconnect_attempt,
  Pid}`. If the reconnection attempt fails, the message is
  `{eredis_reconnect_failed, Pid, Reason}`. Thanks to Piotr Nosek for
  the patch.

* No more deprecation warnings of the `queue` type on OTP 17. Thanks
  to Daniel Kempkens for the patch.

* Various spec fixes. Thanks to Hernan Rivas Acosta and Anton Kalyaev.

## v1.0.6

* If the connection to Redis is lost, requests in progress will
  receive `{error, tcp_closed}` instead of the `gen_server:call`
  timing out. Thanks to Seth Falcon for the patch.

## v1.0.5

* Added support for not selecting any specific database. Thanks to
  Mikl Kurkov for the patch.

## v1.0.4

* Added `eredis:q_noreply/2` which sends a fire-and-forget request to
  Redis. Thanks to Ransom Richardson for the patch.

* Various type annotation improvements, typo fixes and robustness
  improvements. Thanks to Michael Gregson, Matthew Conway and Ransom
  Richardson.

## v1.0.3

* Fixed bug in eredis_sub where when the connection to Redis was lost,
  the socket would not be set into {active, once} on reconnect. Thanks
  to georgeye for the patch.

## v1.0.2

* Fixed bug in eredis_sub where the socket was incorrectly set to
  `{active, once}` twice. At large volumes of messages, this resulted
  in too many messages from the socket and we would be unable to keep
  up. Thanks to pmembrey for reporting.

## v1.0

* Support added for pubsub thanks to Dave Peticolas
  (jdavisp3). Implemented in `eredis_sub` and `eredis_sub_client` is a
  subscriber that will forward messages from Redis to an Erlang
  process with flow control. The user can configure to either drop
  messages or crash the driver if a certain queue size inside the
  driver is reached.

* Fixed error handling when eredis starts up and Redis is still
  loading the dataset into memory.

## v0.7.0

* Support added for pipelining requests, which allows batching
  multiple requests in a single call to eredis. Thanks to Dave
  Peticolas (jdavisp3) for the implementation.

## v0.6.0

* Support added for transactions, by Dave Peticolas (jdavisp3) who implemented
  parsing of nested multibulks.

## v0.5.0

* Configurable reconnect sleep time, by Valentino Volonghi (dialtone)

* Support for using eredis as a poolboy worker, by Valentino Volonghi
  (dialtone)
