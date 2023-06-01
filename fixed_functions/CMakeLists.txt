
set(SHADER_SRC
  ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag
  ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert
)

add_executable(fixed_functions fixed_functions.cpp)
target_link_libraries(fixed_functions glm::glm Vulkan::Vulkan glfw)
target_include_directories(fixed_functions PUBLIC ${glfw_INCLUDE_DIRS})
set_property(TARGET fixed_functions PROPERTY CXX_STANDARD 17)

# Adding compiling shader command for every shader from SHADER_SRC
# the binary files will be put under output binary folder in shaders sub-folder
foreach(shader ${SHADER_SRC})
  cmake_path(GET shader EXTENSION LAST_ONLY shader_ext)
  string(REPLACE "." "" shader_ext ${shader_ext})
  add_custom_command(
    TARGET fixed_functions
    POST_BUILD
    COMMAND ${CMAKE_COMMAND}
    ARGS -E make_directory  "$<TARGET_FILE_DIR:fixed_functions>/shaders"
    COMMAND ${GLSLC}
    ARGS ${shader} -o "$<TARGET_FILE_DIR:fixed_functions>/shaders/${shader_ext}.spv"
    DEPENDS ${SHADER_SRC}
  )
endforeach()
