
# these default settings can be overridden by setting environment variables
PREFIX   ?= /usr/local
LIBDIR   ?= $(PREFIX)/lib
CXX      ?= g++
CXXFLAGS ?= -O3
AR       ?= ar
PROTOC   ?= protoc

CXXFLAGS += -Wall -Wextra -pedantic -Wredundant-decls -Wdisabled-optimization -Wctor-dtor-privacy -Wnon-virtual-dtor -Woverloaded-virtual -Wsign-promo -Wno-long-long

# Added c settings
CC		 ?= gcc
CFLAGS   ?= -O3 -std=c99 -Wall -Wextra -pedantic
PROTOC-C ?= protoc-c

.PHONY: clean install

# Changed to use C versions
all: libosmpbf.a ../include/osmpbf/fileformat.pb-c.h ../include/osmpbf/osmformat.pb-c.h
# all: libosmpbf.a ../include/osmpbf/fileformat.pb.h ../include/osmpbf/osmformat.pb.h

# Changed to use C versions
libosmpbf.a: fileformat.pb-c.o osmformat.pb-c.o
	$(AR) -cr $@ fileformat.pb-c.o osmformat.pb-c.o
# libosmpbf.a: fileformat.pb.o osmformat.pb.o
	#$(AR) -cr $@ fileformat.pb.o osmformat.pb.o

# C++ (original)
%.pb.o: %.pb.cc
	$(CXX) $(CXXFLAGS) -fPIC -c -o $@ $<

%.pb.cc ../include/osmpbf/%.pb.h: %.proto
	$(PROTOC) --proto_path=. --cpp_out=. $<
	cp -v $(subst .proto,.pb.h,$<) ../include/osmpbf/

# C (custom)
%.pb-c.o: %.pb-c.c
	$(CC) $(CFLAGS) -fPIC -c -o $@ $<

%.pb-c.c ../include/osmpbf/%.pb-c.h: %.proto
	$(PROTOC-C) --proto_path=. --c_out=. $<
	cp -v $(subst .proto,.pb-c.h,$<) ../include/osmpbf/

install:
	# Unchanged
	install -m 755 -d $(DESTDIR)$(LIBDIR)
	install -m 644 libosmpbf.a $(DESTDIR)$(LIBDIR)
	install -m 755 -d $(DESTDIR)$(PREFIX)/include/osmpbf
	install -m 644 ../include/osmpbf/osmpbf.h $(DESTDIR)$(PREFIX)/include/osmpbf

	# Changed to use C versions
	install -m 644 ../include/osmpbf/fileformat.pb-c.h $(DESTDIR)$(PREFIX)/include/osmpbf
	install -m 644 ../include/osmpbf/osmformat.pb-c.h $(DESTDIR)$(PREFIX)/include/osmpbf

	# install -m 644 ../include/osmpbf/fileformat.pb.h $(DESTDIR)$(PREFIX)/include/osmpbf
	# install -m 644 ../include/osmpbf/osmformat.pb.h $(DESTDIR)$(PREFIX)/include/osmpbf

clean:
	# Added C version to clean
	rm -f *.pb.h *.pb.cc *.pb.o *.pb-c.h *.pb-c.c *.pb-c.o libosmpbf.a
