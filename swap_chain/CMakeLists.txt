
add_executable(swap_chain swap_chain.cpp)
target_link_libraries(swap_chain glm::glm Vulkan::Vulkan glfw)
target_include_directories(swap_chain PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET swap_chain PROPERTY CXX_STANDARD 17)

