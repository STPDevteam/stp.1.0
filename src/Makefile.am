# Copyright (c) 2013-2016 The Bitcoin Core developers
# Copyright (c) 2017 The STP Core developers
# Distributed under the MIT software license, see the accompanying
# file COPYING or http://www.opensource.org/licenses/mit-license.php.

DIST_SUBDIRS = secp256k1 univalue

AM_LDFLAGS = $(PTHREAD_CFLAGS) $(LIBTOOL_LDFLAGS) $(HARDENED_LDFLAGS)
AM_CXXFLAGS = $(HARDENED_CXXFLAGS) $(ERROR_CXXFLAGS)
AM_CPPFLAGS = $(HARDENED_CPPFLAGS)
EXTRA_LIBRARIES =

if EMBEDDED_UNIVALUE
LIBUNIVALUE = univalue/libunivalue.la

$(LIBUNIVALUE): $(wildcard univalue/lib/*) $(wildcard univalue/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)
else
LIBUNIVALUE = $(UNIVALUE_LIBS)
endif

STP_INCLUDES=-I$(builddir) -I$(builddir)/obj $(BDB_CPPFLAGS) $(BOOST_CPPFLAGS) $(LEVELDB_CPPFLAGS) $(CRYPTO_CFLAGS) $(SSL_CFLAGS)

STP_INCLUDES += -I$(srcdir)/secp256k1/include
STP_INCLUDES += $(UNIVALUE_CFLAGS)

LIBSTP_SERVER=libstp_server.a
LIBSTP_COMMON=libstp_common.a
LIBSTP_CONSENSUS=libstp_consensus.a
LIBSTP_CLI=libstp_cli.a
LIBSTP_UTIL=libstp_util.a
LIBSTP_CRYPTO=crypto/libstp_crypto.a
LIBSTPQT=qt/libstpqt.a
LIBSECP256K1=secp256k1/libsecp256k1.la

if ENABLE_ZMQ
LIBSTP_ZMQ=libstp_zmq.a
endif
if BUILD_STP_LIBS
LIBSTPCONSENSUS=libstpconsensus.la
endif
if ENABLE_WALLET
LIBSTP_WALLET=libstp_wallet.a
endif

$(LIBSECP256K1): $(wildcard secp256k1/src/*) $(wildcard secp256k1/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)

# Make is not made aware of per-object dependencies to avoid limiting building parallelization
# But to build the less dependent modules first, we manually select their order here:
EXTRA_LIBRARIES += \
  $(LIBSTP_CRYPTO) \
  $(LIBSTP_UTIL) \
  $(LIBSTP_COMMON) \
  $(LIBSTP_CONSENSUS) \
  $(LIBSTP_SERVER) \
  $(LIBSTP_CLI) \
  $(LIBSTP_WALLET) \
  $(LIBSTP_ZMQ)

lib_LTLIBRARIES = $(LIBSTPCONSENSUS)

bin_PROGRAMS =
noinst_PROGRAMS =
TESTS =
BENCHMARKS =

if BUILD_STPD
  bin_PROGRAMS += stpd
endif

if BUILD_STP_CLI
  bin_PROGRAMS += stp-cli
endif

if BUILD_STP_TX
  bin_PROGRAMS += stp-tx
endif

.PHONY: FORCE check-symbols check-security
# stp core #
STP_CORE_H = \
  addrdb.h \
  addressindex.h \
  algo/sph_groestl.h \
  algo/sph_types.h \
  algo/sph_blake.h \
  algo/sph_bmw.h \
  algo/sph_jh.h \
  algo/sph_keccak.h \
  algo/sph_skein.h \
  algo/sph_luffa.h \
  algo/sph_cubehash.h \
  algo/sph_simd.h \
  algo/sph_echo.h \
  algo/sph_shavite.h \
  algo/sph_sha2.h \
  algo/sph_haval.h \
  algo/sph_tiger.h \
  algo/lyra2.h \
  algo/sponge.h \
  algo/gost_streebog.h \
  algo/hash_algos.h \
  algo/groestl.c \
  algo/blake.c \
  algo/bmw.c \
  algo/cubehash.c \
  algo/echo.c \
  algo/jh.c \
  algo/keccak.c \
  algo/luffa.c \
  algo/shavite.c \
  algo/simd.c \
  algo/skein.c \
  algo/hamsi.c \
  algo/whirlpool.c \
  algo/shabal.c \
  algo/fugue.c \
  algo/sha2.c \
  algo/extra.c \
  algo/extra.h \
  algo/sph_sha2big.c \
  algo/haval.c \
  algo/tiger.cpp \
  algo/lyra2.cpp \
  algo/sponge.cpp \
  algo/sph_sha2.c \
  algo/gost_streebog.c \
  spentindex.h \
  timestampindex.h \
  addrman.h \
  assets/assets.h \
  assets/assetdb.h \
  assets/assettypes.h \
  assets/restricteddb.h \
  base58.h \
  bloom.h \
  blockencodings.h \
  chain.h \
  chainparams.h \
  chainparamsbase.h \
  chainparamsseeds.h \
  checkpoints.h \
  checkqueue.h \
  clientversion.h \
  coins.h \
  compat.h \
  compat/byteswap.h \
  compat/endian.h \
  compat/sanity.h \
  compressor.h \
  consensus/consensus.h \
  consensus/tx_verify.h \
  core_io.h \
  core_memusage.h \
  cuckoocache.h \
  fs.h \
  httprpc.h \
  httpserver.h \
  indirectmap.h \
  init.h \
  key.h \
  keystore.h \
  dbwrapper.h \
  limitedmap.h \
  memusage.h \
  merkleblock.h \
  miner.h \
  net.h \
  net_processing.h \
  netaddress.h \
  netbase.h \
  netmessagemaker.h \
  noui.h \
  policy/feerate.h \
  policy/fees.h \
  policy/policy.h \
  policy/rbf.h \
  pow.h \
  protocol.h \
  random.h \
  reverse_iterator.h \
  reverselock.h \
  rpc/blockchain.h \
  rpc/client.h \
  rpc/mining.h \
  rpc/protocol.h \
  rpc/safemode.h \
  rpc/server.h \
  rpc/register.h \
  scheduler.h \
  script/sigcache.h \
  script/sign.h \
  script/standard.h \
  script/ismine.h \
  streams.h \
  support/allocators/secure.h \
  support/allocators/zeroafterfree.h \
  support/cleanse.h \
  support/events.h \
  support/lockedpool.h \
  sync.h \
  threadsafety.h \
  threadinterrupt.h \
  timedata.h \
  torcontrol.h \
  txdb.h \
  txmempool.h \
  ui_interface.h \
  undo.h \
  util.h \
  utilmoneystr.h \
  utiltime.h \
  validation.h \
  validationinterface.h \
  versionbits.h \
  wallet/coincontrol.h \
  wallet/crypter.h \
  wallet/db.h \
  wallet/feebumper.h \
  wallet/fees.h \
  wallet/init.h \
  wallet/rpcwallet.h \
  wallet/wallet.h \
  wallet/walletdb.h \
  warnings.h \
  zmq/zmqabstractnotifier.h \
  zmq/zmqconfig.h\
  zmq/zmqnotificationinterface.h \
  zmq/zmqpublishnotifier.h


obj/build.h: FORCE
	@$(MKDIR_P) $(builddir)/obj
	@$(top_srcdir)/share/genbuild.sh "$(abs_top_builddir)/src/obj/build.h" \
	  "$(abs_top_srcdir)"
libstp_util_a-clientversion.$(OBJEXT): obj/build.h

# server: shared between stpd and stp-qt
libstp_server_a_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES) $(MINIUPNPC_CPPFLAGS) $(EVENT_CFLAGS) $(EVENT_PTHREADS_CFLAGS)
libstp_server_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libstp_server_a_SOURCES = \
  addrdb.cpp \
  addrman.cpp \
  bloom.cpp \
  blockencodings.cpp \
  chain.cpp \
  checkpoints.cpp \
  consensus/consensus.cpp \
  consensus/tx_verify.cpp \
  httprpc.cpp \
  httpserver.cpp \
  init.cpp \
  dbwrapper.cpp \
  merkleblock.cpp \
  miner.cpp \
  net.cpp \
  net_processing.cpp \
  noui.cpp \
  assets/assets.cpp \
  assets/assetdb.cpp \
  assets/assettypes.cpp \
  policy/fees.cpp \
  policy/policy.cpp \
  policy/rbf.cpp \
  pow.cpp \
  rest.cpp \
  rpc/assets.cpp \
  rpc/blockchain.cpp \
  rpc/mining.cpp \
  rpc/misc.cpp \
  rpc/net.cpp \
  rpc/rawtransaction.cpp \
  rpc/safemode.cpp \
  rpc/server.cpp \
  script/sigcache.cpp \
  script/ismine.cpp \
  timedata.cpp \
  torcontrol.cpp \
  txdb.cpp \
  txmempool.cpp \
  ui_interface.cpp \
  validation.cpp \
  validationinterface.cpp \
  versionbits.cpp \
  $(STP_CORE_H)

if ENABLE_ZMQ
libstp_zmq_a_CPPFLAGS = $(STP_INCLUDES) $(ZMQ_CFLAGS)
libstp_zmq_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libstp_zmq_a_SOURCES = \
  zmq/zmqabstractnotifier.cpp \
  zmq/zmqnotificationinterface.cpp \
  zmq/zmqpublishnotifier.cpp
endif


# wallet: shared between stpd and stp-qt, but only linked
# when wallet enabled
libstp_wallet_a_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES)
libstp_wallet_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libstp_wallet_a_SOURCES = \
  wallet/crypter.cpp \
  wallet/db.cpp \
  wallet/feebumper.cpp \
  wallet/fees.cpp \
  wallet/init.cpp \
  wallet/rpcdump.cpp \
  wallet/rpcwallet.cpp \
  wallet/wallet.cpp \
  wallet/walletdb.cpp \
  $(STP_CORE_H)

# crypto primitives library
crypto_libstp_crypto_a_CPPFLAGS = $(AM_CPPFLAGS)
crypto_libstp_crypto_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
crypto_libstp_crypto_a_SOURCES = \
  crypto/aes.cpp \
  crypto/aes.h \
  crypto/chacha20.h \
  crypto/chacha20.cpp \
  crypto/common.h \
  crypto/hmac_sha256.cpp \
  crypto/hmac_sha256.h \
  crypto/hmac_sha512.cpp \
  crypto/hmac_sha512.h \
  crypto/ripemd160.cpp \
  crypto/aes_helper.c \
  crypto/sha1.cpp \
  crypto/sha1.h \
  crypto/sha256.cpp \
  crypto/sha256.h \
  crypto/sha512.h \
  crypto/sha512.cpp

if USE_ASM
crypto_libstp_crypto_a_SOURCES += crypto/sha256_sse4.cpp
endif

# consensus: shared between all executables that validate any consensus rules.
libstp_consensus_a_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES)
libstp_consensus_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libstp_consensus_a_SOURCES = \
  amount.h \
  arith_uint256.cpp \
  arith_uint256.h \
  consensus/merkle.cpp \
  consensus/merkle.h \
  consensus/params.h \
  consensus/validation.h \
  hash.cpp \
  hash.h \
  prevector.h \
  primitives/block.cpp \
  primitives/block.h \
  primitives/transaction.cpp \
  primitives/transaction.h \
  pubkey.cpp \
  pubkey.h \
  script/stpconsensus.cpp \
  script/interpreter.cpp \
  script/interpreter.h \
  script/script.cpp \
  script/script.h \
  script/script_error.cpp \
  script/script_error.h \
  serialize.h \
  support/cleanse.h \
  support/cleanse.cpp \
  tinyformat.h \
  uint256.cpp \
  uint256.h \
  utilstrencodings.cpp \
  utilstrencodings.h \
  version.h \
  $(STP_CORE_H)

# common: shared between stpd, and stp-qt and non-server tools
libstp_common_a_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES)
libstp_common_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libstp_common_a_SOURCES = \
  base58.cpp \
  chainparams.cpp \
  coins.cpp \
  compressor.cpp \
  core_read.cpp \
  core_write.cpp \
  key.cpp \
  keystore.cpp \
  netaddress.cpp \
  netbase.cpp \
  policy/feerate.cpp \
  protocol.cpp \
  scheduler.cpp \
  script/sign.cpp \
  script/standard.cpp \
  warnings.cpp \
  $(STP_CORE_H)

# util: shared between all executables.
# This library *must* be included to make sure that the glibc
# backward-compatibility objects and their sanity checks are linked.
libstp_util_a_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES)
libstp_util_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libstp_util_a_SOURCES = \
  support/lockedpool.cpp \
  chainparamsbase.cpp \
  clientversion.cpp \
  compat/glibc_sanity.cpp \
  compat/glibcxx_sanity.cpp \
  compat/strnlen.cpp \
  fs.cpp \
  random.cpp \
  rpc/protocol.cpp \
  support/cleanse.cpp \
  sync.cpp \
  threadinterrupt.cpp \
  util.cpp \
  utilmoneystr.cpp \
  utilstrencodings.cpp \
  utiltime.cpp \
  $(STP_CORE_H)

if GLIBC_BACK_COMPAT
libstp_util_a_SOURCES += compat/glibc_compat.cpp
AM_LDFLAGS += $(COMPAT_LDFLAGS)
endif

# cli: shared between stp-cli and stp-qt
libstp_cli_a_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES)
libstp_cli_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libstp_cli_a_SOURCES = \
  rpc/client.cpp \
  $(STP_CORE_H)

nodist_libstp_util_a_SOURCES = $(srcdir)/obj/build.h
#

# stpd binary #
stpd_SOURCES = stpd.cpp
stpd_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES)
stpd_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
stpd_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
stpd_SOURCES += stpd-res.rc
endif

stpd_LDADD = \
  $(LIBSTP_SERVER) \
  $(LIBSTP_COMMON) \
  $(LIBUNIVALUE) \
  $(LIBSTP_UTIL) \
  $(LIBSTP_WALLET) \
  $(LIBSTP_ZMQ) \
  $(LIBSTP_CONSENSUS) \
  $(LIBSTP_CRYPTO) \
  $(LIBLEVELDB) \
  $(LIBLEVELDB_SSE42) \
  $(LIBMEMENV) \
  $(LIBSECP256K1)

stpd_LDADD += $(BOOST_LIBS) $(BDB_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(MINIUPNPC_LIBS) $(EVENT_PTHREADS_LIBS) $(EVENT_LIBS) $(ZMQ_LIBS)

# stp-cli binary #
stp_cli_SOURCES = stp-cli.cpp
stp_cli_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES) $(EVENT_CFLAGS)
stp_cli_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
stp_cli_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
stp_cli_SOURCES += stp-cli-res.rc
endif

stp_cli_LDADD = \
  $(LIBSTP_CLI) \
  $(LIBUNIVALUE) \
  $(LIBSTP_UTIL) \
  $(LIBSTP_CRYPTO)

stp_cli_LDADD += $(BOOST_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(EVENT_LIBS)
#

# stp-tx binary #
stp_tx_SOURCES = stp-tx.cpp
stp_tx_CPPFLAGS = $(AM_CPPFLAGS) $(STP_INCLUDES)
stp_tx_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
stp_tx_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
stp_tx_SOURCES += stp-tx-res.rc
endif

stp_tx_LDADD = \
  $(LIBUNIVALUE) \
  $(LIBSTP_COMMON) \
  $(LIBSTP_UTIL) \
  $(LIBSTP_CONSENSUS) \
  $(LIBSTP_CRYPTO) \
  $(LIBSECP256K1)

stp_tx_LDADD += $(BOOST_LIBS) $(CRYPTO_LIBS)
#

# stpconsensus library #
if BUILD_STP_LIBS
include_HEADERS = script/stpconsensus.h
libstpconsensus_la_SOURCES = $(crypto_libstp_crypto_a_SOURCES) $(libstp_consensus_a_SOURCES)

if GLIBC_BACK_COMPAT
  libstpconsensus_la_SOURCES += compat/glibc_compat.cpp
endif

libstpconsensus_la_LDFLAGS = $(AM_LDFLAGS) -no-undefined $(RELDFLAGS)
libstpconsensus_la_LIBADD = $(LIBSECP256K1)
libstpconsensus_la_CPPFLAGS = $(AM_CPPFLAGS) -I$(builddir)/obj -I$(srcdir)/secp256k1/include -DBUILD_STP_INTERNAL
libstpconsensus_la_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)

endif
#

CTAES_DIST =  crypto/ctaes/bench.c
CTAES_DIST += crypto/ctaes/ctaes.c
CTAES_DIST += crypto/ctaes/ctaes.h
CTAES_DIST += crypto/ctaes/README.md
CTAES_DIST += crypto/ctaes/test.c

CLEANFILES = $(EXTRA_LIBRARIES)

CLEANFILES += *.gcda *.gcno
CLEANFILES += compat/*.gcda compat/*.gcno
CLEANFILES += consensus/*.gcda consensus/*.gcno
CLEANFILES += crypto/*.gcda crypto/*.gcno
CLEANFILES += policy/*.gcda policy/*.gcno
CLEANFILES += primitives/*.gcda primitives/*.gcno
CLEANFILES += script/*.gcda script/*.gcno
CLEANFILES += support/*.gcda support/*.gcno
CLEANFILES += univalue/*.gcda univalue/*.gcno
CLEANFILES += wallet/*.gcda wallet/*.gcno
CLEANFILES += wallet/test/*.gcda wallet/test/*.gcno
CLEANFILES += zmq/*.gcda zmq/*.gcno
CLEANFILES += obj/build.h

EXTRA_DIST = $(CTAES_DIST)


config/stp-config.h: config/stamp-h1
	@$(MAKE) -C $(top_builddir) $(subdir)/$(@)
config/stamp-h1: $(top_srcdir)/$(subdir)/config/stp-config.h.in $(top_builddir)/config.status
	$(AM_V_at)$(MAKE) -C $(top_builddir) $(subdir)/$(@)
$(top_srcdir)/$(subdir)/config/stp-config.h.in:  $(am__configure_deps)
	$(AM_V_at)$(MAKE) -C $(top_srcdir) $(subdir)/config/stp-config.h.in

clean-local:
	-$(MAKE) -C secp256k1 clean
	-$(MAKE) -C univalue clean
	-rm -f leveldb/*/*.gcda leveldb/*/*.gcno leveldb/helpers/memenv/*.gcda leveldb/helpers/memenv/*.gcno
	-rm -f config.h
	-rm -rf test/__pycache__

.rc.o:
	@test -f $(WINDRES)
	## FIXME: How to get the appropriate modulename_CPPFLAGS in here?
	$(AM_V_GEN) $(WINDRES) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(CPPFLAGS) -DWINDRES_PREPROC -i $< -o $@

check-symbols: $(bin_PROGRAMS)
if GLIBC_BACK_COMPAT
	@echo "Checking glibc back compat..."
	$(AM_V_at) READELF=$(READELF) CPPFILT=$(CPPFILT) $(top_srcdir)/contrib/devtools/symbol-check.py < $(bin_PROGRAMS)
endif

check-security: $(bin_PROGRAMS)
if HARDEN
	@echo "Checking binary security..."
	$(AM_V_at) READELF=$(READELF) OBJDUMP=$(OBJDUMP) $(top_srcdir)/contrib/devtools/security-check.py < $(bin_PROGRAMS)
endif

%.pb.cc %.pb.h: %.proto
	@test -f $(PROTOC)
	$(AM_V_GEN) $(PROTOC) --cpp_out=$(@D) --proto_path=$(<D) $<

if EMBEDDED_LEVELDB
include Makefile.leveldb.include
endif

if ENABLE_TESTS
include Makefile.test.include
endif

if ENABLE_BENCH
include Makefile.bench.include
endif

