
add_executable(physical_device_selection physical_device_selection.cpp)
target_link_libraries(physical_device_selection glm::glm Vulkan::Vulkan glfw)
target_include_directories(physical_device_selection PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET physical_device_selection PROPERTY CXX_STANDARD 17)

