cmake_minimum_required(VERSION 3.8)
project(barista_robot_description)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(urdf REQUIRED)
find_package(xacro REQUIRED)
find_package(ros2_control REQUIRED)
find_package(gazebo_ros2_control REQUIRED)
find_package(joint_state_broadcaster REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(tf2_geometry_msgs  REQUIRED)
find_package(robot_chase  REQUIRED)

# find_package(teleop_twist_keyboard  REQUIRED)
add_subdirectory(robot_chase)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

add_executable(world_to_odom src/world_to_odom.cpp)
ament_target_dependencies(world_to_odom rclcpp nav_msgs geometry_msgs tf2_ros)

ament_target_dependencies(robot_chase robot_chase)


install(TARGETS
   world_to_odom
   DESTINATION lib/${PROJECT_NAME}
 )

install(
  DIRECTORY
    urdf
    rviz
    launch
    meshes
    config
	models
	worlds
	xacro
  DESTINATION
    share/${PROJECT_NAME}/
)

ament_package()
