CMAKE_MINIMUM_REQUIRED(VERSION 3.1)

PROJECT (VSPAERO)

# add the OpenVSP specific cmake files to module path
set(CMAKE_MODULE_PATH ${VSPAERO_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

INCLUDE( SetupC++11 )

IF(CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64" OR CMAKE_SYSTEM_PROCESSOR MATCHES "amd64")
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
  SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
ENDIF()

IF( VSPAERO_LIBRARY_PATH )
	INCLUDE( ${VSPAERO_LIBRARY_PATH}/VSPAERO_Libraries_Config.cmake )
ENDIF( VSPAERO_LIBRARY_PATH )

FIND_PACKAGE( Doxygen )


IF( NOT VSP_NO_GRAPHICS )
    SET( CMAKE_PREFIX_PATH ${FLTK_INSTALL_DIR} ${ORIG_CMAKE_PREFIX_PATH} )
    FIND_PACKAGE(FLTK REQUIRED)
    SET( FLTK_INCLUDE_DIR ${FLTK_INCLUDE_DIR} PARENT_SCOPE )
    SET( FLTK_LIBRARIES ${FLTK_LIBRARIES} PARENT_SCOPE )

    IF( UNIX )
        EXECUTE_PROCESS( COMMAND which ${FLTK_FLUID_EXECUTABLE}
                        OUTPUT_VARIABLE FLTK_FLUID_EXECUTABLE
                        OUTPUT_STRIP_TRAILING_WHITESPACE )
    ENDIF( UNIX )

    SET( FLTK_FLUID_EXECUTABLE ${FLTK_FLUID_EXECUTABLE} PARENT_SCOPE )
    SET( OPENGL_LIBRARIES ${OPENGL_LIBRARIES} PARENT_SCOPE )


    ADD_SUBDIRECTORY( stb )

ENDIF()


ADD_SUBDIRECTORY( ../ vsp_aero )
