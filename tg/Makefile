srcdir=.

CFLAGS=-g -O2
LDFLAGS= -L/usr/local/lib -L/usr/lib -L/usr/lib  
CPPFLAGS= -I/usr/local/include -I/usr/include -I/usr/include -I/usr/include/lua5.2  
DEFS=-DHAVE_CONFIG_H
COMPILE_FLAGS=${CFLAGS} ${CPFLAGS} ${CPPFLAGS} ${DEFS} -Wall -Wextra -Werror -Wno-deprecated-declarations -fno-strict-aliasing -fno-omit-frame-pointer -ggdb -Wno-unused-parameter -fPIC

EXTRA_LIBS=-lconfig -lz -levent -lm   -lreadline -llua5.2  -ldl -lssl -lcrypto  
LOCAL_LDFLAGS=-rdynamic -ggdb -levent ${EXTRA_LIBS}
LINK_FLAGS=${LDFLAGS} ${LOCAL_LDFLAGS}

DEP=dep
AUTO=auto
EXE=bin
OBJ=objs
LIB=libs
DIR_LIST=${DEP} ${AUTO} ${EXE} ${OBJ} ${LIB} ${DEP}/auto ${OBJ}/auto

EXE_LIST=${EXE}/telegram-cli

TG_OBJECTS=${OBJ}/main.o ${OBJ}/loop.o ${OBJ}/interface.o ${OBJ}/lua-tg.o

INCLUDE=-I. -I${srcdir}
CC=gcc

.SUFFIXES:

.SUFFIXES: .c .h .o

all: ${EXE_LIST} ${DIR_LIST}
create_dirs_and_headers: ${DIR_LIST} ${AUTO}/auto.c ${AUTO}/auto-header.h ${AUTO}/constants.h
create_dirs: ${DIR_LIST}
dump-tl: ${EXE}/dump-tl-file

include ${srcdir}/Makefile.tl-parser
include ${srcdir}/Makefile.tgl

${DIR_LIST}:
	@test -d $@ || mkdir -p $@

OBJ_LIST=${TG_OBJECTS} ${TGL_OBJECTS} ${TGL_COMMON_OBJECTS} ${GENERATE_OBJECTS} ${TL_PARSER_OBJECTS}
DEPENDENCE=$(subst ${OBJ}/,${DEP}/,$(patsubst %.o,%.d,${OBJ_LIST}))
DEPENDENCE_LIST=${DEPENDENCE}

-include ${DEPENDENCE_LIST}

${TG_OBJECTS}: ${OBJ}/%.o: ${srcdir}/%.c | create_dirs_and_headers
	${CC} ${INCLUDE} ${COMPILE_FLAGS} -c -MP -MD -MF ${DEP}/$*.d -MQ ${OBJ}/$*.o -o $@ $<

${EXE}/telegram-cli: ${TG_OBJECTS} ${LIB}/libtgl.a
	${CC} $^ ${LINK_FLAGS} -o $@

clean:
	rm -rf ${DIR_LIST} config.log config.status > /dev/null || echo "all clean"

