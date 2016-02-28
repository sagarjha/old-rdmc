cmake_minimum_required(VERSION 2.8)

PROJECT(verbs-mcast CXX)
SET(CMAKE_CXX_FLAGS "-std=c++14 -Wall -O3 -ggdb")

ADD_LIBRARY(rdmc rdmc.cpp util.cpp experiment.cpp group_send.cpp verbs_helper.cpp connection.cpp)
TARGET_LINK_LIBRARIES(rdmc rdmacm ibverbs boost_system rt pthread)

if (SLURM_LIBRARY)
  target_link_libraries(rdmc slurm)
  add_definitions(-DUSE_SLURM)
endif (SLURM_LIBRARY)

ADD_EXECUTABLE(verbs-mcast experiment.cpp)
TARGET_LINK_LIBRARIES(verbs-mcast rdmc)

add_custom_target(format clang-format-3.5 -i *.cpp *.h)
 
 