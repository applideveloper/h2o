VPATH=../srcdoc
OUTPUT=\
    index.html \
    install.html \
    configure.html \
    configure/quick_start.html \
    configure/command_options.html \
    configure/syntax_and_structure.html \
    faq.html \

PUBLISH_REPO=git@github.com:h2o/h2o.github.io.git
COMMIT=$(shell git rev-parse --short HEAD)

%.html: %.mt
	../misc/makedoc.pl $< $@

all: mkdir html search/searchindex.js

mkdir:
	mkdir -p doc/configure

html: $(OUTPUT)

search/searchindex.js: html
	../misc/oktavia/bin/oktavia-mkindex $(patsubst %,-i %,$(OUTPUT)) -m html -u h2 -c 10 -t js -s english

publish: all publish-check do-publish

publish-check:
	@CLEAN=`LANG=C git status . 2>&1 | grep 'working directory clean'` ; \
	if [ -z "$$CLEAN" ] ; then \
		echo "uncommitted changes exist; aborting" >&2 ; \
		exit 1 ; \
	fi
	$(MAKE) do-publish

do-publish:
	../misc/git-pushdir/git-pushdir -m "pushing docs of $(COMMIT)" "$(PUBLISH_REPO)"

clean:
	rm -f $(OUTPUT) search/searchindex.js

.PHONY: mkdir html publish publish-check do-publish
