#----------------------------------------------------------------------
#
#  Single example osmium-contrib CMakeLists.txt
#
#----------------------------------------------------------------------
project(osmium-amenity-list)

cmake_minimum_required(VERSION 2.8.5)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")

find_package(Boost REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})

find_package(Osmium REQUIRED COMPONENTS io sparsehash)
include_directories(${OSMIUM_INCLUDE_DIRS})

include(common)

enable_testing()

#----------------------------------------------------------------------

set(PROG amenity_list)
file(GLOB SOURCES *.cpp *.hpp)
add_executable(${PROG} ${SOURCES})
target_link_libraries(${PROG} ${Boost_LIBRARIES} ${OSMIUM_LIBRARIES})

add_test(NAME amenity_list_node
         WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
         COMMAND amenity_list ${CMAKE_CURRENT_SOURCE_DIR}/test/node.osm
)
set_tests_properties(amenity_list_node PROPERTIES
                     PASS_REGULAR_EXPRESSION "  8\\.8721, 53\\.0966 post_office"
)


#----------------------------------------------------------------------
