Changes in 3.6.0
================

* Removed unused RTPStream code concerning video streams. Also
  consolidated the rtpstream audio port usage to reuse the global
  `[media_port]` instead of the `[rtpstream_audio_port]`.
  Also the `-min_rtp_port` and `-max_rtp_port` options have been
  removed. Advantages: cleaner code, fewer scenario variables.
  Drawbacks: possible ICMP port unreachable messages for RCTP and video.
  Also, no easy way to discern different streams if you want to bombard
  a single UAS with multiple RTP streams. (Issue #192, reported by
  @atsakiridis.)

* Added RTP echo processing for multiple ports/sockets. Based on `rtp_echo`
  action in script. Compile with  `--with-rtpstream` and use it by adding 
 `<rtp_echo value="1">` to start the RTP echo on every call
  (should be enabled via `-rtp_echo`).

Features added in 3.6.0
=======================

* Added `play_dtmf` code originally from
  https://sourceforge.net/p/sipp/patches/50/ (Dmitry Kunilov), then
  pull #82 (@horacimacias) and then #141 (@vodik). Compile with
  pcap-play support, and use it by adding `<exec play_dtmf="1234*#"/>`
  similar to how you use `play_pcap_audio`.
  - Add RTP payload 96 in your SDP:
    m=audio [media_port] RTP/AVP 0 96 97
    a=rtpmap:0 PCMU/8000
    a=rtpmap:96 telephone-event/8000
    a=fmtp:96 0-15
    a=rtpmap:97 no-op/8000
  - Exec syntax is `<exec play_dtmf="digits[,length]"/>` where digits
    can be one or more of "0123456789#*ABCD" and length defaults to 200
    and must be between 50 and 2000.
  - Instead of digits a `[field...]` keyword is also accepted.
  - Make sure you add enough `<pause/>` after `play_dtmf`.
* Added `rtp_echo` action (pull #259 by Snom Technology). Compile with
  `--with-rtpstream` and use it by adding `<rtp_echo value="0">` to stop
  the RTP echo enabled via `-rtp_echo`. RTP echo can be restarted via
  `<rtp_echo value="1">` action. Usage example in `regress/github-#0259/uas.xml`

Bugs fixed in 3.5.x
===================

* Recompile entire source after a reconfigure.
* Compile with -lncurses if -lcurses does not exist. (Issue #205, reported
  by Paul Malpass.)
* Handle Contact header with extra angle brackets ('<...>') outside of the
  uri-parameters (in the contact-params). The contact params should not be
  used in the `next_url`. (Issue #234, reported by Justin Zimmer.)
* Fix TLS issues for during high load. (Issue #241, #243, reported by @sgel83.)
* Fix compile issues on old CentOS and Solaris. (Issue #211, #245.)


Bugs fixed in 3.5.1
===================

* Fix qop-value in authorization Digest. It can only hold a single value
  (auth, auth-int, ...) and does not take double quotes, in contrast to
  the challenge. Some servers returned a 400 upon receiving this.
  (Issue #191, reported by @artlov.)
* Fix compile error on Cygwin. (Issue #193, reported by @Gankarloo.)


Features added in 3.5.0
=======================

* Clean up source code, fix typo's, alter warning and error messages,
  fix pedantic coding style. Add gtest framework and tests. Add regression
  tests. Fix and improve build scripts (see also: `build.sh`).
* Use better timing with `clock_gettime` (or `clock_get_time` on OSX).
* Don't complain about the dummy variable `_` being used only once.
* Ignore 4x NUL keepalive (next to ignoring the CRLF CRLF keepalive).
* Add `[date]` keyword.
* Add `-trace_screen` option to log screen output in a file.
* Add `-rate_increase` option to increase load periodically.
* Add `-callid_slash_ign` to disable the magic triple slash behaviour.a
* Alter `-aa` to also reply to OPTIONS.
* Allow replaying pcaps with `LINUX_SLL`, `EN10MB` and 802.11 (and ratiotap)
  link layer types. Handle 802.1Q tagged frames.
* Allow starting SIPp without a TERM setting (a "working" terminal).
* Allow m=image in SDP to pcapplay faxes/images.
* `<exec play_pcap_audio="..."/>` and friends:
  - If the argument is not an absolute path, the pcap is searched next
    to the scenario, before falling back to checking the current
    working directory.
  - The argument may be enclosed in brackets, in which case it is
    interpreted as a keyword value; set through the `-key` command line
    option. Example: `<exec play_pcap_audio="[file1]"/>` with option
    `-key file1 /path/to/pcap`.


Bugs fixed in 3.5.0
===================

* Start SDP search in body instead of in header. Fix IPv6 media address in SDP.
* Allow single CR and single LF in SDP.
* Don't confuse cnonce with nonce, improve other auth parsing.
* Don't abort SIPp if the To header is missing.
* Fixes to XML parser; improve `get_peer_tag` behaviour.
* Fix jump recursion crashes.
* Fix a few (potential) memory leaks and dangerous code.
* Remove a few autogenerated files from tree (configure, manpage).
* Fix digest calculation when `qop` is given.


3.4.1
=====

* Not documented here.
