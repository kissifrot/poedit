
POEDIT_LINGUAS = af_ZA am ar ast be bg bn br ca cs da de el eo es es_PR et eu fa_IR fi fr fur gl he hi hr hu id is it ja ka ko ku ky lt lv mk mn nb nl nn pa pl pt_BR pt_PT ro ru sk sl sq_AL sr sv ta tr tt uk wa zh_CN zh_TW
WXWIN_LINGUAS  = af ca cs da de el es eu fi fr hi hu id it ja nb_NO nl pl pt_BR ru sl sv tr uk zh_CN zh_TW

localedir = $(datadir)/locale

install-data-local: @CATALOGS_TO_INSTALL@

install-poedit-catalogs:
	for i in $(POEDIT_LINGUAS) ; do \
	    $(mkinstalldirs) $(DESTDIR)$(localedir)/$$i/LC_MESSAGES ; \
	    $(INSTALL_DATA) $(srcdir)/$$i.mo $(DESTDIR)$(localedir)/$$i/LC_MESSAGES/poedit.mo ; \
	done

install-wxstd-catalogs:
	for i in $(WXWIN_LINGUAS) ; do \
	    $(mkinstalldirs) $(DESTDIR)$(localedir)/$$i/LC_MESSAGES ; \
	    $(INSTALL_DATA) $(srcdir)/wxwin/$$i.mo $(DESTDIR)$(localedir)/$$i/LC_MESSAGES/poedit-wxstd.mo ; \
	done



# ----------------------------------------------------------------------------
# Logic for catalogs updating follows
# (shamelessly stolen from wxWindows makefile):
# ----------------------------------------------------------------------------


# the programs we use (TODO: use configure to detect them)
MSGFMT=msgfmt --verbose
MSGMERGE=msgmerge
XGETTEXT=xgettext
XARGS=xargs

# common xgettext args: C++ syntax, use the specified macro names as markers
XGETTEXT_ARGS=-C -k_ -kwxGetTranslation -kwxTRANSLATE -s -j

# implicit rules
%.mo: %.po
	$(MSGFMT) -o $@ $<

# a PO file must be updated from wxstd.po to include new translations
%.po: $(srcdir)/poedit.pot
	if [ -f $@ ]; then $(MSGMERGE) $@ $(srcdir)/poedit.pot > $@.new && mv $@.new $@; else cp $(srcdir)/poedit.pot $@; fi

$(srcdir)/poedit.pot:
	touch $@
	(cd $(srcdir) ; find ../src -name "*.h" | $(XARGS) $(XGETTEXT) $(XGETTEXT_ARGS) -o poedit.pot)
	(cd $(srcdir) ; find ../src -name "*.cpp" | $(XARGS) $(XGETTEXT) $(XGETTEXT_ARGS) -o poedit.pot)
	(cd $(srcdir) ; find ../extras/wx-contrib/src/gizmos -name "*.cpp" | $(XARGS) $(XGETTEXT) $(XGETTEXT_ARGS) -o poedit.pot)

allpo: force-update
	@-for t in $(POEDIT_LINGUAS); do $(MAKE) $(srcdir)/$$t.po; done

allmo:
	@for t in $(POEDIT_LINGUAS); do $(MAKE) $(srcdir)/$$t.mo; done

force-update:
	$(RM) $(srcdir)/poedit.pot

# print out the percentage of the translated strings
stats:
	@for i in $(POEDIT_LINGUAS); do \
		x=`$(MSGFMT) -o /dev/null "$(srcdir)/$$i.po" 2>&1 | sed -e 's/[,\.]//g' \
			-e 's/\([0-9]\+\) translated messages\?/TR=\1/' \
			-e 's/\([0-9]\+\) fuzzy translations\?/FZ=\1/' \
			-e 's/\([0-9]\+\) untranslated messages\?/UT=\1/'`; \
		TR=0 FZ=0 UT=0; \
		eval $$x; \
		TOTAL=`expr $$TR + $$FZ + $$UT`; \
		echo "\"$$i\" => \"`expr 100 "*" $$TR / $$TOTAL`\", /* $$TOTAL strings */"; \
	done

#echo "$$i.po `expr 100 "*" $$TR / $$TOTAL`% of $$TOTAL strings"; 

.PHONY: allpo allmo force-update stats FORCE