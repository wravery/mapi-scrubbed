cmake_minimum_required(VERSION 3.17.1)
project(mapi-scrubbed)

set(CMAKE_CXX_STANDARD 20)

# Fetch the https://github.com/microsoft/MAPIStubLibrary repository.
include(FetchContent)
FetchContent_Declare(MAPIStubLibrary
  GIT_REPOSITORY "https://github.com/microsoft/MAPIStubLibrary"
  GIT_TAG "a888542d2e2630bf0da3b3855d198773b61c700e"
  GIT_PROGRESS TRUE)
FetchContent_GetProperties(MAPIStubLibrary)

if(NOT mapistublibrary_POPULATED)
  FetchContent_Populate(MAPIStubLibrary)
  file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/scrubber DESTINATION ${mapistublibrary_SOURCE_DIR})
else()
  file(GLOB SCRUBBER_SOURCES LIST_DIRECTORIES false RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}/scrubber/" "${CMAKE_CURRENT_SOURCE_DIR}/scrubber/*")
  foreach(SOURCE ${SCRUBBER_SOURCES})
    file(COPY_FILE
        ${CMAKE_CURRENT_SOURCE_DIR}/scrubber/${SOURCE}
        ${mapistublibrary_SOURCE_DIR}/scrubber/${SOURCE}
      ONLY_IF_DIFFERENT)
  endforeach()
endif()

add_subdirectory(${mapistublibrary_SOURCE_DIR}/scrubber ${mapistublibrary_BINARY_DIR})

add_library(testbuild STATIC testbuild.cpp)
target_include_directories(testbuild PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
add_dependencies(testbuild copyheaders)