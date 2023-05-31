
add_executable(graphics_pipeline graphics_pipeline.cpp)
target_link_libraries(graphics_pipeline glm::glm Vulkan::Vulkan glfw)
target_include_directories(graphics_pipeline PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET graphics_pipeline PROPERTY CXX_STANDARD 17)

