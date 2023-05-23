add_executable(logical_device logical_device.cpp)
target_link_libraries(logical_device glm::glm Vulkan::Vulkan glfw)
target_include_directories(logical_device PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET logical_device PROPERTY CXX_STANDARD 17)