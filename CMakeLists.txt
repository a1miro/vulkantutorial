cmake_minimum_required(VERSION 3.22)

#if(DEFINED ENV{VCPKG_ROOT})
#  message(STATUS "Found VCPKG_ROOT=$ENV{VCPKG_ROOT}")
#  set(vcpkg_root "$ENV{VCPKG_ROOT}")
#  set(CMAKE_TOOLCHAIN_FILE ${vcpkg_root}/scripts/buildsystems/vcpkg.cmake CACHE FILEPATH "" FORCE)
#else()
#  message(
#    [[
#    VCPKG_ROOT was not found,
#    I highly recommend to install VCPKG and set VCPKG_ROOT, it can be done for Winows, MacOS and Linux.
#    Please follow isntructions from this page https://vcpkg.io/en/getting-started.html,
#    overwise you have to install manually the following packages:
#
#    glm         OpenGL Mathematics
#    glfw3       Free, open-source GL Window system
#    stb         header only, portable C utility library
#    ]])
#endif()

#[[
if(NOT CMAKE_TOOLCHAIN_FILE)
  set(CMAKE_TOOLCHAIN_FILE ${CMAKE_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake CACHE FILEPATH
  "VCPKG CMake toolchain file" FORCE)
endif()
message(STATUS "CMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE}")

# Get VCPKG_RROT from CMAKE_TOOLCHAIN_FILE
get_filename_component(VCPKG_ROOT ${CMAKE_TOOLCHAIN_FILE} DIRECTORY)
string(REPLACE "/scripts/buildsystems" "" VCPKG_ROOT ${VCPKG_ROOT})
message(STATUS "VCPKG_ROOT=${VCPKG_ROOT}")

# check directory existence
if(NOT EXISTS ${VCPKG_ROOT}/triplets)
  message(STATUS "vcpkg is not initialised, let's run bootstrap-vcpkg")

  if(CMAKE_HOST_SYTEM_NAME STREQUAL "Windows")
    #set(VCPKG_TARGET_TRIPLET "x64-windows" CACHE STRING "VCPKG target triplet" FORCE)
    execute_process(COMMAND ${CMAKE_SOURCE_DIR}/vcpkg/bootstrap-vcpkg.bat)
  else()
    #set(VCPKG_TARGET_TRIPLET "x64-linux" CACHE STRING "VCPKG target triplet" FORCE)
    execute_process(COMMAND ${CMAKE_SOURCE_DIR}/vcpkg/bootstrap-vcpkg.sh)
  endif()

endif()


find_package(glfw3)
if(glfw3_FOUND)
  message(STATUS "glfw3 is found")
else()
  message("Installing glfw3 package using vcpkg")
  execute_process(COMMAND vcpkg install glfw3)
  find_package(glfw3 REQUIRED)
endif()

find_package(glm)
if(glm_FOUND)
  message(STATUS "glm is found")
else()
  message("Installing glm package using vcpkg")
  execute_process(COMMAND vcpkg install glm)
  find_package(glm REQUIRED)
endif()

#find_package(glm REQUIRED)
#find_package(Stb REQUIRED)
#]]

# Adding our own cmake modules
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# include vcpkg cmake module
include(vcpkg-utils)

# initialize vcpkg
#vcpkg_init()

# Adding vcpkg cmake modules
#list(APPEND CMAKE_MODULE_PATH ${VCPKG_ROOT}/scripts/cmake)
#list(APPEND CMAKE_MODULE_PATH ${VCPKG_ROOT}/packages/stb_x64-osx/share/stb)

message(STATUS "VCPKG_ROOT=${VCPKG_ROOT}")
message(STATUS "CMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE}")
#message(FATAL_ERROR "CMAKE_MODULE_PATH=${CMAKE_MODULE_PATH}")


project(VulkanTutorial LANGUAGES CXX)


# set the packages to install
set(VCPKG_PACKAGES
  glfw3     # GL Window system
  glm       # OpenGL Mathematics
  Stb       # header only, portable C utility library
  )

# install the packages
# vcpkg_install(${VCPKG_PACKAGES})

#[[
# TODO: Replace hard-coded list of packages with reading from vcpkg.json
# Read packages from vcpkg.json
vcpkg_read_json(${CMAKE_CURRENT_SOURCE_DIR}/vcpkg.json VCPKG_PACKAGES)
message(FATAL_ERROR "VCPKG_PACKAGES=${VCPKG_PACKAGES}")
#]]

foreach(package ${VCPKG_PACKAGES})
  find_package(${package} REQUIRED)
endforeach()

if(DEFINED ENV{VULKAN_SDK})
  set(VULKAN_SDK "$ENV{VULKAN_SDK}")
  find_package(Vulkan REQUIRED)
else()
  message(STATUS "VULKAN_SDK variable was not found, trying to find it in the system")
  find_package(Vulkan REQUIRED)
  if(Vulkan_FOUND)
    message(STATUS "Vulkan SDK is found")
  else()
    message(FATAL_ERROR "Vulkan SDK is not found")
  endif()
endif()

add_compile_definitions(HOST_SYSTEM_NAME="${CMAKE_HOST_SYSTEM_NAME}")

find_program(GLSLC NAMES glslc glslc.exe)
if(GLSLC)
  message(STATUS "Found GLSLC compiler ${GLSLC}")
else()
  message(FATAL_ERROR "GSLC compiler is not found")
endif()


option(BASE_CODE                  "0  Base_code"                  No)
option(INSTANCE_CREATION          "1  Instance Creation"          No)
option(VALIDATION_LAYERS          "2  Validation Layers"          No)
option(PHYSICAL_DEVICE_SELECTION  "3  Physical device selection"  No)
option(LOGICAL_DEVICE             "4  Logical Device"             No)
option(WINDOW_SURFACE             "5  Window Surface"             No)
option(SWAP_CHAIN                 "6  Swap chain"                 No)
option(IMAGE_VIEWS                "7  Image Views"                No)
option(GRAPHICS_PIPELINE          "8  Graphics Pipeline"          No)
option(SHADER_BASE                "9  Shader base"                No)
option(FIXED_FUNCTIONS            "10 Fixed functions"            No)
option(HELLO_TRIANGLE             "15 Hello triangle"             No)
option(TEXTURE_MAPPING            "26 Texture mapping"            Yes)
option(LIST_PHYSICAL_DEVICES      "Llist physical devices"        No)

if(BASE_CODE)
  add_subdirectory(base_code)
endif()

if(INSTANCE_CREATION)
  add_subdirectory(instance_creation)
endif()

if(VALIDATION_LAYERS)
  add_subdirectory(validation_layers)
endif()

if(PHYSICAL_DEVICE_SELECTION)
  add_subdirectory(physical_device_selection)
endif()

if(LOGICAL_DEVICE)
  add_subdirectory(logical_device)
endif()

if(WINDOW_SURFACE)
  add_subdirectory(window_surface)
endif()

if(SWAP_CHAIN)
  add_subdirectory(swap_chain)
endif()

if(IMAGE_VIEWS)
  add_subdirectory(swap_chain)
endif()

if(GRAPHICS_PIPELINE)
  add_subdirectory(swap_chain)
endif()

if(SHADER_BASE)
  add_subdirectory(shader_base)
endif()

if(FIXED_FUNCTIONS)
  add_subdirectory(fixed_functions)
endif()

if(HELLO_TRIANGLE)
  add_subdirectory(hello_triangle)
endif()

if(TEXTURE_MAPPING)
  add_subdirectory(texture_mapping)
endif()


if(LIST_PHYSICAL_DEVICES)
  add_subdirectory(list_physical_devices)
endif()

