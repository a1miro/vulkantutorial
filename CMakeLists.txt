cmake_minimum_required(VERSION 3.22)


# Adding our own cmake modules
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# include vcpkg cmake module
include(vcpkg-utils)

# initialize vcpkg
vcpkg_init()


project(VulkanTutorial LANGUAGES CXX)

#[[
# TODO: Replace hard-coded list of packages with reading from vcpkg.json
# Read packages from vcpkg.json
vcpkg_read_json(${CMAKE_CURRENT_SOURCE_DIR}/vcpkg.json VCPKG_PACKAGES)
#]]

# set the packages to install
set(VCPKG_PACKAGES
  glfw3     # GL Window system
  glm       # OpenGL Mathematics
  Stb       # header only, portable C utility library
  )

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
option(HELLO_TRIANGLE             "15 Hello triangle"             Yes)
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

