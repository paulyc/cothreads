include ../../Makefile.template

INCLUDES= -I +threads -I ..

BACKEND = threads

AUXMOD = libext
AUXMODBYT = $(AUXMOD:%=%.cmo)
AUXMODNAT = $(AUXMOD:%=%.cmx)

COMMONMOD = thread mutex condition event
COMMONMODNAT =  $(COMMONMOD:%=%.cmx)
COMMONMODNATA = $(COMMONMOD:%=%.o)

EXTRAMOD = cothread stm
EXTRAMODBYT = $(EXTRAMOD:%=%.cmo)
EXTRAMODNAT = $(EXTRAMOD:%=%.cmx)
EXTRAMODINTFSRC = $(EXTRAMOD:%=%.mli)
EXTRAMODINTFCOM = $(EXTRAMOD:%=%.cmi)
EXTRAMODINTF = $(EXTRAMODINTFSRC) $(EXTRAMODINTFCOM)

LIB = cothreads
LIBBYT = $(LIB:%=%.cma)
LIBNAT = $(LIB:%=%.cmxa)
LIBNATA = $(LIB:%=%.a)

INSTALLDIR = $(INSTALLLIBDIR)/$(BACKEND)
INSTALLFILES = $(EXTRAMODINTFCOM) $(EXTRAMODNAT) $(LIBBYT) $(LIBNAT) $(LIBNATA)

$(EXTRAMODINTF):%:../%
	@if [ ! -e $@ -a -e $< ]; then ln -s $< .; fi

$(COMMONMODNAT): %: $(OCAMLSTDLIBPATH)/threads/%
	@if [ ! -e $@ -a -e $< ]; then ln -s $< .; fi

$(COMMONMODNATA):$(OCAMLSTDLIBPATH)/threads/threads.a
	$(AR) -x $<

$(LIBBYT): $(EXTRAMODBYT)
	$(OCAMLC) -a -o $@ threads.cma $(AUXMODBYT) $+
$(LIBNAT): $(COMMONMODNAT) $(COMMONMODNATA) $(EXTRAMODNAT) 
	$(OCAMLOPT) -a -o $@ $(COMMONMODNAT) $(AUXMODNAT) $(EXTRAMODNAT) -cclib -lthreadsnat -cclib -lunix -cclib -lpthread

.PHONY: all install clean uninstall

all: $(EXTRAMODINTF) $(LIBBYT) $(LIBNAT)

install: all
	$(MKDIR) $(INSTALLDIR)
	$(CP) $(INSTALLFILES) $(INSTALLDIR)

clean: ocamlclean

uninstall:
	for i in $(INSTALLFILES); do $(RM) $(INSTALLDIR)/$$i; done