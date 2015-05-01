INSTALLDIR = /usr/local
CC = gcc
CFLAGS = -O2 -Wall -ansi -pedantic
LIBS = -lpbm

#
# Below this point, should not need changing
#
EXES = cistopbm     pbmtocis
MAN1 = cistopbm.1   pbmtocis.1
MAN  = cistopbm.man pbmtocis.man
DOCS = cistopbm.txt pbmtocis.txt
SRC  = cistopbm.c   pbmtocis.c
OBJS = cistopbm.o   pbmtocis.o

DISTFILES = INSTALL Makefile cis.magic cisrle.txt \
	    $(MAN1) $(DOC) $(SRC)
DISTNAME  = cistopbm-1.0.1

all:	$(EXES) $(DOCS)

pbmtocis:	pbmtocis.o
	$(CC) -o $@ $< $(LIBS)

cistopbm:	cistopbm.o
	$(CC) -o $@ $< $(LIBS)

%.o:	%.c
	$(CC) -c $(CFLAGS) -o $@ $<

%.man:	%.1
	nroff -c -mandoc $< > $@

%.txt:	%.man
	col -b < $< > $@

tidy:
	rm -f $(OBJS) $(MAN)

clean:	tidy
	rm -f $(EXES) $(DOCS)

install:	$(EXES) $(DOCS)
	install -m 755 -o root $(EXES) $(INSTALLDIR)/bin
	install -m 644 -o root $(MAN1) $(INSTALLDIR)/man/man1


dist:
	rm -rf $(DISTNAME)
	mkdir $(DISTNAME)
	cp $(DISTFILES) $(DISTNAME)
	tar zchf $(DISTNAME).tar.gz $(DISTNAME)
	rm -rf $(DISTNAME)
	
