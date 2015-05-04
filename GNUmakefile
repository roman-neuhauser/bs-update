# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=         /usr/local
BINDIR ?=         $(PREFIX)/bin
MANDIR ?=         $(PREFIX)/share/man
MAN1DIR ?=        $(MANDIR)/man1

GZIPCMD ?=        gzip
INSTALL_DATA ?=   install -m 644
INSTALL_DIR ?=    install -m 755 -d
INSTALL_SCRIPT ?= install -m 755
RST2HTML ?=       $(call first_in_path,rst2html.py rst2html)

name =            bs-update

artifacts =       $(installed) $(htmlfiles)
installed =       $(name).1.gz $(name)
htmlfiles =       README.html

ALL_INCLUSIVE  ?= 0

.DEFAULT_GOAL  := $(if $(subst 0,,$(ALL_INCLUSIVE)),all,most)


all: $(artifacts)
most: $(installed)

.PHONY: all most
all most:
	@touch .built

.PHONY: clean
clean:
	$(RM) .built $(artifacts)

.PHONY: check
check: $(.DEFAULT_GOAL)
	SHELL=$(SHELL) $(SHELL) rnt/run-tests.sh tests $$PWD/$(name)

$(name): $(name).in
	$(INSTALL_SCRIPT) $< $@

.PHONY: html
html: $(htmlfiles)

%.html: %.rest
	$(RST2HTML) $< $@

%.1.gz: %.1
	$(GZIPCMD) < $< > $@

.PHONY: install
install: .built
	$(INSTALL_DIR) $(DESTDIR)$(BINDIR)
	$(INSTALL_DIR) $(DESTDIR)$(MAN1DIR)
	$(INSTALL_SCRIPT) $(name).in $(DESTDIR)$(BINDIR)/$(name)
	$(INSTALL_DATA) $(name).1.gz $(DESTDIR)$(MAN1DIR)/$(name).1.gz

.built:
	@printf "%s\n" '' "ERROR: run '$(MAKE) all' first." '' >&2
	@false

define first_in_path
  $(firstword $(wildcard \
    $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
  ))
endef

