project(game.libretro.quicknes)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  set(LIBRETRO_DEBUG DEBUG=1)
endif()

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  set(BUILD_COMMAND mingw32-make -C libretro platform=win ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND make -C libretro ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin")
  set(BUILD_COMMAND make -C libretro platform=osx ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
# TODO
endif()

include(ExternalProject)
externalproject_add(quicknes
                    GIT_REPOSITORY https://github.com/libretro/QuickNES_Core
                    GIT_TAG bc2846d
                    PREFIX quicknes
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

set(QUICKNES_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/quicknes/src/quicknes/libretro/quicknes_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                           game.libretro.quicknes${CMAKE_SHARED_LIBRARY_SUFFIX} quicknes)

build_addon(game.libretro.quicknes QUICKNES DEPLIBS)
add_dependencies(game.libretro.quicknes quicknes)

include(CPack)
