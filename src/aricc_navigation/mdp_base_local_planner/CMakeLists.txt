cmake_minimum_required(VERSION 2.8.3)
project(mdp_base_local_planner)
set(ROS_BUILD_TYPE Release)

find_package(catkin REQUIRED
            COMPONENTS
            base_local_planner
            costmap_2d
            dynamic_reconfigure
            nav_core
            nav_msgs
            pluginlib
            pcl_conversions
            roscpp
            tf
            cmake_modules
        )

find_package(Eigen REQUIRED)

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${EIGEN_INCLUDE_DIRS}
    )

add_definitions(${EIGEN_DEFINITIONS})
link_directories(${catkin_LIBRARY_DIRS})

# dynamic reconfigure
generate_dynamic_reconfigure_options(
    cfg/CustBaseLocalPlanner.cfg
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES cust_base_local_planner
  CATKIN_DEPENDS
      dynamic_reconfigure
      pluginlib
      roscpp
)

add_library(mdp_base_local_planner src/trajectory_planner.cpp src/trajectory_planner_ros.cpp)
target_link_libraries(mdp_base_local_planner base_local_planner ${catkin_LIBRARIES})
#add_dependencies(cust_base_local_planner base_local_planner_gencfg)
#add_dependencies(cust_base_local_planner nav_msgs_gencpp)

#install(TARGETS cust_base_local_planner
#       LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#       )

#install(FILES blp_plugin.xml
#    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#)

#install(DIRECTORY include/${PROJECT_NAME}/
#  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#  PATTERN ".svn" EXCLUDE
#)


