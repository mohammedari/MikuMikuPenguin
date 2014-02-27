cmake_minimum_required(VERSION 2.8.11)
project(MMPEditor)

#include_directories(SYSTEM /home/sn0w75/Applications/Qt/5.2.1/gcc_64/include/QtWidgets)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

add_definitions(-O2 -D_FILE_OFFSET_BITS=64 -fPIC)
add_definitions(${Qt5Widgets_DEFINITIONS})

# Find the QtWidgets library
find_package(Qt5Widgets REQUIRED)
find_package(Qt5OpenGL REQUIRED)
find_package(Qt5Gui REQUIRED)
find_package(Qt5Quick REQUIRED)
find_package(OpenGL REQUIRED)
find_package(PkgConfig)

include_directories (${CMAKE_SOURCE_DIR}/src ${QT_QTDECLARATIVE_INCLUDE_DIR})

set(libmmp_dep ${libmmp_dep}

${OPENGL_glu_LIBRARY}

${OPENGL_gl_LIBRARY})


# Tell CMake to create the helloworld executable
add_executable(MMPEditor main.cpp openglscene.cpp fboinsgrenderer.cpp logorenderer.cpp)

#Copy QML to build directory (also installation dir???)
add_custom_command(TARGET MMPEditor PRE_BUILD
COMMAND ${CMAKE_COMMAND} -E copy_directory
${CMAKE_CURRENT_SOURCE_DIR}/qml
${CMAKE_BINARY_DIR}/qml
)

# Use the Widgets module from Qt 5.
target_link_libraries(MMPEditor mmp SOIL ${Qt5Widgets_LIBRARIES} ${libmmp_dep})

qt5_use_modules(MMPEditor Core Quick Gui Widgets OpenGL)
