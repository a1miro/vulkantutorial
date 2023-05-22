
add_executable(list_physical_devices list_physical_devices.cpp)
target_link_libraries(list_physical_devices glm::glm Vulkan::Vulkan glfw)
target_include_directories(list_physical_devices PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET list_physical_devices PROPERTY CXX_STANDARD 17)

