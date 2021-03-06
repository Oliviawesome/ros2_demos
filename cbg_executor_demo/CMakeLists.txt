cmake_minimum_required(VERSION 3.5)
project(cbg_executor_demo)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

include_directories(include)

add_executable(ping_node_only src/main.cpp src/cbg_executor_demo/ping_node.cpp src/cbg_executor_demo/parameter_helper.cpp)
target_compile_definitions(ping_node_only PRIVATE ADD_PING_NODE=1)
ament_target_dependencies(ping_node_only rclcpp std_msgs)

add_executable(pong_node_only src/main.cpp src/cbg_executor_demo/pong_node.cpp src/cbg_executor_demo/parameter_helper.cpp)
target_compile_definitions(pong_node_only PRIVATE ADD_PONG_NODE=1)
ament_target_dependencies(pong_node_only rclcpp std_msgs)

add_executable(ping_and_pong_node src/main.cpp src/cbg_executor_demo/ping_node.cpp src/cbg_executor_demo/pong_node.cpp src/cbg_executor_demo/parameter_helper.cpp)
target_compile_definitions(ping_and_pong_node PRIVATE ADD_PING_NODE=1 ADD_PONG_NODE=1)
ament_target_dependencies(ping_and_pong_node rclcpp std_msgs)

install(TARGETS ping_node_only pong_node_only ping_and_pong_node
  DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_export_dependencies(rclcpp std_msgs)
ament_package()
