
add_executable(window_surface window_surface.cpp)
target_link_libraries(window_surface glm::glm Vulkan::Vulkan glfw)
target_include_directories(window_surface PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET window_surface PROPERTY CXX_STANDARD 17)

