cmake_minimum_required(VERSION 3.15)
project(VulkanTutorial LANGUAGES CXX)


if(DEFINED ENV{ROGUEDDK})
  message(STATUS "ROGUE_DDK is found here $ENV{VULKAN_SDK}")
  if(NOT DEFINED ENV{DDK_BRANCH})
    message(FATAL_ERROR "Please export ROGUEDDK_BRANCH")
  endif()

  if(NOT DEFINED ENV{LLVM_BUILD_DIR})
    message(FATAL_ERROR "Please export LLVM_BUILD_DIR")
  endif()
    
else()
  message(STATUS "ROGUE_DDK is not found, checking VULKAN_SDK")
  if(DEFINED ENV{VULKAN_SDK})
    message(STATUS "VULKAN_SDK environment variable is found here: $ENV{VULKAN_SDK}")
  else()
    message(STATUS "VULKAN_SDK is not found, checking if ROGUE_DDK installed")
  endif()
endif()


find_program(GLSLC NAMES glslc glslc.exe)
if(GLSLC)
  message(STATUS "Found GLSLC compiler ${GLSLC}")
else()
  message(FATAL_ERROR "GSLC compiler is not found")
endif()

find_package(Vulkan REQUIRED)
find_package(glfw3 REQUIRED)
find_package(glm REQUIRED)


option(BASE_CODE                "0  Base_code"            Yes)
option(INSTANCE_CREATION        "1 Instance Creation"     Yes)
option(HELLO_TRIANGLE           "15 Hello triangle"       Yes)

if(BASE_CODE)
  add_subdirectory(base_code)
endif()

if(INSTANCE_CREATION)
  add_subdirectory(instance_creation)
endif()

if(HELLO_TRIANGLE)
  add_subdirectory(hello_triangle)
endif()

