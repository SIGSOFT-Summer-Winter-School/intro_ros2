cmake_minimum_required(VERSION 3.5)
project(ir2_vff_avoidance)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE Debug)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(visualization_msgs REQUIRED)

set(dependencies
    rclcpp
    sensor_msgs
    geometry_msgs
    visualization_msgs
)

include_directories(include)

add_library(${PROJECT_NAME} SHARED src/ir2_vff_avoidance/AvoidanceNode.cpp)
ament_target_dependencies(${PROJECT_NAME} ${dependencies})

add_executable(avoidance_vff src/avoidance_vff_main.cpp)
ament_target_dependencies(avoidance_vff ${dependencies})
target_link_libraries(avoidance_vff ${PROJECT_NAME})

install(TARGETS
  ${PROJECT_NAME}
  avoidance_vff
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY launch DESTINATION share/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()

  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()

  find_package(ament_cmake_gtest REQUIRED)
  add_subdirectory(tests)
endif()

ament_export_dependencies(${dependencies})
ament_package()
