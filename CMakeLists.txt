cmake_minimum_required(VERSION 3.15)
project(VulkanTutorial LANGUAGES CXX)


if(DEFINED ENV{VULKAN_SDK})
  message(STATUS "VULKAN_SDK environment variable is found")
else()
  message(FATAL_ERROR "Please set VULKAN_SDK environment variable")
endif()

find_program(GLSLC NAMES glslc glslc.exe)
if (GLSLC)
  message(STATUS "Found GLSLC compiler ${GLSLC}")
else ()
  messag(FATAL_ERROR "GSLC compiler is not found")
endif()


find_package(glfw3 REQUIRED)
find_package(glm REQUIRED)
find_package(Vulkan REQUIRED)



option(HELLO_TRIANGLE "Hello triangle" Yes)



if(HELLO_TRIANGLE)
  add_subdirectory(hello_triangle)
endif()

