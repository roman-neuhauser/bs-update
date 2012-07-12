# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=	/usr/local
BINDIR ?=	$(PREFIX)/bin

INSTALL_SCRIPT?=	install -m 755
RST2HTML?=$(call first_in_path,rst2html.py rst2html)

all check:

%.html: %.rest
	$(RST2HTML) $< $@

install:
	@$(INSTALL_SCRIPT) bs-update.in $(DESTDIR)$(BINDIR)/bs-update


define first_in_path
  $(firstword $(wildcard \
    $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
  ))
endef

