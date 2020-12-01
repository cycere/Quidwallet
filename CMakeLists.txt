cmake_minimum_required(VERSION 2.8)

include(QuidWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(QUID_LIB quid)

include_directories(${CMAKE_BINARY_DIR}
    src
    quid/external
    quid/include
    quid/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("quid/src/version.h.in" "version.h")
configure_file("src/QuidWalletConfig.h.in" "QuidWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(QUID_SOURCES
  quid/external/miniupnpc/connecthostport.c
  quid/external/miniupnpc/igd_desc_parse.c
  quid/external/miniupnpc/minisoap.c
  quid/external/miniupnpc/miniupnpc.c
  quid/external/miniupnpc/miniwget.c
  quid/external/miniupnpc/minixml.c
  quid/external/miniupnpc/portlistingparse.c
  quid/external/miniupnpc/receivedata.c
  quid/external/miniupnpc/upnpcommands.c
  quid/external/miniupnpc/upnpreplyparse.c
  quid/src/Common/Base58.cpp
  quid/src/Common/CommandLine.cpp
  quid/src/Common/Util.cpp
  quid/src/Common/StringTools.cpp
  quid/src/Common/JsonValue.cpp
  quid/src/Common/ConsoleTools.cpp
  quid/src/Common/MemoryInputStream.cpp
  quid/src/Common/PathTools.cpp
  quid/src/Common/StdInputStream.cpp
  quid/src/Common/StdOutputStream.cpp
  quid/src/Common/StreamTools.cpp
  quid/src/Common/StringOutputStream.cpp
  quid/src/Common/StringView.cpp
  quid/src/Common/VectorOutputStream.cpp
  quid/src/crypto/blake256.c
  quid/src/crypto/chacha8.c
  quid/src/crypto/crypto-ops-data.c
  quid/src/crypto/crypto-ops.c
  quid/src/crypto/crypto.cpp
  quid/src/crypto/groestl.c
  quid/src/crypto/hash-extra-blake.c
  quid/src/crypto/hash-extra-groestl.c
  quid/src/crypto/hash-extra-jh.c
  quid/src/crypto/hash-extra-skein.c
  quid/src/crypto/hash.c
  quid/src/crypto/jh.c
  quid/src/crypto/keccak.c
  quid/src/crypto/oaes_lib.c
  quid/src/crypto/random.c
  quid/src/crypto/skein.c
  quid/src/crypto/slow-hash.c
  quid/src/crypto/slow-hash.cpp
  quid/src/crypto/tree-hash.c
  quid/src/QuidCore/BlockchainIndices.cpp
  quid/src/QuidCore/BlockchainMessages.cpp
  quid/src/QuidCore/BlockIndex.cpp
  quid/src/QuidCore/CoreConfig.cpp
  quid/src/QuidCore/QuidBasic.cpp
  quid/src/QuidCore/QuidTools.cpp
  quid/src/QuidCore/Currency.cpp
  quid/src/QuidCore/MinerConfig.cpp
  quid/src/QuidCore/Transaction.cpp
  quid/src/QuidCore/Account.cpp
  quid/src/QuidCore/Blockchain.cpp
  quid/src/QuidCore/Checkpoints.cpp
  quid/src/QuidCore/QuidBasicImpl.cpp
  quid/src/QuidCore/Core.cpp
  quid/src/QuidCore/QuidFormatUtils.cpp
  quid/src/QuidCore/QuidSerialization.cpp
  quid/src/QuidCore/Difficulty.cpp
  quid/src/QuidCore/IBlock.cpp
  quid/src/QuidCore/Miner.cpp
  quid/src/QuidCore/TransactionExtra.cpp
  quid/src/QuidCore/TransactionPool.cpp
  quid/src/QuidCore/TransactionPrefixImpl.cpp
  quid/src/QuidCore/TransactionUtils.cpp
  quid/src/InProcessNode/InProcessNode.cpp
  quid/src/InProcessNode/InProcessNodeErrors.cpp
  quid/src/NodeRpcProxy/NodeErrors.cpp
  quid/src/NodeRpcProxy/NodeRpcProxy.cpp
  quid/src/P2p/NetNodeConfig.cpp
  quid/src/Serialization/BinaryInputStreamSerializer.cpp
  quid/src/Serialization/BinaryOutputStreamSerializer.cpp
  quid/src/Serialization/JsonInputValueSerializer.cpp
  quid/src/Serialization/JsonOutputStreamSerializer.cpp
  quid/src/Serialization/KVBinaryInputStreamSerializer.cpp
  quid/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  quid/src/Serialization/SerializationOverloads.cpp
  quid/src/Transfers/BlockchainSynchronizer.cpp
  quid/src/Transfers/SynchronizationState.cpp
  quid/src/Transfers/TransfersConsumer.cpp
  quid/src/Transfers/TransfersContainer.cpp
  quid/src/Transfers/TransfersSubscription.cpp
  quid/src/Transfers/TransfersSynchronizer.cpp
  quid/src/Wallet/LegacyKeysImporter.cpp
  quid/src/Wallet/WalletAsyncContextCounter.cpp
  quid/src/Wallet/WalletErrors.cpp
  quid/src/WalletLegacy/KeysStorage.cpp
  quid/src/WalletLegacy/WalletLegacy.cpp
  quid/src/WalletLegacy/WalletHelper.cpp
  quid/src/WalletLegacy/WalletLegacySerializer.cpp
  quid/src/WalletLegacy/WalletLegacySerialization.cpp
  quid/src/WalletLegacy/WalletTransactionSender.cpp
  quid/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  quid/src/WalletLegacy/WalletUserTransactionsCache.cpp
  quid/src/System/ContextGroup.cpp
  quid/src/System/Event.cpp
  quid/src/System/EventLock.cpp
  quid/src/System/InterruptedException.cpp
  quid/src/System/Ipv4Address.cpp
  quid/src/System/TcpStream.cpp
  quid/src/HTTP/HttpRequest.cpp
  quid/src/HTTP/HttpParser.cpp
  quid/src/HTTP/HttpParserErrorCodes.cpp
  quid/src/HTTP/HttpResponse.cpp
  quid/src/Rpc/HttpClient.cpp
  quid/src/Rpc/JsonRpc.cpp
  quid/src/P2p/NetNode.cpp
  quid/src/P2p/LevinProtocol.cpp
  quid/src/P2p/NetNodeConfig.cpp
  quid/src/P2p/PeerListManager.cpp
  quid/src/QuidProtocol/QuidProtocolHandler.cpp
  quid/src/Logging/ILogger.cpp
  quid/src/Logging/LoggerGroup.cpp
  quid/src/Logging/CommonLogger.cpp
  quid/src/Logging/LoggerManager.cpp
  quid/src/Logging/FileLogger.cpp
  quid/src/Logging/StreamLogger.cpp
  quid/src/Logging/ConsoleLogger.cpp
  quid/src/Logging/LoggerMessage.cpp
  quid/src/Logging/LoggerRef.cpp
  quid/src/BlockchainExplorer/BlockchainExplorer.cpp
  quid/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  quid/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(quid/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/quidwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(QUID_SOURCES ${QUID_SOURCES} quid/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "quid GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME quidWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE quid.icns)
    set(APPLICATION_ICON src/images/quid.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(quid/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES quid/src/Platform/${PLATFORM_DIR}/System/*)
set(QUID_SOURCES ${QUID_SOURCES} ${PLATFORM_SOURCES})

add_library(${QUID_LIB} STATIC ${QUID_SOURCES})
set_target_properties(${QUID_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${QUID_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${QUID_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
