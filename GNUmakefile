# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=	/usr/local
BINDIR ?=	$(PREFIX)/bin
MANDIR ?=	$(PREFIX)/share/man
MAN1DIR ?=	$(MANDIR)/man1

GZIPCMD?=	gzip
INSTALL_DATA?=	install -m 644
INSTALL_SCRIPT?=	install -m 755
RST2HTML?=$(call first_in_path,rst2html.py rst2html)

all: bs-update.1.gz

check:

%.html: %.rest
	$(RST2HTML) $< $@

%.1.gz: %.1
	$(GZIPCMD) < $< > $@

install: bs-update.1.gz
	@$(INSTALL_SCRIPT) bs-update.in $(DESTDIR)$(BINDIR)/bs-update
	@$(INSTALL_DATA) bs-update.1.gz $(DESTDIR)$(MAN1DIR)/bs-update.1.gz

define first_in_path
  $(firstword $(wildcard \
    $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
  ))
endef

