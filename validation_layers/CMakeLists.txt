
add_executable(validation_layers validation_layers.cpp)
target_link_libraries(validation_layers glm::glm Vulkan::Vulkan glfw)
target_include_directories(validation_layers PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET validation_layers PROPERTY CXX_STANDARD 17)

