# Copyright (c) 2009-2010 Satoshi Nakamoto
# Distributed under the MIT/X11 software license, see the accompanying
# file license.txt or http://www.opensource.org/licenses/mit-license.php.

CXX=g++

DEFS=-DNOPCH -DFOURWAYSSE2 -DUSE_SSL

# for boost 1.37, add -mt to the boost libraries
LIBS= \
 -Wl,-Bstatic \
   -l boost_system \
   -l boost_filesystem \
   -l boost_program_options \
   -l boost_thread \
   -l db_cxx \
   -l ssl \
   -l crypto

LIBS+= \
 -Wl,-Bdynamic \
   -l gthread-2.0 \
   -l z \
   -l dl \
   -l pthread

CXXFLAGS=-O2 -Wno-invalid-offsetof -Wformat $(DEFS)
HEADERS=headers.h strlcpy.h serialize.h uint256.h util.h key.h bignum.h base58.h \
    script.h db.h net.h irc.h keystore.h main.h wallet.h rpc.h noui.h init.h

OBJS= \
    obj/util.o \
    obj/script.o \
    obj/db.o \
    obj/net.o \
    obj/irc.o \
    obj/keystore.o \
    obj/main.o \
    obj/wallet.o \
    obj/rpc.o \
    obj/init.o \
    cryptopp/obj/sha.o \
    cryptopp/obj/cpu.o


all: bitcoind

cryptopp/obj/%.o: cryptopp/%.cpp
	$(CXX) -c $(CXXFLAGS) -O3 -o $@ $<

obj/nogui/%.o: %.cpp $(HEADERS)
	$(CXX) -c $(CXXFLAGS) -o $@ $<

bitcoind: $(OBJS:obj/%=obj/nogui/%)
	$(CXX) $(CXXFLAGS) -o $@ $^ $(LIBS)

obj/test/%.o: test/%.cpp $(HEADERS)
	$(CXX) -c $(CFLAGS) -o $@ $<

test_bitcoin: obj/test/test_bitcoin.o
	$(CXX) $(CFLAGS) -o $@ $(LIBPATHS) $^ $(LIBS) -lboost_unit_test_framework

clean:
	-rm -f bitcoind test_bitcoin
	-rm -f obj/*.o
	-rm -f obj/nogui/*.o
	-rm -f obj/test/*.o
	-rm -f cryptopp/obj/*.o
	-rm -f headers.h.gch
