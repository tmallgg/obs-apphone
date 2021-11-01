cmake_minimum_required(VERSION 3.16)
project(obs-multi-rtmp VERSION 0.2.8.0)

add_library(obs-multi-rtmp MODULE)
add_library(OBS::obs-multi-rtmp ALIAS obs-multi-rtmp)

if(BUILD_QT EQUAL 5)
  find_package(Qt5 COMPONENTS Widgets REQUIRED)
  
  target_link_libraries(obs-multi-rtmp PRIVATE Qt5::Widgets Qt5::GuiPrivate)

  target_include_directories(
  obs-multi-rtmp PRIVATE "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
                                  "$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>")
							   
elseif(BUILD_QT EQUAL 6)
  message(FATAL_ERROR "OBS: Building with Qt6 not supported yet.")
endif()

target_sources(obs-multi-rtmp PRIVATE "src/obs-multi-rtmp.cpp"
    "src/obs-multi-rtmp.h"
    "src/edit-widget.cpp"
    "src/edit-widget.h"
    "src/push-widget.cpp"
    "src/push-widget.h"
    "src/pch.h")

target_link_libraries(obs-multi-rtmp PRIVATE OBS::libobs OBS::frontend-api)

target_compile_features(obs-multi-rtmp PRIVATE cxx_std_17)

set_target_properties(obs-multi-rtmp PROPERTIES FOLDER "plugins/obs-multi-rtmp" PREFIX "")

setup_plugin_target(obs-multi-rtmp)
