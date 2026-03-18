cmake_minimum_required(VERSION 3.5)
project(fr3_description)
find_package(ament_cmake REQUIRED)

install(DIRECTORY
    launch robots rviz mjcf
    DESTINATION share/${PROJECT_NAME}/
)

ament_package()