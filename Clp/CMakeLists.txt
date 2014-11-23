set(CMAKE_LEGACY_CYGWIN_WIN32 0)
project(clp)
cmake_minimum_required(VERSION 2.8)
aux_source_directory(. SRC_LIST)


# Underr MSWINDOWS
# call "c:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat" amd64 
# set CMAKEGENERATOR="NMake Makefiles"
# mkdir build-shared-debug
# cd build-shared-debug
# "c:\Program Files (x86)\CMake 2.8\bin\cmake.exe" -G%CMAKEGENERATOR% -DCMAKE_INSTALL_PREFIX=D:/msvc2013/shared/debug/ -DCMAKE_BUILD_TYPE=Debug ..



FIND_PACKAGE(SuiteSparse)

FILE(GLOB CLP_SRCS
    src/Clp*.cpp
    src/Cbc*.cpp
    src/Idi*.cpp
    src/MyMessageHandler.cpp
    src/MyEventHandler.cpp
    src/unitTest.cpp
    )

LIST(REMOVE_ITEM CLP_SRCS
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyMumps.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyWssmp.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyWssmpKKT.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyUfl.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/ClpCholeskyTaucs.cpp
    )

INCLUDE_DIRECTORIES(
    SYSTEM
    ../CoinUtils/src/
    )

LINK_DIRECTORIES(
    ${CMAKE_INSTALL_PREFIX}/lib64
    ${CMAKE_INSTALL_PREFIX}/lib
    )

ADD_DEFINITIONS(/D__BYTE_ORDER=__LITTLE_ENDIAN /DHAVE_MATH_H /DCOIN_HAS_CLP )

if (${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
    SET(CMAKE_FIND_LIBRARY_PREFIXES "" "lib")
    SET(CMAKE_FIND_LIBRARY_SUFFIXES ".dll" ".lib" ".a" )
    #    FIND_LIBRARY(LIBCHOLMOD names cholmod cholmodd PATHS "${CMAKE_INSTALL_PREFIX}/lib64")
    FIND_LIBRARY(LIBCOINUTILS names CoinUtils.lib PATHS "${CMAKE_INSTALL_PREFIX}/lib")
ENDIF (${CMAKE_SYSTEM_NAME} STREQUAL "Windows")

ADD_EXECUTABLE(clp ${CLP_SRCS})
target_link_libraries(clp ${SUITESPARSE_LDFLAGS} ${LIBCHOLMOD} ${LIBCOINUTILS} ${SUITESPARSE_LIBS})

# TODO - Copy include files

INSTALL(
    TARGETS
    clp
    DESTINATION
    "bin"
    )
