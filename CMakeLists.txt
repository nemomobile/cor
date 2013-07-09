PROJECT(cor)
cmake_minimum_required(VERSION 2.8)

find_package(PkgConfig)

SET(LIB_SUFFIX "")

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

INCLUDE_DIRECTORIES(
  ${CMAKE_CURRENT_SOURCE_DIR}/tut/include
)

set(prefix ${CMAKE_INSTALL_PREFIX})
configure_file(cor.pc.in cor.pc @ONLY)
install(FILES cor.pc DESTINATION lib${LIB_SUFFIX}/pkgconfig)

install(DIRECTORY include/cor DESTINATION include FILES_MATCHING REGEX "\\.hp?p?$")

add_subdirectory(src)
enable_testing()
add_subdirectory(tests)
