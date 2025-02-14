cmake_minimum_required(VERSION 3.5)

project(beine_dienen_legs)

if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(argparse REQUIRED)
find_package(beine_cpp REQUIRED)
find_package(musen REQUIRED)
find_package(rclcpp REQUIRED)

install(DIRECTORY "include" DESTINATION ".")

add_library(${PROJECT_NAME} "src/client.cpp")

target_include_directories(${PROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)

ament_target_dependencies(${PROJECT_NAME} argparse beine_cpp musen rclcpp)

install(TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION "lib"
  LIBRARY DESTINATION "lib"
  RUNTIME DESTINATION "bin")

add_executable(client "bin/client.cpp")

target_include_directories(client PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)

target_link_libraries(client ${PROJECT_NAME})

install(TARGETS client
  DESTINATION "lib/${PROJECT_NAME}")

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_export_dependencies(argparse beine_cpp musen rclcpp)
ament_export_include_directories("include")
ament_export_libraries(${PROJECT_NAME})

ament_package()
