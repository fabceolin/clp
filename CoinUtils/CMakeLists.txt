set(CMAKE_LEGACY_CYGWIN_WIN32 0)
project(CoinUtils)
cmake_minimum_required(VERSION 2.8)
aux_source_directory(. SRC_LIST)

FILE(GLOB COINUTILS_SRCS
         src/*.cpp
    )


INCLUDE_DIRECTORIES(
    SYSTEM
    )


if (${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
    SET(CMAKE_FIND_LIBRARY_PREFIXES "" "lib")
    SET(CMAKE_FIND_LIBRARY_SUFFIXES ".dll" ".lib" ".a" )
ENDIF (${CMAKE_SYSTEM_NAME} STREQUAL "Windows")

ADD_LIBRARY(CoinUtils ${COINUTILS_SRCS})
INSTALL(
    TARGETS
    CoinUtils
    ARCHIVE DESTINATION 
    lib
    )
