PKG     := uc-tool-mx
BIN     := uc-tool
MAN     := uc-tool.1
SRCDIR  := $(PKG)
SCRIPT  := ../$(BIN)

.PHONY: all help deb release prepare clean sign

all: help

help:
	@echo "Usage: make <target>"
	@echo ""
	@echo "  deb       Build unsigned .deb (for local testing)"
	@echo "  release   Build signed .deb for upload"
	@echo "  sign      Alias for release"
	@echo "            Override signing key: make release GPGKEY=ABCDEF1234"
	@echo "  clean     Remove build artifacts and prepared source files"
	@echo ""
	@echo "Tip: run './build' from the repo root (wraps make from deb-src/)"

# Unsigned build for local testing.
deb: prepare
	cd $(SRCDIR) && debuild -uc -us

# Signed release build.  Override GPGKEY to use a specific key:
#   make release GPGKEY=ABCDEF1234
release: prepare
	cd $(SRCDIR) && debuild $(if $(GPGKEY),-k$(GPGKEY),)
	@echo ""
	@echo "Built: $$(ls -1 $(PKG)_*.deb 2>/dev/null | tail -1)"

prepare:
	install -m 0755 ../$(BIN)                  $(SRCDIR)/$(BIN)
	cp ../$(MAN)                               $(SRCDIR)/$(MAN)
	cp ../README.md                            $(SRCDIR)/README.md
	install -Dm0644 ../$(BIN).bash-completion  $(SRCDIR)/completions/$(BIN)

sign: release

clean:
	#rm -f $(PKG)_*.deb $(PKG)_*.buildinfo $(PKG)_*.changes
	rm $(PKG)_*.build          || true
	rm -f $(SRCDIR)/$(BIN)     || true
	rm -f $(SRCDIR)/$(MAN)     || true
	rm -f $(SRCDIR)/README.md  || true
	rm -f $(SRCDIR)/completions/$(BIN) || true
	rmdir $(SRCDIR)/completions || true
	cd $(SRCDIR) && debian/rules clean 2>/dev/null || true
