cmake_minimum_required(VERSION 3.0)
set(PROJECT_NAME "example")

project(${PROJECT_NAME})

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # not necessary, but encouraged

if (APPLE)
  # Use automatic reference counting for Objective-C portions
  add_compile_options(-fobjc-arc)
endif()

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

add_executable("${PROJECT_NAME}1" "src/${PROJECT_NAME}1.cpp")
target_link_libraries(${PROJECT_NAME}1 ${CONAN_LIBS})
target_link_libraries(${PROJECT_NAME}1 ${EXTRA_LIBS})

# RESOURCES
# # Glob up resource files
# file(GLOB resources "${CMAKE_CURRENT_SOURCE_DIR}/resources/*.ttf")

# # Concatenate resource files into a comma separated string
# string (REGEX REPLACE "([^\\]|^);" "\\1," resources_string "${resources}")
# string (REGEX REPLACE "[\\](.)" "\\1" resources_string "${resources_string}")

# # Create command line for running bin2c cmake script
# set(bin2c_cmdline
#   -DOUTPUT_C=nanogui_resources.cpp
#   -DOUTPUT_H=nanogui_resources.h
#   "-DINPUT_FILES=${resources_string}"
#   -P "${CMAKE_CURRENT_SOURCE_DIR}/resources/bin2c.cmake")

# # Run bin2c on resource files
# add_custom_command(
#   OUTPUT nanogui_resources.cpp nanogui_resources.h
#   COMMAND ${CMAKE_COMMAND} ARGS ${bin2c_cmdline}
#   DEPENDS ${resources}
#   COMMENT "Running bin2c"
#   PRE_BUILD VERBATIM)

# # Needed to generated files
# include_directories(${CMAKE_CURRENT_BINARY_DIR})

# Copy icons for example application
file(COPY resources/icons DESTINATION "${CMAKE_CURRENT_BINARY_DIR}/bin")


add_executable("${PROJECT_NAME}2" "src/${PROJECT_NAME}2.cpp")
target_link_libraries(${PROJECT_NAME}2 ${CONAN_LIBS})
target_link_libraries(${PROJECT_NAME}2 ${EXTRA_LIBS})

add_executable("${PROJECT_NAME}3" "src/${PROJECT_NAME}3.cpp")
target_link_libraries(${PROJECT_NAME}3 ${CONAN_LIBS})
target_link_libraries(${PROJECT_NAME}3 ${EXTRA_LIBS})

add_executable("${PROJECT_NAME}4" "src/${PROJECT_NAME}4.cpp")
target_link_libraries(${PROJECT_NAME}4 ${CONAN_LIBS})
target_link_libraries(${PROJECT_NAME}4 ${EXTRA_LIBS})
# target_link_libraries(main ${CONAN_LIBS})
set(CMAKE_BUILD_TYPE Release)
