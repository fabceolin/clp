set(CMAKE_LEGACY_CYGWIN_WIN32 0)
project(clp)
cmake_minimum_required(VERSION 2.8)
aux_source_directory(. SRC_LIST)

FIND_PACKAGE(SuiteSparse)

FILE(GLOB CLP_SRCS
    src/Clp*.cpp
    src/Cbc*.cpp
    src/Idi*.cpp
    src/MyMessageHandler.cpp
    src/MyEventHandler.cpp
    src/unitTest.cpp
    )

FILE(GLOB SUITESPARSE_LIBS
   ${CMAKE_INSTALL_PREFIX}/lib64/*.lib
   ${CMAKE_INSTALL_PREFIX}/lib64/lapack_blas_windows/*.lib
   )

LIST(REMOVE_ITEM CLP_SRCS
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyMumps.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyWssmp.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyWssmpKKT.cpp
    )

INCLUDE_DIRECTORIES(
    SYSTEM
    ${CMAKE_INSTALL_PREFIX}/include/suitesparse/
    ../CoinUtils/src/
    )

LINK_DIRECTORIES(
    ${CMAKE_INSTALL_PREFIX}/lib64
    ${CMAKE_INSTALL_PREFIX}/lib
    )

ADD_DEFINITIONS(/D__BYTE_ORDER=__LITTLE_ENDIAN /DHAVE_MATH_H /DCOIN_HAS_CHOLMOD /DCOIN_HAS_CLP )

if (${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
    SET(CMAKE_FIND_LIBRARY_PREFIXES "" "lib")
    SET(CMAKE_FIND_LIBRARY_SUFFIXES ".dll" ".lib" ".a" )
    FIND_LIBRARY(LIBCHOLMOD names cholmod cholmodd PATHS "${CMAKE_INSTALL_PREFIX}/lib64")
    FIND_LIBRARY(LIBCOINUTILS names CoinUtils.lib PATHS "${CMAKE_INSTALL_PREFIX}/lib")
ENDIF (${CMAKE_SYSTEM_NAME} STREQUAL "Windows")

ADD_EXECUTABLE(clp ${CLP_SRCS})
target_link_libraries(clp ${SUITESPARSE_LDFLAGS} ${LIBCHOLMOD} ${LIBCOINUTILS} ${SUITESPARSE_LIBS})

INSTALL(
    TARGETS
    clp
    DESTINATION
    "bin"
    )
