# make and install sample templates

INSTALL ?= install
TAR ?= tar
prefix ?= $(HOME)
template_dir ?= $(prefix)/share/git-core/templates/
# DESTDIR=

# Shell quote;
# Result of this needs to be placed inside ''
shq = $(subst ','\'',$(1))
# This has surrounding ''
shellquote = '$(call shq,$(1))'

all: boilerplates.made custom

# Put templates that can be copied straight from the source
# in a file direc--tory--file in the source.  They will be
# just copied to the destination.

bpsrc = $(filter-out %~,$(wildcard *--*))
boilerplates.made : $(bpsrc)
	ls *--* 2>/dev/null | \
	while read boilerplate; \
	do \
		case "$$boilerplate" in *~) continue ;; esac && \
		dst=`echo "$$boilerplate" | sed -e 's|^this|.|;s|--|/|g'` && \
		dir=`expr "$$dst" : '\(.*\)/'` && \
		mkdir -p blt/$$dir && \
		case "$$boilerplate" in \
		*--) ;; \
		*) cp $$boilerplate blt/$$dst ;; \
		esac || exit; \
	done || exit
	date >$@

# If you need build-tailored templates, build them into blt/
# directory yourself here.
custom:
	: no custom templates yet

clean:
	rm -rf blt boilerplates.made

install: all
	$(INSTALL) -d -m755 $(call shellquote,$(DESTDIR)$(template_dir))
	(cd blt && $(TAR) cf - .) | \
	(cd $(call shellquote,$(DESTDIR)$(template_dir)) && $(TAR) xf -)
