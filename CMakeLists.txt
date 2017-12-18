project(imagedecoder.mpo)

cmake_minimum_required(VERSION 2.6)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(Kodi REQUIRED)
if(NOT CMAKE_SYSTEM_NAME MATCHES Windows)
  find_package(JPEG REQUIRED)
else()
  find_package(libjpeg-turbo REQUIRED NO_MODULE)
endif()

include_directories(${KODI_INCLUDE_DIR}/..
                    ${JPEG_INCLUDE_DIR})

add_subdirectory(lib/libmpo EXCLUDE_FROM_ALL)

set(MPO_SOURCES src/MPOPicture.cpp)

set(DEPLIBS mpo_static)
if(NOT CMAKE_SYSTEM_NAME MATCHES Windows)
  list(APPEND DEPLIBS ${JPEG_LIBRARIES})
else()
  list(APPEND DEPLIBS libjpeg-turbo::jpeg-static)
endif()

build_addon(imagedecoder.mpo MPO DEPLIBS)

include(CPack)
