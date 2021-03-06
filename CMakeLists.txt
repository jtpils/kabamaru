cmake_minimum_required(VERSION 2.8)
project(kabamaru)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

# compile with C++11 support
add_definitions("-std=c++11")

find_package(CUDA)
find_package(SIFTGPU REQUIRED)
find_package(OpenCV 3 REQUIRED)
find_package(PCL 1.8 REQUIRED)
find_package(namaris REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

include_directories(include)
file(GLOB src_files src/*.cpp)

set(LIB_NAME "kabamaru")

add_library(${LIB_NAME} SHARED ${src_files})
target_link_libraries(${LIB_NAME} ${SIFTGPU_LIBRARIES} ${OpenCV_LIBRARIES} ${PCL_LIBRARIES} ${namaris_LIBRARIES} ${CUDA_LIBRARIES})

install(TARGETS ${LIB_NAME} LIBRARY DESTINATION lib)
install(DIRECTORY include/ DESTINATION include)

# uninstall target
configure_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
	"${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
	IMMEDIATE @ONLY)

add_custom_target(uninstall COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
