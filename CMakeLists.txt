# Based on https://github.com/gilborty/imgui/blob/docking/examples/example_glfw_opengl3/Makefile
# And: https://github.com/Kettenhoax/imgui_vendor/blob/master/CMakeLists.txt
cmake_minimum_required(VERSION 3.5)
project(implot)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

find_package(ament_cmake REQUIRED)

# Prefer OpenGL neutral dispatch
set(OpenGL_GL_PREFERENCE GLVND)

find_package(glfw3 3.3 REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)
find_package(imgui REQUIRED)

add_library(implot SHARED
    implot/implot.cpp
    implot/implot_items.cpp)

target_include_directories(implot PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/implot>
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/implot/backends>
    $<INSTALL_INTERFACE:include>)

target_link_libraries(implot PUBLIC glfw OpenGL::GL GLEW::GLEW imgui)
ament_export_targets(export_implot HAS_LIBRARY_TARGET)

add_executable(implot_example src/implot_demo.cpp)
target_link_libraries(implot_example PRIVATE implot)

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

install(TARGETS implot_example
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

ament_package()