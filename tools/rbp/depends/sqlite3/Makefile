include ../../Makefile.include
include ../depends.mk

# lib name, version
LIBNAME=sqlite
VERSION=3.6.11
SOURCE=$(LIBNAME)-$(VERSION)
ARCHIVE=$(SOURCE).tar.gz

# configuration settings
export CFLAGS+=-L${TARGETFS}/lib -L${TARGETFS}/usr/lib
export CXXFLAGS+=-DSQLITE_ENABLE_COLUMN_METADATA=1
export TCLLIBDIR=/dev/null
CONFIGURE=./configure --prefix=$(PREFIX) --host=$(HOST) --enable-shared \
  --enable-threadsafe --disable-tcl \
  --with-readline-inc="-I$(PREFIX)/include" \
  --with-readline-lib="-L$(PREFIX)/lib -lreadline -lm -lncurses"

LIBDYLIB=$(SOURCE)/.libs/lib$(LIBNAME).so

CLEAN_FILES=$(ARCHIVE) $(SOURCE)

all: $(LIBDYLIB) .installed

$(TARBALLS_LOCATION)/$(ARCHIVE):
	$(RETRIEVE_TOOL) $(RETRIEVE_TOOL_FLAGS) $(BASE_URL)/$(ARCHIVE)

$(SOURCE): $(TARBALLS_LOCATION)/$(ARCHIVE)
	rm -rf $(SOURCE)
	$(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(TARBALLS_LOCATION)/$(ARCHIVE)
	echo $(SOURCE) > .gitignore
	cd $(SOURCE); $(CONFIGURE)

$(LIBDYLIB): $(SOURCE)
	make -j $(JOBS) -C $(SOURCE)

.installed:
	make -C $(SOURCE) install
	touch $@

clean:
	make -C $(SOURCE) clean
	rm -f .installed

distclean::
	rm -rf $(SOURCE) .installed

