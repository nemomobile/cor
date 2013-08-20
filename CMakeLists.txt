PROJECT(cor)
cmake_minimum_required(VERSION 2.8)

find_package(PkgConfig)

IF(NOT DEFINED COR_VERSION)
MESSAGE(FATAL_ERROR "Define COR_VERSION")
ENDIF(NOT DEFINED COR_VERSION)

option(ENABLE_MULTIARCH "Enable support for multi-arch distros (lib64)" OFF)
IF(ENABLE_MULTIARCH)
  IF(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX "")
  ELSE(CMAKE_SIZEOF_VOID_P EQUAL 4)
    SET(LIB_SUFFIX 64)
  ENDIF(CMAKE_SIZEOF_VOID_P EQUAL 4)
ELSE()
  SET(LIB_SUFFIX "")
ENDIF()

set(CMAKE_CXX_FLAGS 
  "${CMAKE_CXX_FLAGS} -Wall -O2 -feliminate-unused-debug-types -std=c++0x"
  )

# fix for gcc 4.6 specific compiling/linking issues, no dwarf-4
set(CMAKE_CXX_FLAGS 
  "${CMAKE_CXX_FLAGS} -gdwarf-3"
  )


INCLUDE_DIRECTORIES(
  ${CMAKE_CURRENT_SOURCE_DIR}/include
)

set(prefix ${CMAKE_INSTALL_PREFIX})
configure_file(cor.pc.in cor.pc @ONLY)
install(FILES cor.pc DESTINATION lib${LIB_SUFFIX}/pkgconfig)

install(DIRECTORY include/cor DESTINATION include FILES_MATCHING REGEX "\\.hp?p?$")

add_custom_target(check COMMAND ${CMAKE_CTEST_COMMAND})

add_subdirectory(src)
enable_testing()
add_subdirectory(tests)
