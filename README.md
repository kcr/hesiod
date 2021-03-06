[![Build Status](https://travis-ci.org/achernya/hesiod.png)](https://travis-ci.org/achernya/hesiod)

This is release 3.2.1 of the Hesiod name service library.  Hesiod can
provide general name service for a variety of applications and is
based on the Berkeley Internet Name Daemon (BIND).

To prepare this directory for building, run the command `./configure`.
configure takes a number of options; use `./configure --help` to find
out what they are.  Hesiod requires a vaguely ANSI compiler to build;
gcc will do.

Run `make` or `make all` to build the Hesiod library.

Run `make install` to install the Hesiod library.

You will want to create a configuration file named hesiod.conf in the
sysconfdir (/usr/local/etc/hesiod.conf by default) on your client
machines, reading something like:

	rhs=.your.domain
	lhs=.ns

The value of rhs can be overridden at run time by the environment
variable HES_DOMAIN.  The value ".ns" for lhs is an unfortunate
historical convention; ".hs" or "hesiod" would have been better.
Nevertheless, you probably want to use ".ns" for compatibility with
existing Hesiod domains.

To create Hesiod information on your central name servers, you need to
make them authoritative for the domain ns.your.domain with a line in
named.boot reading something like:

	primary		ns.your.domain		named.hesiod

And then in named.hesiod, you need data looking something like:

	; SOA and NS records.
	@	IN	SOA	server1.your.domain admin-address.your.domain (
			40000	      ; serial - database version number
			1800	      ; refresh - sec servers
			300	      ; retry - for refresh
			3600000       ; expire - unrefreshed data
			7200 )        ; min
			NS	server1.your.domain
			NS	server2.your.domain

	; Actual Hesiod data.
	haynes.grplist	TXT	"haynes:2638"
	haynes.group	TXT	"haynes:*:2638:"
	2638.gid	CNAME	haynes.group
	zephyr.sloc	TXT	"zephyrserver1.my.domain"
	zephyr.sloc	TXT	"zephyrserver2.my.domain"

There is a mailing list at MIT for Hesiod users, hesiod@mit.edu.  To
get yourself on or off the list, send mail to hesiod-request@mit.edu.
