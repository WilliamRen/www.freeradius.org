# Set this to where ever the files are located.
MAN	:= ../../../stable.git.freeradius.org/man

VPATH	:= $(MAN)/man1:$(MAN)/man5:$(MAN)/man8

FILES	:= $(notdir $(wildcard $(MAN)/man1/*\.1) $(wildcard $(MAN)/man5/*\.5) $(wildcard $(MAN)/man8/*\.8))

TXT	:= $(addsuffix .txt,$(basename $(FILES)))

HTML	:= $(addsuffix .html,$(basename $(FILES)))

BIN	:= $(filter %.1,$(FILES))
BIN	+= $(filter %.8,$(FILES))

MODS	:= $(filter rlm_%,$(FILES))

CONFIG	:= $(filter %.5,$(FILES))
CONFIG	:= $(filter-out rlm_%,$(CONFIG))

%.txt:%.1
	nroff -man -Tascii $< | col -b > $@

%.txt:%.5
	nroff -man -Tascii $< | col -b > $@

%.txt:%.8
	nroff -man -Tascii $< | col -b > $@

SED_FIX = 's/man[0-8]/man/g;s/\.[0-8]//g;s,</HEAD>,<link rel="stylesheet" type="text/css" href="/css/style.css" /></HEAD>,;s,<BODY>,<BODY><!--\#include virtual="/header.html" --><div id="content">,;s,</BODY>,</div><!--\#include virtual="/footer.html" --></BODY>,;s,<A HREF="../,<A HREF=",;s,HREF="man/,HREF="/radiusd/man/,;s,<A HREF="/cgi-bin/man/manhtml">manhtml</A>,manhtml,'

%.html:%.1
	man2html -r $< | tail -n +2 | sed $(SED_FIX) > $@
	@chmod a+x $@

%.html:%.5
	man2html -r $< | tail -n +2 | sed $(SED_FIX) > $@
	@chmod a+x $@

%.html:%.8
	man2html -r $< | tail -n +2 | sed $(SED_FIX) > $@
	@chmod a+x $@

.PHONY: all
all:	$(TXT) $(HTML) index.html

index.html: $(HTML) programs-inc.html config-inc.html modules-inc.html
	@touch $@
	@chmod +x $@

programs-inc.html:
	echo "<ul>" >> $@
	for x in $(BIN); do \
		name=`echo $$x | sed 's,\..*,,'`; \
		echo "<a href=\"$$name.html\">$$name</a>&nbsp;(<a href=\"$$name.txt\">txt</a>)<br />" >> $@; \
	done	
	echo "</ul>" >> $@

config-inc.html:
	echo "<ul>" >> $@
	for x in $(CONFIG); do \
		name=`echo $$x | sed 's,\..*,,'`; \
		echo "<a href=\"$$name.html\">$$name</a>&nbsp;(<a href=\"$$name.txt\">txt</a>)<br />" >> $@; \
	done	
	echo "</ul>" >> $@
	echo >> $@

modules-inc.html:
	echo "<ul>" >> $@
	for x in $(MODS); do \
		name=`echo $$x | sed 's,\..*,,'`; \
		echo "<a href=\"$$name.html\">$$name</a>&nbsp;(<a href=\"$$name.txt\">txt</a>)<br />" >> $@; \
	done	
	echo "</ul>" >> $@

clean:
	@rm -f programs-inc.html config-inc.html modules-inc.html \
		$(TXT) $(HTML)
