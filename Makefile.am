AUTOMAKE_OPTIONS = foreign no-dependencies

program = beanstalkd

bin_PROGRAMS = $(program)
beanstalkd_SOURCES = beanstalkd.c \
					 conn.c \
					 job.c \
					 ms.c \
					 net.c \
					 pq.c \
					 primes.c \
					 prot.c \
					 tube.c \
					 util.c

cutgen_CFLAGS = -D__LINUX__

EXTRA_DIST = conn.h job.h ms.h net.h pq.h primes.h prot.h stat.h tube.h util.h

tests = $(abs_srcdir)/tests/test_conn.c \
		$(abs_srcdir)/tests/test_job.c \
		$(abs_srcdir)/tests/test_ms.c \
		$(abs_srcdir)/tests/test_net.c \
		$(abs_srcdir)/tests/test_pq.c \
		$(abs_srcdir)/tests/test_prot.c \
		$(abs_srcdir)/tests/test_reserve.c \
		$(abs_srcdir)/tests/test_tube.c \
		$(abs_srcdir)/tests/test_util.c

check: $(program) tests/cutcheck 
	tests/cutcheck
	@echo

tests/cutcheck.c: $(tests) tests/cutgen
	tests/cutgen -o tests/cutcheck.c $(tests)

tests/cutcheck.o: tests/cutcheck.c
	$(CC) $(CFLAGS) -I$(abs_srcdir)/tests/ -c tests/cutcheck.c -o tests/cutcheck.o

tests/cutcheck: tests/cutcheck.o
	for file in `ls $(abs_srcdir)/tests/test_*.c`; do  cd $(abs_builddir)/tests/ && $(CC) $(CFLAGS) -I$(abs_srcdir)/tests/ -c $$file;  done;
	$(CC) $(CFLAGS) -I$(abs_srcdir)/tests/ $(LDFLAGS) $(LIBS) -pg `ls *.o | grep -v $(program)` `ls $(abs_builddir)/tests/test_*.o` tests/cutcheck.o -o tests/cutcheck

tests/cutgen: $(abs_srcdir)/tests/cutgen.c
	@if ! test -d tests; then mkdir tests; fi
	$(CC) $(CFLAGS) $(cutgen_CFLAGS) -I$(abs_srcdir)/tests/ $(LDFLAGS) $(abs_srcdir)/tests/cutgen.c -o tests/cutgen

clean:
	rm -f *.o tests/*.o tests/cutgen tests/cutcheck*
	rm -f $(program) .*.d core core.* gmon.out
	rm -f $(program)-*.tar.gz

