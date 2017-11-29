# Unix makefile for the JBIG-KIT library

# Select an ANSI/ISO C compiler here, GNU gcc is recommended
CC = gcc

# Options for the compiler: A high optimization level is suggested
CFLAGS = ${RPM_OPT_FLAGS} -W -Wall -ansi -pedantic # --coverage
LDFLAGS ?= -Zhigh-mem -Zomf
LIBS ?= -lcx
JBIGLIBS = $(LIBS) -ljbig
JBIG85LIBS = $(LIBS) -ljbig85
EXEEXT ?= .exe
VENDOR ?=community
BUILD_INFO=\#\#1\#\# $(shell date +'%d %b %Y %H:%M:%S')     $(shell uname -n)
BUILDLEVEL_INFO=@\#$(VENDOR):2.1.3\#@$(BUILD_INFO)::::0::

all: libjbig.a libjbig85.a tstcodec$(EXEEXT) tstcodec85$(EXEEXT)

tstcodec$(EXEEXT): tstcodec.o libjbig.a
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ tstcodec.o $(JBIGLIBS)

tstcodec85$(EXEEXT): tstcodec85.o libjbig85.a
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ tstcodec85.o $(JBIG85LIBS)

libjbig.a: jbig.o jbig_ar.o
	rm -f libjbig.a
	ar rc libjbig.a jbig.o jbig_ar.o
	-ranlib libjbig.a

libjbig85.a: jbig85.o jbig_ar.o
	rm -f libjbig85.a
	ar rc libjbig85.a jbig85.o jbig_ar.o
	-ranlib libjbig85.a

jbig.o: jbig.c jbig.h jbig_ar.h
jbig85.o: jbig85.c jbig85.h jbig_ar.h
jbig_ar.o: jbig_ar.c jbig_ar.h
tstcodec.o: tstcodec.c jbig.h
tstcodec85.o: tstcodec85.c jbig85.h

update-po: jbig.c jbig85.c Makefile
	xgettext -ojbig.pot -k_ \
	  --copyright-holder='Markus Kuhn' \
	  --msgid-bugs-address='http://www.cl.cam.ac.uk/~mgk25/jbigkit/' \
	  --package-name jbigkit \
	jbig.c jbig85.c
	cd po && for po in *.po ; do \
	  msgmerge --update $$po ../jbig.pot ; done

analyze:
	clang --analyze *.c

test: tstcodec$(EXEEXT) tstcodec85$(EXEEXT)
	./tstcodec
	./tstcodec85

t82test.pbm: tstcodec$(EXEEXT)
	./tstcodec $@

clean:
	rm -f *.o *.gcda *.gcno *.gcov *.plist *~ core gmon.out dbg_d\=??.pbm
	rm -f *.a
	rm -f t82test.pbm
	rm -f tstcodec${EXEEXT} tstcodec85${EXEEXT}
