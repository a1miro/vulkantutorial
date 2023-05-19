
add_executable(instance_creation instance_creation.cpp)
target_link_libraries(instance_creation glm::glm Vulkan::Vulkan glfw)
target_include_directories(instance_creation PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET instance_creation PROPERTY CXX_STANDARD 17)
