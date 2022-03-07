STYLES = r b i bi
FONTS  = $(addprefix ptserif_,$(STYLES))
WOFF   = $(addsuffix .woff,$(FONTS))
WOFF2  = $(addsuffix .woff2,$(FONTS))

all: $(WOFF) $(WOFF2)

ptserifweb.zip:
	wget https://company.paratype.com/system/attachments/636/original/ptserifweb.zip

%.ttf: ptserifweb.zip
	unzip ptserifweb.zip $@

%.woff: %.ttf
	sfnt2woff-zopfli $<

%.woff2: %.ttf
	woff2_compress $<

ptserif_r.ttf: PTF55F_W.ttf
	mv $< $@

ptserif_b.ttf: PTF75F_W.ttf
	mv $< $@

ptserif_i.ttf: PTF56F_W.ttf
	mv $< $@

ptserif_bi.ttf: PTF76F_W.ttf
	mv $< $@

tidy:
	rm -f ptserifweb.zip *.ttf

clean: tidy
	rm -f $(WOFF) $(WOFF2)

.PHONY: all tidy clean
