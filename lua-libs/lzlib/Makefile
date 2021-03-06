# $Id: Makefile,v 1.8 2004/07/22 19:10:47 tngd Exp $
# makefile for zlib library for Lua

# dist location
DISTDIR=dist
TMP=/tmp

# change these to reflect your Lua installation
LUA= $(HOME)/local/lua-5.1
LUAINC= $(LUA)/include
LUALIB= $(LUA)/lib
LUABIN= $(LUA)/bin

ZLIB=./

CC = gcc

# no need to change anything below here
CFLAGS= $(INCS) $(DEFS) $(WARN) -O3 -fPIC
WARN= -Werror -Wall -pedantic #-ansi
INCS= -I$(LUAINC) -I/usr/local/include -I/usr/local/include/luajit-2.0 -I/usr/local/include/luajit-2.1 -I$(ZLIB) $(LIBLUA)
LIBS= -L$(ZLIB) -lz $(LIBLUA) -llua

ifneq (,$(findstring luajit,$(LIBLUA)))
LIBS= -L$(ZLIB) -lz $(LIBLUA)
endif

MYLIB=lzlib

ZLIB_NAME = zlib
GZIP_NAME = gzip

T_ZLIB= $(ZLIB_NAME).so
T_GZIP= $(GZIP_NAME).so

VER=0.4-work3
TARFILE = $(DISTDIR)/$(MYLIB)-$(VER).tar.gz
TARFILES = Makefile README README.lgzip CHANGES \
	lzlib.c gzip.lua \
	test_zlib2.lua \
	test_zlib3.lua \
	test_gzip.lua \
	test_prologue.lua

all: $(T_ZLIB) # $(T_GZIP)

test: $(T_ZLIB) # $(T_GZIP)
	$(LUABIN)/lua -lluarc test_prologue.lua
	$(LUABIN)/lua -lluarc test_gzip.lua
	$(LUABIN)/lua -lluarc test_zlib2.lua
	$(LUABIN)/lua -lluarc test_zlib3.lua

$(T_ZLIB): lzlib.o
	$(CC) -o $@ -shared $(LIBS) $<

clean:
	rm -f *.o *.so core core.* a.out

dist: $(TARFILE)

$(TARFILE): $(TARFILES)
	@ln -sf `pwd` $(TMP)/$(MYLIB)-$(VER)
	tar -zcvf $(TARFILE) -C $(TMP) $(addprefix $(MYLIB)-$(VER)/,$(TARFILES))
	@rm -f $(TMP)/$(MYLIB)-$(VER)
	@# @lsum $(TARFILE) $(DISTDIR)/md5sums.txt
