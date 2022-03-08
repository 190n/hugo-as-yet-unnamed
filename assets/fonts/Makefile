# dependencies:
# - wget
# - unzip
# - sfnt2woff-zopfli
# - woff2
# - fonttools

STYLES = r b i bi
FONTS  = $(addprefix ptserif_,$(STYLES))
WOFF   = $(addsuffix .woff,$(FONTS))
WOFF2  = $(addsuffix .woff2,$(FONTS))

all: $(WOFF) $(WOFF2)

# download ZIP of all fonts
ptserifweb.zip:
	wget https://company.paratype.com/system/attachments/636/original/ptserifweb.zip

# subset the TTF files
# pyftsubset can only output WOFF/WOFF2. other tools produce smaller files in those formats, so we
# convert to WOFF and then back to TTF
%.sub.ttf: %.ttf
	pyftsubset $< --unicodes="U+0000-007F" --flavor=woff --output-file=$@.woff
	woff2sfnt-zopfli $@.woff > $@

# extract TTF files from the ZIP
%.ttf: ptserifweb.zip
	unzip ptserifweb.zip $@

# compress TTF to WOFF
%.woff: %.sub.ttf
	sfnt2woff-zopfli $<
	mv $(basename $<).woff $@

# compress TTF to WOFF2
%.woff2: %.sub.ttf
	woff2_compress $<
	mv $(basename $<).woff2 $@

# rename files from the ZIP
ptserif_r.ttf: PTF55F_W.ttf
	mv $< $@

ptserif_b.ttf: PTF75F_W.ttf
	mv $< $@

ptserif_i.ttf: PTF56F_W.ttf
	mv $< $@

ptserif_bi.ttf: PTF76F_W.ttf
	mv $< $@

# delete all output files that we don't need to distribute
tidy:
	rm -f ptserifweb.zip *.ttf *.ttf.woff

# delete all output files
clean: tidy
	rm -f $(WOFF) $(WOFF2)

.PHONY: all tidy clean
