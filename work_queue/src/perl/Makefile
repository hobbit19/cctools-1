CCTOOLS_HOME = ../../..

include ${CCTOOLS_HOME}/Makefile.config

ccflags=${CCTOOLS_PERL_CCFLAGS} -I${CCTOOLS_HOME}/dttools/src -I${CCTOOLS_HOME}/work_queue/src
ldflags=${CCTOOLS_PERL_LDFLAGS} ${CCTOOLS_HOME}/work_queue/src/libwork_queue.a ${CCTOOLS_HOME}/dttools/src/libdttools.a

all: work_queue.pm work_queue.${CCTOOLS_DYNAMIC_SUFFIX}

work_queue_wrap.c work_queue.pm: ../work_queue.i ${CCTOOLS_HOME}/work_queue/src/libwork_queue.a
	@echo "SWIG work_queue.i (perl5)"
	@${CCTOOLS_SWIG} -perl5 -exportall -I${CCTOOLS_HOME}/dttools/src -o work_queue_wrap.c ../work_queue.i

work_queue_wrap.o: work_queue_wrap.c
	${CCTOOLS_CC} -fPIC -Wno-unused-value -Wno-unused-variable -c $< -o $@ ${ccflags}

work_queue.${CCTOOLS_DYNAMIC_SUFFIX}: work_queue_wrap.o
	${CCTOOLS_LD} ${CCTOOLS_DYNAMIC_FLAG} $< -o $@ ${ldflags}

test: all

clean:
	rm -fr work_queue.pm work_queue.${CCTOOLS_DYNAMIC_SUFFIX} work_queue_wrap.*

PERL_INSTALL_DIR=$(CCTOOLS_INSTALL_DIR)/lib/perl5/site_perl/$(CCTOOLS_PERL_VERSION)

install: all
	mkdir -p $(PERL_INSTALL_DIR)
	mkdir -p ${CCTOOLS_INSTALL_DIR}/doc
	chmod 755 work_queue_example.pl
	cp work_queue.$(CCTOOLS_DYNAMIC_SUFFIX) $(PERL_INSTALL_DIR)
	cp work_queue.pm $(PERL_INSTALL_DIR)
	cp work_queue_example.pl ${CCTOOLS_INSTALL_DIR}/doc

