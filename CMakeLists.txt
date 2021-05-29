cmake_minimum_required(VERSION 2.8.3)
project(yaml-cpp)
add_compile_options(-std=c++11)

find_package(catkin REQUIRED
)

# Build yaml-cpp version 0.6.2
include(ExternalProject)

ExternalProject_Add(yaml-cpp-062
  SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/yaml-cpp
  BINARY_DIR ${CMAKE_CURRENT_SOURCE_DIR}/yaml-cpp/build
  CMAKE_ARGS
    -DBUILD_SHARED_LIBS=ON
    -DYAML_CPP_BUILD_TESTS=OFF
    -Wno-dev
  BUILD_COMMAND $(MAKE) -j
  INSTALL_COMMAND cmake -E echo "Skipping install step."
)

set(headers
  ${catkin_INCLUDE_DIRS}
  ${CMAKE_CURRENT_SOURCE_DIR}/yaml-cpp/include/
)

set(libraries
  ${CMAKE_CURRENT_SOURCE_DIR}/yaml-cpp/build/libyaml-cpp.so
)

catkin_package(
  INCLUDE_DIRS ${headers}
  LIBRARIES ${libraries}
  CATKIN_DEPENDS
  # DEPENDS
)

add_custom_command(TARGET yaml-cpp-062
  POST_BUILD
  COMMAND cp ${CMAKE_CURRENT_SOURCE_DIR}/yaml-cpp/build/libyaml-cpp.so.0.6.2  
    ${CATKIN_DEVEL_PREFIX}/lib/libyaml-cpp.so.0.6.2
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/yaml-cpp/include/yaml-cpp/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
