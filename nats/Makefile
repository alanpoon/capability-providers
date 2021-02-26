CONST_FILE := src/lib.rs
define readvar
$(or $(shell sed -nE 's/^.*$(1).*"(.*)";$$/\1/p' $(CONST_FILE)),$(error "Cannot find variable $(1)"))
endef

CAPABILITY_ID := $(call readvar,CAPABILITY_ID)
NAME          := $(shell cargo metadata --no-deps --format-version 1 | jq -r '.packages[].name')
LIBNAME       := $(shell echo $(NAME) | tr '-' '_')
VENDOR        := waSCC

TARGETS := x86_64-apple-darwin
# not currently supported
MIPS_TARGETS := mips-unknown-linux-gnu \
	mipsel-unknown-linux-gnu \
	mips64-unknown-linux-gnuabi64 \
	mips64el-unknown-linux-gnuabi64

$(TARGETS):
	cross build --target $@

par: $(TARGETS)
	wash par create \
		--arch x86_64-macos \
		--binary target/x86_64-apple-darwin/debug/lib$(LIBNAME).dylib \
		--capid $(CAPABILITY_ID) \
		--name $(NAME) \
		--vendor $(VENDOR) \
		--compress \
		lib$(LIBNAME).par.gz
par2:
	wash par create \
		--arch x86_64-macos \
		--binary target/debug/lib$(LIBNAME).dylib \
		--capid $(CAPABILITY_ID) \
		--name $(NAME) \
		--vendor $(VENDOR) \
		--compress \
		lib$(LIBNAME).par.gz