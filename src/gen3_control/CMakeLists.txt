cmake_minimum_required(VERSION 3.8)
project(gen3_control)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Wno-reorder -g)
endif()

# Activate C++ 11
set (CMAKE_CXX_STANDARD 11)
add_definitions(-D_OS_UNIX) 

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(trajectory_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(std_srvs REQUIRED)

add_executable(gen3_vel_control src/gen3_vel_control.cpp src/kortex_robot.cpp src/robot_util.cpp)
add_executable(gen3_vel_control_sim src/gen3_vel_control_sim.cpp  src/robot_util.cpp)
add_executable(gen3_control src/gen3_control.cpp src/kortex_robot.cpp src/robot_util.cpp)

ament_target_dependencies(gen3_vel_control PUBLIC
  rclcpp
  Eigen3
  sensor_msgs
  trajectory_msgs
  std_msgs
  std_srvs
  )
ament_target_dependencies(gen3_control PUBLIC
  rclcpp
  Eigen3
  sensor_msgs
  trajectory_msgs
  std_msgs
  )

ament_target_dependencies(gen3_vel_control_sim PUBLIC
  rclcpp
  Eigen3
  sensor_msgs
  trajectory_msgs
  )

target_link_libraries(gen3_vel_control PUBLIC
  pthread
  ${PROJECT_SOURCE_DIR}/lib/release/libKortexApiCpp.a
  )
target_link_libraries(gen3_control PUBLIC
  pthread
  ${PROJECT_SOURCE_DIR}/lib/release/libKortexApiCpp.a
  )
target_link_libraries(gen3_vel_control_sim PUBLIC
  pthread
  ${PROJECT_SOURCE_DIR}/lib/release/libKortexApiCpp.a
  )

target_include_directories(gen3_vel_control
  PUBLIC
  # ${PROJECT_SOURCE_DIR}/include
  ${PROJECT_SOURCE_DIR}/include/gen3_control
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client_stubs
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client/common
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client/messages
  )
target_include_directories(gen3_control
  PUBLIC
  # ${PROJECT_SOURCE_DIR}/include
  ${PROJECT_SOURCE_DIR}/include/gen3_control
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client_stubs
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client/common
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client/messages
  )
target_include_directories(gen3_vel_control_sim
  PUBLIC
  # ${PROJECT_SOURCE_DIR}/include
  ${PROJECT_SOURCE_DIR}/include/gen3_control
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client_stubs
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client/common
  # ${PROJECT_SOURCE_DIR}/include/gen3_control/client/messages
  )

#Install include
install(DIRECTORY include
  DESTINATION include/${PROJECT_NAME}
)
install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)
install(DIRECTORY config
  DESTINATION share/${PROJECT_NAME}
)
#Install exe
install(
  TARGETS 
    gen3_vel_control
    gen3_vel_control_sim
    gen3_control
  DESTINATION lib/${PROJECT_NAME}
)

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

ament_package()
