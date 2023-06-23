# Based on https://github.com/gilborty/imgui/blob/docking/examples/example_glfw_opengl3/Makefile
# And: https://github.com/Kettenhoax/imgui_vendor/blob/master/CMakeLists.txt
cmake_minimum_required(VERSION 3.5)
project(implot)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# Prefer OpenGL neutral dispatch
set(OpenGL_GL_PREFERENCE GLVND)

find_package(ament_cmake REQUIRED)

# ==============================================
# Dependencies
set(THIS_PACKAGE_INCLUDE_DEPENDS
    imgui
)

find_package(glfw3 3.3 REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)
find_package(imgui REQUIRED)

add_library(implot SHARED
    implot/implot.cpp
    implot/implot_demo.cpp
    implot/implot_items.cpp)

target_include_directories(implot PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/implot>
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/implot/backends>
    $<INSTALL_INTERFACE:include>)

ament_target_dependencies(
    implot
  ${THIS_PACKAGE_INCLUDE_DEPENDS}
)
ament_export_targets(export_implot HAS_LIBRARY_TARGET)

ament_export_include_directories(include)
ament_export_libraries(implot)
ament_export_dependencies(glfw3 OpenGL GLEW imgui)

install(FILES
    implot/implot.h
    implot/implot_internal.h
  DESTINATION include)

install(TARGETS implot EXPORT export_implot
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

ament_package()
