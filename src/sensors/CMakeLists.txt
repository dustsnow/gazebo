include (${gazebo_cmake_dir}/GazeboUtils.cmake)

set (sources Sensors.cc
             Sensor.cc
             SensorFactory.cc
             SensorManager.cc
             CameraSensor.cc
             DepthCameraSensor.cc
             RaySensor.cc
             GpuRaySensor.cc
             ContactSensor.cc
             RFIDSensor.cc
             RFIDTag.cc
             RFIDTagManager.cc
) 

set (headers Sensors.hh
             Sensor.hh
             SensorTypes.hh
             SensorFactory.hh
             SensorManager.hh
             CameraSensor.hh
             DepthCameraSensor.hh
             RaySensor.hh
             GpuRaySensor.hh
             ContactSensor.hh
             RFIDSensor.hh
             RFIDTag.hh
             RFIDTagManager.hh
)

set (sensors_headers "" CACHE INTERNAL "sensors headers" FORCE)
foreach (hdr ${headers})
  APPEND_TO_CACHED_STRING(sensors_headers "sensors headers" "#include \"sensors/${hdr}\"\n")
endforeach()
configure_file ( ${CMAKE_CURRENT_SOURCE_DIR}/sensors.h.in ${CMAKE_CURRENT_BINARY_DIR}/sensors.h )


add_library(gazebo_sensors SHARED ${sources})

target_link_libraries(gazebo_sensors gazebo_common 
                                     gazebo_rendering
                                     gazebo_physics
                                     ${libtool_library} 
                                     ${boost_libraries}
                                     ${ogre_ldflags}
                                     )

add_executable(gzsensor server.cc ../gazebo.cc)
target_link_libraries( gzsensor gazebo_common
                                gazebo_transport
                                gazebo_sensors
                                gazebo_rendering
                                gazebo_msgs
                                pthread)

INSTALL_EXECUTABLE( gzsensor )
INSTALL_LIBRARY(gazebo_sensors)
INSTALL_INCLUDES("sensors" ${headers})
