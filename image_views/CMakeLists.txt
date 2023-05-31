
add_executable(image_views image_views.cpp)
target_link_libraries(image_views glm::glm Vulkan::Vulkan glfw)
target_include_directories(image_views PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET image_views PROPERTY CXX_STANDARD 17)

