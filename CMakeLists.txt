cmake_minimum_required(VERSION 3.22)
project(gz_led_plugin)

find_package(ament_cmake REQUIRED)
find_package(gz-sim REQUIRED)
find_package(gz-math REQUIRED)
find_package(gz-msgs REQUIRED)
find_package(gz-plugin REQUIRED)
find_package(gz-transport REQUIRED)

# Add the plugin shared library
add_library(${PROJECT_NAME} SHARED src/LedPlugin.cpp)
target_include_directories(${PROJECT_NAME}
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
target_link_libraries(${PROJECT_NAME} PUBLIC gz-sim::gz-sim)

# Install
install(
  TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)

install(
  DIRECTORY include/
  DESTINATION include
)

install(
  DIRECTORY examples
  DESTINATION share/${PROJECT_NAME}
)

# Environment hooks to set GZ_SIM_PLUGIN_PATH when workspace is source
ament_environment_hooks("${CMAKE_CURRENT_SOURCE_DIR}/env_hooks/gz_led_plugin.sh.in")

ament_package()
