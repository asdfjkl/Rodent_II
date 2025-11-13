# Makefile for Rodent II (Linux + Windows cross compile)

# === Settings ===

BINDIR = .
DATADIR_LINUX = /usr/share/rodentII
DATADIR_WIN = .

EXENAME_LINUX = rodentII
EXENAME_WIN = rodentII.exe
CONFIGFILE = basic.ini

# === Compiler selection ===
# Default: native build
OS ?= linux

ifeq ($(OS),windows)
	CC = x86_64-w64-mingw32-g++
	EXENAME = $(EXENAME_WIN)
	DATADIR = $(DATADIR_WIN)
	LDFLAGS = -static -static-libgcc -static-libstdc++
	CFLAGS = -O3 -DNDEBUG -DBOOKPATH=\"$(DATADIR)\"
else
	CC = g++
	EXENAME = $(EXENAME_LINUX)
	DATADIR = $(DATADIR_LINUX)
	CFLAGS = -g -w -Wfatal-errors -pipe -DNDEBUG -O3 -fno-rtti -finline-functions -fprefetch-loop-arrays -DBOOKPATH=\"$(DATADIR)\"
	LDFLAGS = -s -lm
endif

.PHONY: all build build-static build-debug clean install help

# === Default ===
all: build

build:
	@echo "Building Rodent II for $(OS)..."
	$(CC) $(CFLAGS) $(LDFLAGS) -o $(EXENAME) -x c++ compile.linux
	echo -e "SHOW_OPTIONS\nPERSONALITY_BOOKS\nNPS_BLUR" > $(CONFIGFILE)

build-static:
	$(MAKE) build LDFLAGS="$(LDFLAGS) -static"

build-debug:
	$(MAKE) build CFLAGS="$(CFLAGS) -DWRITEDEBUGFILE"

clean:
	rm -f $(EXENAME)

install:
	mkdir -p -m 755 $(BINDIR)
	mkdir -p -m 755 $(DATADIR)
	cp $(EXENAME) $(BINDIR)
	cp $(CONFIGFILE) $(DATADIR)
	find -name '*.txt' -type f -exec cp '{}' $(DATADIR) \;
	cp -r ../books $(DATADIR)
	cp -r ../personalities $(DATADIR)
	chmod 755 $(BINDIR)/$(EXENAME)
	chmod -R 644 $(DATADIR)/*
	find $(DATADIR) -type d -exec chmod 755 '{}' \;

help:
	@echo ""
	@echo "Build targets:"
	@echo "  make build            - Build for Linux (default)"
	@echo "  make build OS=windows - Cross-compile for Windows"
	@echo "  make build-static     - Static Linux binary"
	@echo "  make build-debug      - Debug build"
	@echo "  make clean            - Remove built files"
