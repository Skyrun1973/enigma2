dist_noinst_SCRIPTS = xml2po.py

LANGS = ar bg ca cs da de el en en_GB es et fa fi fr fy he hr hu id is it ku lt lv nl no nb pl pt pt_BR ro ru sv sk sl sr th tr uk vi zh_CN zh_HK
LANGMO = $(LANGS:=.mo)
LANGPO = $(LANGS:=.po)

EXTRA_DIST = $(LANGPO)

# the TRANSLATORS: allows putting translation comments before the to-be-translated line.
enigma2-py.pot: $(top_srcdir)/lib/python/*.py $(top_srcdir)/lib/python/*/*.py $(top_srcdir)/lib/python/*/*/*.py $(top_srcdir)/lib/python/Plugins/*/*/*.py $(top_srcdir)/lib/python/Plugins/*/*/*/*.py
	$(XGETTEXT) --no-wrap -L Python --from-code=UTF-8 -kpgettext:1c,2 --add-comments="TRANSLATORS:" -d @PACKAGE_NAME@ -s -o $@ $^
	$(SED) --regexp-extended --in-place $@ --expression="s/(#:\s).*\/(.*\/plugin\.py|[^.*\/plugin\.py].*)(:\d*)/\1\2\3/"

enigma2-xml.pot: $(srcdir)/xml2po.py $(top_srcdir)/data/*.xml $(top_srcdir)/lib/python/Plugins/SystemPlugins/*/*.xml $(top_srcdir)/lib/python/Plugins/Extensions/*/*.xml
	$(PYTHON) $^ > $@
	$(SED) --regexp-extended --in-place $@ --expression="s/(#:\s).*\/(.*\.xml)/\1\2/"

enigma2.pot: enigma2-py.pot enigma2-xml.pot
	$(SED) --in-place $^ --expression="s/CHARSET/UTF-8/"
	cat $^ | $(MSGUNIQ) --no-wrap -o $@ -

%.po: enigma2.pot
	if [ -f $@ ]; then \
		$(MSGMERGE) --backup=none --no-wrap -s -U $@ $< && touch $@; \
	else \
		$(MSGINIT) -l $@ -o $@ -i $< --no-translator; \
	fi

.po.mo:
	$(MSGFMT) -o $@ $<

BUILT_SOURCES = $(LANGMO)
CLEANFILES = $(LANGMO) enigma2-py.pot enigma2-xml.pot enigma2.pot

dist-hook: $(LANGPO)

install-data-local: $(LANGMO)
	for lang in $(LANGS); do \
		$(mkinstalldirs) $(DESTDIR)$(pkgdatadir)/po/$$lang/LC_MESSAGES; \
		$(INSTALL_DATA) $$lang.po $(DESTDIR)$(pkgdatadir)/po/@PACKAGE_NAME@-$$lang.po; \
		$(INSTALL_DATA) $$lang.mo $(DESTDIR)$(pkgdatadir)/po/$$lang/LC_MESSAGES/@PACKAGE_NAME@.mo; \
		$(INSTALL_DATA) $$lang.po $(DESTDIR)$(pkgdatadir)/po/$$lang.po; \
	done

uninstall-local:
	for lang in $(LANGS); do \
		$(RM) $(DESTDIR)$(pkgdatadir)/po/@PACKAGE_NAME@-$$lang.po; \
		$(RM) $(DESTDIR)$(pkgdatadir)/po/$$lang/LC_MESSAGES/@PACKAGE_NAME@.mo; \
	done
