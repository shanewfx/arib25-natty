PREFIX = /usr/local
MAJOR = 0
MINOR = 2
REVISION = 4
VER = $(MAJOR).$(MINOR).$(REVISION)

DEST_HEADER = $(PREFIX)/include/arib25

CC     = gcc
PCSC_CFLAGS ?= `pkg-config libpcsclite --cflags`
CPPFLAGS = -Wall $(PCSC_CFLAGS) -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64
CFLAGS = -O2 -g -fPIC

PCSC_LIBS ?= `pkg-config libpcsclite --libs`
LIBS = $(PCSC_LIBS) -lm
LDFLAGS ?=

OBJS  = arib_std_b25.o b_cas_card.o multi2.o ts_section_parser.o
HEADERS = arib_std_b25.h b_cas_card.h portable.h
TARGET_APP = b25
TARGET_LIB = libarib25.so
TARGETS = $(TARGET_APP) $(TARGET_LIB)
DEPEND = Makefile.dep
SONAME = $(TARGET_LIB).$(MAJOR)

all: $(TARGETS)

clean:
	rm -f $(OBJS) td.o $(TARGETS) $(DEPEND)

$(TARGET_APP): $(OBJS) td.o
	$(CC) $(LDFLAGS) -o $(TARGET_APP) $(OBJS) td.o $(LIBS)

$(TARGET_LIB): $(OBJS)
	$(CC) $(LDFLAGS) -shared -o $(TARGET_LIB) $(OBJS) $(LIBS) -Wl,-soname,$(SONAME)

$(DEPEND):
	$(CC) -MM $(OBJS:.o=.c) $(CPPFLAGS) > $@

install: $(TARGET) install-headers
	install -m755 b25 $(PREFIX)/bin
	install -m755 $(TARGET_LIB) $(PREFIX)/lib/$(TARGET_LIB).$(VER)
	ln -sf $(PREFIX)/lib/$(TARGET_LIB).$(VER) $(PREFIX)/lib/$(TARGET_LIB).$(MAJOR)
	ln -sf $(PREFIX)/lib/$(TARGET_LIB).$(MAJOR) $(PREFIX)/lib/$(TARGET_LIB)
	ldconfig

install-headers:
	mkdir -p $(DEST_HEADER)
	install -m644 $(HEADERS) $(DEST_HEADER)

-include $(DEPEND)
