CMAKE_MINIMUM_REQUIRED(VERSION 3.1)

IF(WIN32)
ELSE()
  ADD_DEFINITIONS( -DMYTIME )
ENDIF()

ADD_EXECUTABLE(vsploads
ADBSlicer.C
EngineFace.C
RotorDisk.C
adb2loads.C
binaryio.C
interp.C
quat.C
search.C
utils.C
ADBSlicer.H
ControlSurface.H
EngineFace.H
PropElement.H
RotorDisk.H
binaryio.H
interp.H
quat.H
surfIDs.H
utils.H
)

TARGET_LINK_LIBRARIES(vsploads
)

INSTALL( TARGETS vsploads RUNTIME DESTINATION . )
INSTALL( TARGETS vsploads RUNTIME DESTINATION python/openvsp/openvsp )

add_custom_command(TARGET vsploads POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
        $<TARGET_FILE:vsploads>
        ${CMAKE_BINARY_DIR}/python_pseudo/openvsp/openvsp/vsploads )

add_custom_command(TARGET vsploads POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
        $<TARGET_FILE:vsploads>
        ${CMAKE_BINARY_DIR}/src/vsp/vsploads )
