# --------------------------
# Setup sources
# --------------------------
message(CHECK_START "Generating ${IMGUI_LIBRARY_NAME}")

if("${REX_BUILD_TARGET}" STREQUAL "${WINDOWS}")
    if("${REX_API_TARGET}" STREQUAL "${DIRECTX}")
        file(GLOB TARGET_INC    "imconfig.h" "imgui.h" "imgui_internal.h" "rectpack.h" "textedit.h" "truetype.h" "imgui_impl_win32.h" "imgui_impl_dx11.h")
        file(GLOB TARGET_SRC    "imgui.cpp" "imgui_draw.cpp" "imgui_widgets.cpp" "imgui_demo.cpp" "imgui_impl_win32.cpp" "imgui_impl_dx11.cpp")
    elseif("${REX_API_TARGET}" STREQUAL "${OPENGL}")
        file(GLOB TARGET_INC    "imconfig.h" "imgui.h" "imgui_internal.h" "rectpack.h" "textedit.h" "truetype.h" "imgui_impl_win32.h" "imgui_impl_opengl3.h")
        file(GLOB TARGET_SRC    "imgui.cpp" "imgui_draw.cpp" "imgui_widgets.cpp" "imgui_demo.cpp" "imgui_impl_win32.cpp" "imgui_impl_opengl3.cpp")
    endif()
else()
    file(GLOB TARGET_INC    "imconfig.h" "imgui.h" "imgui_internal.h" "rectpack.h" "textedit.h" "truetype.h" "imgui_impl_opengl3.h")
    file(GLOB TARGET_SRC    "imgui.cpp" "imgui_draw.cpp" "imgui_widgets.cpp" "imgui_demo.cpp" "imgui_impl_opengl3.cpp")
endif()

# --------------------------
# Setup library
# --------------------------
# Compiler flag /MD
SETUP_MULTITHREADING_MD()

ADD_LIBRARY(${IMGUI_LIBRARY_NAME} STATIC
            ${TARGET_INC}
            ${TARGET_SRC}) 
            
add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GLEW)

target_compile_options(${IMGUI_LIBRARY_NAME} PRIVATE /W3 /WX /Zi)

# --------------------------
# Target properties
# --------------------------
set_target_properties( ${IMGUI_LIBRARY_NAME} PROPERTIES     VS_DEBUGGER_WORKING_DIRECTORY    "${OUTPUT_BINDIR}"             # Working directory
                                                            FOLDER                            "0. External Libraries"        # Project filter location
                                                            DEFINE_SYMBOL                     "" )                           # Library symbol

# --------------------------
# Target include directories.
# --------------------------
target_include_directories(${IMGUI_LIBRARY_NAME} PUBLIC
                                                    $<BUILD_INTERFACE:${IMGUI_INCLUDE_DIRECTORY}> 
                                                    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${IMGUI_LIBRARY_NAME}>)

# --------------------------
# Rendering API
# --------------------------
SETUP_RENDERINGAPI(${IMGUI_LIBRARY_NAME})

# --------------------------
# Install.
# --------------------------
SET(EXPORT_TARGETS ${IMGUI_LIBRARY_NAME})
SET(EXPORT_TARGETS_NAME ${IMGUI_LIBRARY_NAME}_targets_name)

INSTAll_TARGETS(${EXPORT_TARGETS} ${EXPORT_TARGETS_NAME})

install(
    FILES ${TARGET_INC}
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${IMGUI_LIBRARY_NAME}
    )

message(CHECK_PASS "Done")
