GCC_FLAGS=-DREDISMODULE_EXPERIMENTAL_API -g -ggdb
LD_FLAGS=-g -ggdb
ifeq ($(DEBUG),1)
	GCC_FLAGS+=-O0
else
	GCC_FLAGS+=-O3
endif

GCC_FLAGS+=-fvisibility=hidden -fPIC -DREDISMODULE_EXPERIMENTAL_API \
-I../deps/hiredis/ \
-I../deps/hiredis/adapters/ \
-I../deps/libevent/include/

ifeq ($(COVERAGE),1)
	GCC_FLAGS+=-fprofile-arcs -ftest-coverage
	LD_FLAGS+=-fprofile-arcs -lgcov
endif

SOURCES=mr.o record.o cluster.o event_loop.o crc16.o utils/adlist.o utils/buffer.o utils/dict.o utils/thpool.o utils/siphash.o

HIREDIS=../deps/hiredis/libhiredis.a
HIREDIS_SSL=../deps/hiredis/libhiredis_ssl.a
LIBEVENT=../deps/libevent/.libs/libevent.a
LIBEVENT_PTHREADS=../deps/libevent/.libs/libevent_pthreads.a

ifeq ($(shell $(CC) -v 2>&1 | grep -c "clang version"), 1)
COMPILER = clang
else
COMPILER = gcc
endif

ARTIFACT_NAME=libmr

all: $(ARTIFACT_NAME)

%.o : %.c
	gcc -c $(GCC_FLAGS) $< -o $@ -DMODULE_NAME=$(MODULE_NAME)


$(ARTIFACT_NAME): $(SOURCES)
	gcc $(SOURCES) $(HIREDIS) $(HIREDIS_SSL) $(LIBEVENT) $(LIBEVENT_PTHREADS) -r -o $(ARTIFACT_NAME).o $(LD_FLAGS) 
    ifeq ($(COMPILER),gcc)
		objcopy --localize-hidden $(ARTIFACT_NAME).o
    endif
	ar rcs $(ARTIFACT_NAME).a $(ARTIFACT_NAME).o

clean:
	rm -f $(ARTIFACT_NAME).a *.o utils/*.o \
	*.gcno utils/*.gcno \
	*.gcda utils/*.gcda \
	*.gcov utils/*.gcov \
	*.out \
	*.html utils/*.html
