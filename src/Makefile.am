#=============================================================================
#
# libvmod-queryfilter: src/Makefile.am
#
#=============================================================================

AM_CPPFLAGS = \
    -I@VARNISHSRC@/include \
    -I@VARNISHSRC@/bin/varnishd \
    -I@VARNISHSRC@ \
    -I@top_builddir@/include

vmoddir = @VMOD_DIR@
vmod_LTLIBRARIES = libvmod_queryfilter.la

libvmod_queryfilter_la_LDFLAGS = \
    -module \
    -export-dynamic \
    -avoid-version \
    -shared

libvmod_queryfilter_la_SOURCES = \
	vcc_if.c \
	vcc_if.h \
	vmod_queryfilter.c

if VARNISHSRC_3_X
vcc_if.c vcc_if.h: @top_srcdir@/src/vmod_queryfilter3.vcc
	@PYTHON@ @VMODTOOL@ @top_srcdir@/src/vmod_queryfilter3.vcc
else
vcc_if.c vcc_if.h: @top_srcdir@/src/vmod_queryfilter.vcc
	@PYTHON@ @VMODTOOL@ @top_srcdir@/src/vmod_queryfilter.vcc
endif

VMOD_TESTS = @abs_srcdir@/tests/*.vtc
.PHONY: $(VMOD_TESTS)

@abs_srcdir@/tests/*.vtc:
	PATH="@VARNISHSRC@/bin/varnishd:${PATH}" @VARNISHTEST@ \
        -Dvarnish_source=@VARNISHSRC@ \
        -Dvarnishd=@VARNISHSRC@/bin/varnishd/varnishd \
        -Dvmod_topbuild=@abs_top_builddir@ $@

check: $(VMOD_TESTS)

EXTRA_DIST = \
	vmod_queryfilter.vcc \
	$(VMOD_TESTS)

CLEANFILES = @builddir@/vcc_if.c @builddir@/vcc_if.h

## EOF

