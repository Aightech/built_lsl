cmake_minimum_required(VERSION 3.0)
###### Author: Alexis Devillard
###### Date:   27/01/2020
###### Brief: This CmakeLists.txt file enable to build generic project with the following structure:
######                                +---------+
######                                | project |
######                                +----+----+
######                                     |
######            +----------+------------++-------------+-----------+
######            |          |            |              |           |
######         +--+--+  +----+----+ +-----+------+ +-----+------+ +--+--+
######         | src |  | include | | CMakelists | | CMakefunct | | lib |
######         +--+--+  +----+----+ |    .txt    | |  ions.txt  | +--+--+
######            |          |      +------------+ +------------+    |
######      +-----+------+   |                                       |
######      |            |   +-------+            +--------------+---+
###### +----+-----+      |           |            |              |
###### | main.cpp | +----+----+ +----+----+ +-----+------+ +-----+------+
###### +----------+ | lib.cpp | | lib.hpp | | subproject | | subproject |
######              +---------+ +---------+ |     1      | |     2      |
######                                      +-----+------+ +------------+
######                                            |
######                        +-----------+-------+------+-----------+
######                        |           |              |           |
######                     +--+---+ +-----+------+ +-----+------+ +--+--+
######                     | .git | | CMakelists | | CMakeFunct | | ... |
######                     +------+ |    .txt    | |  ions.txt  | +-----+
######                              +------------+ +------------+
###### 
######

###### PROJECT NAME AND VERSION ######
set(EXEC_NAME build_lsl)
set(PROJECT_VERSION 1.3)
project(${EXEC_NAME} VERSION ${PROJECT_VERSION})

###### LIBRARY NAME ######
set(LIB_NAME ${EXEC_NAME}.${PROJECT_VERSION})

###### C++ standard ######
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)



###### Get the sources and headers ######
###### Exec main src file ######
set(MAIN_FILE "src/main.cpp")

###### Library src and header files (everything else) ######
file(GLOB LIB_SRCS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp" "src/*.c")
list(REMOVE_ITEM LIB_SRCS ${MAIN_FILE})
file(GLOB LIB_HEADERS "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h" "include/*.hpp")
#list(REMOVE_ITEM LIB_HEADERS ${EXTRA_HEADERS})



###### include helper functions ######
include(CMakeFunctions.txt)

link_directories(${CMAKE_SOURCE_DIR}/bin)



# ###### Create the library ######
# add_library(${LIB_NAME} ${LIB_SRCS})
# target_link_libraries (${LIB_NAME}
#   PUBLIC ${EXTRA_LIBS})
# target_include_directories(${LIB_NAME}
#   PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include"  ${EXTRA_INCLUDES})






 
# ###### Instalation rule ######
# install(TARGETS ${LIB_NAME} DESTINATION lib)
# install(FILES ${HEADERS} DESTINATION include)


# enable_testing()

# # define a function to simplify adding tests
# function(do_test target arg result)
#   add_test(NAME Comp${arg} COMMAND ${target} ${arg})
#   set_tests_properties(Comp${arg}
#     PROPERTIES PASS_REGULAR_EXPRESSION ${result}
#     )
# endfunction(do_test)

# # do a bunch of result based tests
# do_test(canopen 1 1000 0 "")
# do_test(canopen 2 607A 0 4 d 1000 "")
