#
# Makefile for the GraphLCD driver library
#

-include ../Make.config

CXXFLAGS += -fPIC

VERMAJOR = 1
VERMINOR = 0
VERMICRO = 0

BASENAME = libglcddrivers.so

LIBNAME = $(BASENAME).$(VERMAJOR).$(VERMINOR).$(VERMICRO)

OBJS = common.o config.o driver.o drivers.o port.o simlcd.o framebuffer.o gu140x32f.o gu256x64-372.o gu256x64-3900.o hd61830.o ks0108.o image.o sed1330.o sed1520.o t6963c.o noritake800.o serdisp.o avrctl.o g15daemon.o network.o gu126x64D-K610A4.o dm140gink.o

HEADERS = config.h driver.h drivers.h

ifeq ($(shell pkg-config --exists libhid && echo 1), 1)
OBJS += futabaMDM166A.o
INCLUDES += $(shell pkg-config --cflags libhid)
LIBS += $(shell pkg-config --libs libhid)
DEFINES += -DHAVE_LIBHID
endif

### Implicit rules:

%.o: %.c
	$(CXX) $(CXXFLAGS) -c $(DEFINES) $(INCLUDES) $<

# Dependencies:

MAKEDEP = g++ -MM -MG
DEPFILE = .dependencies
$(DEPFILE): Makefile
	@$(MAKEDEP) $(DEFINES) $(INCLUDES) $(OBJS:%.o=%.c) > $@

-include $(DEPFILE)

### Targets:

all: $(LIBNAME)

$(LIBNAME): $(OBJS)
	$(CXX) $(CXXFLAGS) $(LDFLAGS) -shared $(OBJS) $(LIBS) -ldl -lpthread -Wl,-soname="$(BASENAME).$(VERMAJOR)" -o $@
	ln -sf $(LIBNAME) $(BASENAME)

install: all
	install -d $(LIBDIR)
	install -m 755 $(LIBNAME) $(LIBDIR)/
	install -d $(INCDIR)/glcddrivers
	install -m 644 $(HEADERS) $(INCDIR)/glcddrivers/
	( cd $(LIBDIR); ln -sf $(LIBNAME) $(BASENAME).$(VERMAJOR); ln -sf $(LIBNAME) $(BASENAME) )

uninstall:
	rm -f $(LIBDIR)/$(BASENAME)
	rm -f $(LIBDIR)/$(BASENAME).$(VERMAJOR)
	rm -f $(LIBDIR)/$(LIBNAME)
	(for i in $(HEADERS); do rm -f $(INCDIR)/glcddrivers/$$i; done)
	rmdir $(INCDIR)/glcddrivers

clean:
	rm -f $(OBJS) $(DEPFILE) $(LIBNAME) $(BASENAME) *~

