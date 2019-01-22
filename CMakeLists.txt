#--------------------------------------------------------
# The CMakeLists.txt for:                uFldHazardSensor
# Author(s):                                Mike Benjamin
#--------------------------------------------------------
cmake_minimum_required(VERSION 3.5)
find_package(PkgConfig)
pkg_check_modules(PC_ZeroMQ QUIET zmq)

find_path(ZeroMQ_INCLUDE_DIR
        NAMES zmq.hpp
        PATHS ${PC_ZeroMQ_INCLUDE_DIRS}
        )
find_library(ZeroMQ_LIBRARY
        NAMES zmq
        PATHS ${PC_ZeroMQ_LIBRARY_DIRS}
        )

# Set System Specific Libraries
if (${WIN32})
  SET(SYSTEM_LIBS
    wsock32)
else (${WIN32})
  SET(SYSTEM_LIBS
    m
    pthread)
endif (${WIN32})

SET(SRC
   HazardSensor_MOOSApp.cpp
   HazardSensor_Info.cpp
   HeadingHistory.cpp
   main.cpp
)

ADD_EXECUTABLE(uFldHazardSensor ${SRC})

target_include_directories(uFldHazardSensor PUBLIC ${ZeroMQ_INCLUDE_DIR})

TARGET_LINK_LIBRARIES(uFldHazardSensor
  ${MOOS_LIBRARIES}
  ufld_hazards
  mbutil
  geometry
  contacts
  apputil
  ${SYSTEM_LIBS}
  ${ZeroMQ_LIBRARY}
)
