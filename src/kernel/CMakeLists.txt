# Copyright (c) 2024-present The Bitcoin Core developers
# Distributed under the MIT software license, see the accompanying
# file COPYING or https://opensource.org/license/mit/.

include(GNUInstallDirs)

# TODO: libbotcoinkernel is a work in progress consensus engine
#       library, as more and more modules are decoupled from the
#       consensus engine, this list will shrink to only those
#       which are absolutely necessary.
add_library(botcoinkernel
  botcoinkernel.cpp
  chain.cpp
  checks.cpp
  chainparams.cpp
  coinstats.cpp
  context.cpp
  cs_main.cpp
  disconnected_transactions.cpp
  mempool_removal_reason.cpp
  ../arith_uint256.cpp
  ../chain.cpp
  ../coins.cpp
  ../compressor.cpp
  ../consensus/merkle.cpp
  ../consensus/tx_check.cpp
  ../consensus/tx_verify.cpp
  ../dbwrapper.cpp
  ../deploymentinfo.cpp
  ../deploymentstatus.cpp
  ../flatfile.cpp
  ../hash.cpp
  ../logging.cpp
  ../node/blockstorage.cpp
  ../node/chainstate.cpp
  ../node/utxo_snapshot.cpp
  ../policy/ephemeral_policy.cpp
  ../policy/feerate.cpp
  ../policy/packages.cpp
  ../policy/policy.cpp
  ../policy/rbf.cpp
  ../policy/settings.cpp
  ../policy/truc_policy.cpp
  ../pow.cpp
  ../primitives/block.cpp
  ../primitives/transaction.cpp
  ../pubkey.cpp
  ../random.cpp
  ../randomenv.cpp
  ../script/interpreter.cpp
  ../script/script.cpp
  ../script/script_error.cpp
  ../script/sigcache.cpp
  ../script/solver.cpp
  ../signet.cpp
  ../streams.cpp
  ../support/lockedpool.cpp
  ../sync.cpp
  ../txdb.cpp
  ../txgraph.cpp
  ../txmempool.cpp
  ../uint256.cpp
  ../util/chaintype.cpp
  ../util/check.cpp
  ../util/expected.cpp
  ../util/feefrac.cpp
  ../util/fs.cpp
  ../util/fs_helpers.cpp
  ../util/hasher.cpp
  ../util/moneystr.cpp
  ../util/rbf.cpp
  ../util/serfloat.cpp
  ../util/signalinterrupt.cpp
  ../util/syserror.cpp
  ../util/threadnames.cpp
  ../util/time.cpp
  ../util/tokenpipe.cpp
  ../validation.cpp
  ../validationinterface.cpp
  ../versionbits.cpp
  $<TARGET_OBJECTS:botcoin_clientversion>
  $<TARGET_OBJECTS:botcoin_crypto>
  $<TARGET_OBJECTS:leveldb>
  $<TARGET_OBJECTS:crc32c>
)
target_link_libraries(botcoinkernel
  PRIVATE
    core_interface
    secp256k1_objs
    $<$<PLATFORM_ID:Windows>:bcrypt>
    $<TARGET_NAME_IF_EXISTS:USDT::headers>
  PUBLIC
    Boost::headers
)

target_include_directories(botcoinkernel PRIVATE $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src/leveldb/include>)

# Add a convenience libbotcoinkernel target as a synonym for botcoinkernel.
add_custom_target(libbotcoinkernel)
add_dependencies(libbotcoinkernel botcoinkernel)

get_target_property(botcoinkernel_type botcoinkernel TYPE)
if(botcoinkernel_type STREQUAL "STATIC_LIBRARY")
  target_compile_definitions(botcoinkernel PUBLIC BITCOINKERNEL_STATIC)
endif()

configure_file(${PROJECT_SOURCE_DIR}/libbotcoinkernel.pc.in ${PROJECT_BINARY_DIR}/libbotcoinkernel.pc @ONLY)
install(FILES ${PROJECT_BINARY_DIR}/libbotcoinkernel.pc DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig" COMPONENT libbotcoinkernel)

install(TARGETS botcoinkernel
  RUNTIME
    DESTINATION ${CMAKE_INSTALL_BINDIR}
    COMPONENT libbotcoinkernel
  LIBRARY
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    COMPONENT libbotcoinkernel
  ARCHIVE
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    COMPONENT libbotcoinkernel
)

install(FILES botcoinkernel.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} COMPONENT libbotcoinkernel)
