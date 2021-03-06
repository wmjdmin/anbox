set(GENERATED_SOURCES
    gles2_dec.cpp
    gles2_opcodes.h
    gles2_server_context.cpp)

add_custom_command(
    OUTPUT ${GENERATED_SOURCES}
    POST_BUILD
    COMMAND ${CMAKE_BINARY_DIR}/external/android-emugl/host/tools/emugen/emugen
            -D ${CMAKE_CURRENT_BINARY_DIR} gles2
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    DEPENDS emugen)

set(SOURCES
    GLESv2Decoder.cpp)

if ("${cmake_build_type_lower}" STREQUAL "trace")
    set(OPENGL_DEBUG "-DOPENGL_DEBUG_PRINTOUT -DCHECK_GL_ERROR")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OPENGL_DEBUG}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OPENGL_DEBUG}")
endif()

add_library(GLESv2_dec ${SOURCES} ${GENERATED_SOURCES})
target_link_libraries(GLESv2_dec OpenglCodecCommon)
