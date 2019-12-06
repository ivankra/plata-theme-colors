# Builds Plata theme and variants with different key colors using Docker.
# Produces tarballs and debian packages in ./dist/

# Commit, tag or branch of https://gitlab.com/tista500/plata-theme.git
# repository to check out and build
REVISION ?= master

# Material Design palette (https://www.google.com/design/spec/style/color.html)
Red300 = \#E57373
Red500 = \#F44336
RedA200 = \#FF5252
Pink300 = \#F06292
Pink500 = \#E91E63
Purple300 = \#BA68C8
Purple500 = \#9C27B0
DPurple300 = \#9575CD
DPurple400 = \#FF7043
DPurple500 = \#673AB7
Indigo300 = \#7986CB
Indigo500 = \#3F51B5
Blue300 = \#64B5F6
Blue500 = \#2196F3
BlueA400 = \#2979FF
LBlue300 = \#4FC3F7
LBlue500 = \#03A9F4
Cyan300 = \#4DD0E1
Cyan500 = \#00BCD4
Teal300 = \#4DB6AC
Teal500 = \#009688
Green300 = \#81C784
Green500 = \#4CAF50
LGreen300 = \#AED581
LGreen500 = \#8BC34A
Lime300 = \#DCE775
Lime500 = \#CDDC39
Lime700 = \#AFB42B
Yellow300 = \#FFF176
Yellow500 = \#FFEB3B
Amber300 = \#FFD54F
Amber400 = \#FFCA28
Amber500 = \#FFC107
Amber600 = \#FFB300
Amber700 = \#FFA000
Orange300 = \#FFB74D
Orange500 = \#FF9800
OrangeA200 = \#FFAB40
DOrange300 = \#FF8A65
DOrange400 = \#FF7043
DOrange500 = \#FF5722
Brown300 = \#A1887F
Brown500 = \#795548
Grey300 = \#E0E0E0
Grey400 = \#BDBDBD
Grey500 = \#9E9E9E
Grey600 = \#757575
Grey700 = \#616161
BlueGrey500 = \#607D8B
BlueGrey300 = \#90A4AE

all: dist

define tmpl
$(1): dist/$(1).tar.xz

dist/$(1).tar.xz:
	mkdir -p dist
	docker build \
	    --build-arg REVISION=$(REVISION) \
	    --build-arg THEME=$(1) \
	    --build-arg FLAGS="$(2)" \
	    --iidfile $(1).iid \
	    .
	docker run --cidfile $(1).cid "`cat $(1).iid`"
	docker cp "`cat $(1).cid`:/dist/." ./dist/
	docker rm -f "`cat $(1).cid`"
	#docker image rm -f "`cat $(1).iid`"
	rm -f $(1).cid $(1).iid
	echo "Built dist/$(1).tar.xz and related artefacts"

ALL_THEMES += $(1)
endef

$(eval $(call tmpl,Plata,))
$(eval $(call tmpl,Plata-Red,        --with-selection_color=$(Red500)      --with-accent_color=$(Red300)      --with-suggestion_color=$(DOrange500)))
$(eval $(call tmpl,Plata-Pink,       --with-selection_color=$(Pink500)     --with-accent_color=$(Pink300)     --with-suggestion_color=$(Red500)))
$(eval $(call tmpl,Plata-Purple,     --with-selection_color=$(Purple500)   --with-accent_color=$(Purple300)   --with-suggestion_color=$(DPurple500)))
$(eval $(call tmpl,Plata-DeepPurple, --with-selection_color=$(DPurple500)  --with-accent_color=$(DPurple300)  --with-suggestion_color=$(Indigo500)))
#$(eval $(call tmpl,Plata-Indigo,     --with-selection_color=$(Indigo500)   --with-accent_color=$(Indigo300)   --with-suggestion_color=$(DPurple500)))  # default colors
$(eval $(call tmpl,Plata-Blue,       --with-selection_color=$(Blue500)     --with-accent_color=$(Blue300)     --with-suggestion_color=$(Indigo500)))
$(eval $(call tmpl,Plata-LightBlue,  --with-selection_color=$(LBlue500)    --with-accent_color=$(LBlue300)    --with-suggestion_color=$(Indigo500)))
$(eval $(call tmpl,Plata-Cyan,       --with-selection_color=$(Cyan500)     --with-accent_color=$(Cyan300)     --with-suggestion_color=$(Teal500)))
$(eval $(call tmpl,Plata-Teal,       --with-selection_color=$(Teal500)     --with-accent_color=$(Teal300)     --with-suggestion_color=$(Teal500)))
$(eval $(call tmpl,Plata-Green,      --with-selection_color=$(Green500)    --with-accent_color=$(Green300)    --with-suggestion_color=$(Teal500)))
$(eval $(call tmpl,Plata-LightGreen, --with-selection_color=$(LGreen500)   --with-accent_color=$(LGreen300)   --with-suggestion_color=$(Green500)))
$(eval $(call tmpl,Plata-Lime,       --with-selection_color=$(Lime700)     --with-accent_color=$(Lime500)     --with-suggestion_color=$(LGreen500)))    # Lime500 is too bright
$(eval $(call tmpl,Plata-Amber,      --with-selection_color=$(Amber700)    --with-accent_color=$(Amber500)    --with-suggestion_color=$(Orange500)))    # Amber500 is too bright
$(eval $(call tmpl,Plata-Orange,     --with-selection_color=$(Orange500)   --with-accent_color=$(Orange300)   --with-suggestion_color=$(DOrange500)))
$(eval $(call tmpl,Plata-DeepOrange, --with-selection_color=$(DOrange500)  --with-accent_color=$(DOrange300)  --with-suggestion_color=$(Brown500)))
$(eval $(call tmpl,Plata-Brown,      --with-selection_color=$(Brown500)    --with-accent_color=$(Brown300)    --with-suggestion_color=$(Brown500)))
$(eval $(call tmpl,Plata-Grey,       --with-selection_color=$(Grey500)     --with-accent_color=$(Grey400)     --with-suggestion_color=$(BlueGrey500)))  # Grey300 is too bright
$(eval $(call tmpl,Plata-BlueGrey,   --with-selection_color=$(BlueGrey500) --with-accent_color=$(BlueGrey300) --with-suggestion_color=$(BlueGrey500)))

dist/plata-theme-colors-$(REVISION).tar.xz: $(ALL_THEMES)
	rm -rf plata-theme-colors-$(REVISION)
	mkdir -p plata-theme-colors-$(REVISION)
	for x in dist/Plata*.tar.xz; do \
	    (cd plata-theme-colors-$(REVISION) && tar xf "../$$x"); \
	done
	tar -cf dist/plata-theme-colors-$(REVISION).tar plata-theme-colors-$(REVISION)
	rm -f dist/plata-theme-colors-$(REVISION).tar.xz
	xz -9 dist/plata-theme-colors-$(REVISION).tar
	rm -rf plata-theme-colors-$(REVISION)

dist/plata-theme-colors-deb-$(REVISION).tar.xz: $(ALL_THEMES)
	rm -rf plata-theme-colors-deb-$(REVISION)
	mkdir -p plata-theme-colors-deb-$(REVISION)
	cp -a dist/plata*gtk-theme*.deb plata-theme-colors-deb-$(REVISION)/
	tar -jcf dist/plata-theme-colors-deb-$(REVISION).tar.xz plata-theme-colors-deb-$(REVISION)
	rm -rf plata-theme-colors-deb-$(REVISION)

ALL_TARGETS = $(ALL_THEMES) \
	      dist/plata-theme-colors-$(REVISION).tar.xz \
	      dist/plata-theme-colors-deb-$(REVISION).tar.xz

.PHONY: dist

dist: $(ALL_TARGETS)

clean:
	rm -rf dist plata-theme-colors-*
