# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=         /usr/local
BINDIR ?=         $(PREFIX)/bin
MANDIR ?=         $(PREFIX)/share/man
MAN1DIR ?=        $(MANDIR)/man1

GZIPCMD ?=        gzip
INSTALL_DATA ?=   install -m 644
INSTALL_DIR ?=    install -d 755
INSTALL_SCRIPT ?= install -m 755
RST2HTML ?=       $(call first_in_path,rst2html.py rst2html)

name =            bs-update

artifacts =       $(name).1.gz $(name)

wc:
	git submodule init
	git submodule update

all: $(artifacts)

clean:
	$(RM) $(artifacts)

check: all
	SHELL=$(SHELL) $(SHELL) rnt/run-tests.sh tests $$PWD/$(name)

$(name): $(name).in
	$(INSTALL_SCRIPT) $< $@

%.html: %.rest
	$(RST2HTML) $< $@

%.1.gz: %.1
	$(GZIPCMD) < $< > $@

install: $(name).1.gz
	$(INSTALL_DIR) $(DESTDIR)$(BINDIR)
	$(INSTALL_DIR) $(DESTDIR)$(MAN1DIR)
	@$(INSTALL_SCRIPT) $(name).in $(DESTDIR)$(BINDIR)/$(name)
	@$(INSTALL_DATA) $(name).1.gz $(DESTDIR)$(MAN1DIR)/$(name).1.gz

define first_in_path
  $(firstword $(wildcard \
    $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
  ))
endef

.DEFAULT_GOAL := all

.PHONY: all
.PHONY: check
.PHONY: clean
.PHONY: install
.PHONY: wc
