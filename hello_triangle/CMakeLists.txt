
set(SHADER_SRC
  ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag
  ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert
)

add_executable(hello_triangle hello_triangle.cpp)
target_link_libraries(hello_triangle glm::glm Vulkan::Vulkan glfw)
target_include_directories(hello_triangle PRIVATE ${glfw_INCLUDE_DIRS})
target_include_directories(hello_triangle PRIVATE ${Vulkan_INCLUDE_DIR})
set_property(TARGET hello_triangle PROPERTY CXX_STANDARD 17)

# Adding compiling shader command for every shader from SHADER_SRC
# the binary files will be put under output binary folder in shaders sub-folder
foreach(shader ${SHADER_SRC})
  cmake_path(GET shader EXTENSION LAST_ONLY shader_ext)
  string(REPLACE "." "" shader_ext ${shader_ext})
  add_custom_command(
    TARGET hello_triangle
    POST_BUILD
    COMMAND ${CMAKE_COMMAND}
    ARGS -E make_directory  "$<TARGET_FILE_DIR:hello_triangle>/shaders"
    COMMAND ${GLSLC}
    ARGS ${shader} -o "$<TARGET_FILE_DIR:hello_triangle>/shaders/${shader_ext}.spv"
    DEPENDS ${SHADER_SRC}
  )
endforeach()
