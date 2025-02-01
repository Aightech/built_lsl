cmake_minimum_required(VERSION 3.3...3.5)
###### LIBRARY NAME AND VERSION ######
include(CMakeConfig.cmake)

if(NOT DEFINED DIR_DEPTH)
set(DIR_DEPTH 0)
else ()
MATH(EXPR DIR_DEPTH "${DIR_DEPTH}+1")
endif()
string(REPEAT "\t" ${DIR_DEPTH} PREFIX_MSG)
MATH(EXPR color_code "${DIR_DEPTH}+33")
MATH(EXPR color_code2 "${DIR_DEPTH}+34")

#try to find the LSL library
find_package(LSL HINTS ${LSL_DIR})

if (LSL_FOUND)
  #the parent calling this fproject uses 
  # get_directory_property(libname
  #       DIRECTORY "tool_lib/${subdir}"
  #       DEFINITION LIB_NAME)
  # set the name of the library to LSL::lsl
  set(LIB_NAME LSL::lsl)
  set_target_properties(${LIB_NAME} PROPERTIES IMPORTED_GLOBAL TRUE)
  message("${PREFIX_MSG}-- ${ColourBold}${Esc}[${color_code}m${LIB_NAME}${ColourReset}: LSL library found")
else ()
message("${PREFIX_MSG}-- ${ColourBold}${Esc}[${color_code}m${LIB_NAME}${ColourReset}: LSL library not found. (Install it for an easier use: https://github.com/sccn/liblsl)")
message("${PREFIX_MSG}-- ${ColourBold}${Esc}[${color_code}m${LIB_NAME}${ColourReset}: Trying to link with built_LSL library...")


  if (TARGET ${LIB_NAME})
    message("${PREFIX_MSG}-- ${ColourBold}${Esc}[${color_code}m${LIB_NAME}${ColourReset} is already built")
  else ()

    # ###### C++ standard ######
    set(CMAKE_CXX_STANDARD 11)
    set(CMAKE_CXX_STANDARD_REQUIRED True)
    
    add_library(${LIB_NAME} SHARED IMPORTED GLOBAL)
    
    if (UNIX)
    set_target_properties( ${LIB_NAME} PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/bin/liblsl64.so" )
    endif (UNIX)
    
    if (WIN32)
    set_target_properties( ${LIB_NAME} PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/bin/liblsl64.dll" )
    set_target_properties( ${LIB_NAME} PROPERTIES IMPORTED_IMPLIB "${CMAKE_CURRENT_SOURCE_DIR}/bin/liblsl64.dll" )
    endif (WIN32)
    
    set_target_properties( ${LIB_NAME} PROPERTIES INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/include" )

  endif ()
endif()