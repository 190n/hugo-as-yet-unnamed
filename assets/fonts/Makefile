# dependencies:
# - wget
# - unzip
# - sfnt2woff-zopfli
# - woff2
# - fonttools

STYLES = r b i bi
FONTS  = $(addprefix ptserif_,$(STYLES))
WOFF2  = $(addsuffix .woff2,$(FONTS)) inter_v.woff2

all: $(WOFF2)

# download ZIP of all fonts
ptserifweb.zip:
	wget https://company.paratype.com/system/attachments/636/original/ptserifweb.zip

Inter-3.19.zip:
	wget https://github.com/rsms/inter/releases/download/v3.19/Inter-3.19.zip

# subset the TTF files
# pyftsubset can only output WOFF/WOFF2. other tools produce smaller files in those formats, so we
# convert to WOFF and then back to TTF
%.sub.ttf: %.ttf
	pyftsubset $< --unicodes="U+0000-007F" --flavor=woff --output-file=$@.woff
	woff2sfnt-zopfli $@.woff > $@

# extract TTF files from the ZIP
PTF%.ttf: ptserifweb.zip
	unzip $< $@

Inter-roman.ttf: Inter-3.19.zip
	unzip -j $< "Inter Variable/Single axis/$@"

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

inter_v.ttf: Inter-roman.ttf
	mv $< $@

# delete all output files that we don't need to distribute
tidy:
	rm -f ptserifweb.zip Inter-3.19.zip *.ttf *.ttf.woff

# delete all output files
clean: tidy
	rm -f $(WOFF2)

.PHONY: all tidy clean
