###############################################################################
## Makefile for apertium-kas
###############################################################################

LANG1=kas
BASENAME=apertium-$(LANG1)

TARGETS_COMMON = $(LANG1).automorf.bin $(LANG1).autogen.bin \
	$(LANG1).autopgen.bin \
	$(LANG1).automorf.att.gz $(LANG1).autogen.att.gz \
        $(LANG1).rlx.bin

@ap_include@

###############################################################################
## Kashmiri transducer
###############################################################################

$(LANG1).autogen.bin: $(BASENAME).$(LANG1).dix
	apertium-validate-dictionary $<
	lt-comp rl $< $@

$(LANG1).automorf.bin: $(BASENAME).$(LANG1).dix $(BASENAME).$(LANG1).acx
	apertium-validate-acx $(BASENAME).$(LANG1).acx
	apertium-validate-dictionary $<
	lt-comp lr $< $@ $(BASENAME).$(LANG1).acx

$(LANG1).autogen.att.gz: $(LANG1).autogen.bin
	lt-print $< | gzip -9 -c -n > $@

$(LANG1).automorf.att.gz: $(LANG1).automorf.bin
	lt-print $< | gzip -9 -c -n > $@

$(LANG1).autopgen.bin: $(BASENAME).post-$(LANG1).dix
	lt-comp lr $< $@

###############################################################################
## Disambiguation rules 
###############################################################################

$(LANG1).rlx.bin: $(BASENAME).$(LANG1).rlx $(CGCOMP)
	$(CGCOMP) $< $@

###############################################################################
## Distribution
###############################################################################
EXTRA_DIST=$(BASENAME).$(LANG1).dix \
	   $(BASENAME).$(LANG1).rlx  \
	   modes.xml

###############################################################################
## Installation stuff
###############################################################################
#
#   apertium_kas_dir: This is where the compiled binaries go
#   apertium_kas_srcdir: This is where the source files go

apertium_kasdir=$(prefix)/share/apertium/$(BASENAME)/
apertium_kas_srcdir=$(prefix)/share/apertium/$(BASENAME)/

apertium_kas_DATA=$(TARGETS_COMMON)

pkgconfigdir = $(prefix)/share/pkgconfig
pkgconfig_DATA = $(BASENAME).pc

# Modes are not installed since they're all just "debug modes". All
# modes are created by this goal.
modes/$(LANG1)-morph.mode: modes.xml
	apertium-validate-modes modes.xml
	apertium-gen-modes modes.xml

noinst_DATA=modes/$(LANG1)-morph.mode

install-data-local: install-modes
	test -d $(DESTDIR)$(apertium_kas_srcdir) || mkdir -p $(DESTDIR)$(apertium_kas_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1).dix $(DESTDIR)$(apertium_kas_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1).rlx $(DESTDIR)$(apertium_kas_srcdir)

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

test: all
	apertium-regtest test
