
add_executable(base_code base_code.cpp)
target_link_libraries(base_code glm::glm Vulkan::Vulkan glfw)
target_include_directories(base_code PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET base_code PROPERTY CXX_STANDARD 17)
