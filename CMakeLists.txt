cmake_minimum_required(VERSION 3.0)
project(mtbGUI)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # not necessary, but encouraged
# Download automatically, you can also just copy the conan.cmake file

if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
   message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
   file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/v0.8/conan.cmake"
                 "${CMAKE_BINARY_DIR}/conan.cmake")
endif()

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup(KEEP_RPATHS)

include(${CMAKE_BINARY_DIR}/conan.cmake)
IF(APPLE)
   INCLUDE_DIRECTORIES ( /System/Library/Frameworks )
   FIND_LIBRARY(COCOA_LIBRARY Cocoa)
   FIND_LIBRARY(GLUT_LIBRARY GLUT )
   FIND_LIBRARY(OpenGL_LIBRARY OpenGL )
   MARK_AS_ADVANCED (COCOA_LIBRARY
                     GLUT_LIBRARY
                     OpenGL_LIBRARY)
   SET(EXTRA_LIBS ${COCOA_LIBRARY} ${GLUT_LIBRARY} ${OpenGL_LIBRARY})
ENDIF (APPLE)

add_executable(mtbGUI src/main.cpp)
target_link_libraries(mtbGUI ${CONAN_LIBS})
# target_link_libraries(main ${CONAN_LIBS})
target_link_libraries(mtbGUI ${EXTRA_LIBS})
set(CMAKE_BUILD_TYPE Release)
