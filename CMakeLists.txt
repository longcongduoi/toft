CMAKE_MINIMUM_REQUIRED(VERSION 3.2)
PROJECT(toft)

SET(CMAKE_CXX_STANDARD 11)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(CheckIncludeFileCXX)
check_include_file_cxx(type_traits _STD_HAS_TYPE_TRAITS)
check_include_file_cxx(tr1/type_traits _TR1_HAS_TYPE_TRAITS)

configure_file (
    "config.h.in"
    "config.h"
    )

include(toft)
include(generic)
cc_library(toft SRCS ${toft_src} DEPS toft_other)
cc_library(toft_other SRCS ${toft_other_src})

if(APPLE)
  target_link_libraries(toft -lz -lcrypto -liconv -llzma -lgcrypt -lidn -lresolv)
else()
  # looks like keytuils is involed when building krb on Linux platform
  target_link_libraries(toft -lz -lcrypto -liconv -llzma -lgcrypt -lidn -lresolv -lkeyutils)
endif()
