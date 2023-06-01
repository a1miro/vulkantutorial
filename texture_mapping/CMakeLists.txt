
set(SHADER_SRC
  ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag
  ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert
)

set(TEXTURE_FILE ${CMAKE_CURRENT_SOURCE_DIR}/textures/texture.jpg)

add_executable(texture_mapping texture_mapping.cpp)
target_link_libraries(texture_mapping glm::glm Vulkan::Vulkan glfw)
target_include_directories(texture_mapping PUBLIC ${glfw_INCLUDE_DIRS})
target_include_directories(texture_mapping PRIVATE ${Stb_INCLUDE_DIR})
set_property(TARGET texture_mapping PROPERTY CXX_STANDARD 17)

# Adding compiling shader command for every shader from SHADER_SRC
# the binary files will be put under output binary folder in shaders sub-folder
foreach(shader ${SHADER_SRC})
  cmake_path(GET shader EXTENSION LAST_ONLY shader_ext)
  string(REPLACE "." "" shader_ext ${shader_ext})
  add_custom_command(
    TARGET texture_mapping
    POST_BUILD
    COMMAND ${CMAKE_COMMAND}
    ARGS -E make_directory  "$<TARGET_FILE_DIR:texture_mapping>/shaders"
    COMMAND ${GLSLC}
    ARGS ${shader} -o "$<TARGET_FILE_DIR:texture_mapping>/shaders/${shader_ext}.spv"
    DEPENDS ${SHADER_SRC}
  )
endforeach()

add_custom_command(
    TARGET texture_mapping
    POST_BUILD
    COMMAND ${CMAKE_COMMAND}
    ARGS -E make_directory  "$<TARGET_FILE_DIR:texture_mapping>/textures"
    COMMAND ${CMAKE_COMMAND}
    ARGS -E copy  ${TEXTURE_FILE} "$<TARGET_FILE_DIR:texture_mapping>/textures"
    DEPENDS ${TEXTURE_FILE}
  )
