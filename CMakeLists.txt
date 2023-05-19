cmake_minimum_required(VERSION 3.15)
project(VulkanTutorial LANGUAGES CXX)


if(DEFINED ENV{VULKAN_SDK})
  message(STATUS "VULKAN_SDK environment variable is found here: $ENV{VULKAN_SDK}")
else()
  message(STATUS "VULKAN_SDK variable is not found, checking if the package is installed")
endif()

if("${CMAKE_HOST_SYSTEM_NAME}" STREQUAL "Linux")
  add_compile_definitions(HOST_SYSTEM_NAME="Linux")
endif()

if("${CMAKE_HOST_SYSTEM_NAME}" STREQUAL "Windows")
  add_compile_definitions(HOST_SYSTEM_NAME="Windows")
endif()

if("${CMAKE_HOST_SYSTEM_NAME}" STREQUAL "Darwin")
  add_compile_definitions(HOST_SYSTEM_NAME="Darwin")
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

