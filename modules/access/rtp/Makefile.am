include $(top_srcdir)/modules/common.am
accessdir = $(pluginsdir)/access

# RTP plugin
access_LTLIBRARIES = librtp_plugin.la
librtp_plugin_la_SOURCES = \
	rtp.c \
	rtp.h \
	input.c \
	session.c \
	xiph.c
librtp_plugin_la_CFLAGS = $(AM_CFLAGS)
librtp_plugin_la_LIBADD = $(SOCKET_LIBS) $(LIBPTHREAD)

# Secure RTP library
libvlc_srtp_la_SOURCES = srtp.c
libvlc_srtp_la_CPPFLAGS =
libvlc_srtp_la_CFLAGS = $(GCRYPT_CFLAGS)
libvlc_srtp_la_LDFLAGS = -static
libvlc_srtp_la_LIBADD = $(GCRYPT_LIBS) $(LIBPTHREAD)
srtp_test_recv_CPPFLAGS =
srtp_test_recv_LDADD = libvlc_srtp.la
srtp_test_aes_CPPFLAGS =
srtp_test_aes_LDADD = $(GCRYPT_LIBS)

librtp_plugin_la_DEPENDENCIES =
if HAVE_GCRYPT
noinst_HEADERS = srtp.h
noinst_LTLIBRARIES = libvlc_srtp.la

check_PROGRAMS = srtp-test-aes srtp-test-recv
TESTS = $(check_PROGRAMS)

librtp_plugin_la_CFLAGS += -DHAVE_SRTP $(GCRYPT_CFLAGS)
librtp_plugin_la_LIBADD += libvlc_srtp.la $(GCRYPT_LIBS)
librtp_plugin_la_DEPENDENCIES += libvlc_srtp.la
endif

lcov-run:
	rm -Rf *.gcda lcov
	$(MAKE) $(AM_MAKEFLAGS) check

lcov-pre.out:
	lcov -c -d . -o lcov.tmp

lcov.out: lcov-pre.out
	lcov -r lcov.tmp '*test*' > lcov.out

lcov: lcov.out
	genhtml lcov.out -o lcov

.PHONY: lcov-run
