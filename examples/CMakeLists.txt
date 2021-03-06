
macro(_local_test_ddp)
  set(list_var "${ARGN}")
  foreach(loop_var IN LISTS list_var)
    ADD_EXECUTABLE(${loop_var} ${loop_var}.cpp)
    TARGET_LINK_LIBRARIES(${loop_var} ${PROJECT_NAME}-examples 
      ${qpOASES_LIBRARIES})
    PKG_CONFIG_USE_DEPENDENCY(${loop_var} eigen3)
  endforeach()
endmacro()

SET(source_files
  pyrene_actuator/pyreneCostFunction.cpp
  pyrene_actuator/pyreneActuator.cpp
  romeo_actuator/costfunctionromeoactuator.cpp
  romeo_actuator/romeosimpleactuator.cpp
  romeo_actuator/romeotorqueactuator.cpp
  temperature_control/costtemp.cpp
  temperature_control/dctemp.cpp
  linear/costLinear.cpp
  linear/modelLinear.cpp
  inverse_pendulum/costIP.cpp
  inverse_pendulum/modelIP.cpp
)

SET(header_files
  pyrene_actuator/pyreneCostFunction.hh
  pyrene_actuator/pyreneActuator.hh
  romeo_actuator/costfunctionromeoactuator.hh
  romeo_actuator/romeosimpleactuator.hh
  romeo_actuator/romeotorqueactuator.hh
  temperature_control/costtemp.hh
  temperature_control/dctemp.hh
  )

ADD_LIBRARY(${PROJECT_NAME}-examples SHARED ${source_files})
SET(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake/find-external/qpOASES")
FIND_PACKAGE("qpOASES" REQUIRED)
INCLUDE_DIRECTORIES(SYSTEM ${qpOASES_INCLUDE_DIRS})

TARGET_LINK_LIBRARIES(${PROJECT_NAME}-examples ${qpOASES_LIBRARIES})
PKG_CONFIG_USE_DEPENDENCY(${PROJECT_NAME}-examples qpOASES)    
PKG_CONFIG_USE_DEPENDENCY(${PROJECT_NAME}-examples eigen3)

INSTALL(TARGETS ${PROJECT_NAME}-examples LIBRARY DESTINATION lib)
INSTALL(FILES ${header_files} DESTINATION include/ddp-actuator-solver/examples)

ADD_SUBDIRECTORY(pyrene_actuator)
ADD_SUBDIRECTORY(romeo_actuator)
ADD_SUBDIRECTORY(temperature_control)
ADD_SUBDIRECTORY(linear)
ADD_SUBDIRECTORY(inverse_pendulum)
