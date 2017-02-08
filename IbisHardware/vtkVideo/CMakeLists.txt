PROJECT( IGNS_VTK_VIDEO )

# Mac OS X
IF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    SET( IGNS_VTK_VIDEO_SRC vtkVideoSource2Factory.cxx
                        vtkVideoSource2.cxx
                        vtkVideoBuffer.cxx )

    SET( IGNS_VTK_VIDEO_HDR vtkVideoSource2Factory.h
                        vtkVideoSource2.h
                        vtkVideoBuffer.h )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

# Linux
IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    SET( IGNS_VTK_VIDEO_SRC vtkVideoSource2Factory.cxx
                        vtkV4L2VideoSource2.cxx
                        vtkVideoSource2.cxx
                        vtkVideoBuffer.cxx )

    SET( IGNS_VTK_VIDEO_HDR vtkVideoSource2Factory.h
                        vtkV4L2VideoSource2.h
                        vtkVideoSource2.h
                        vtkVideoBuffer.h )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

# Windows
IF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    SET( IGNS_VTK_VIDEO_SRC vtkVideoSource2Factory.cxx
                        vtkVideoSource2.cxx
                        vtkVideoBuffer.cxx )

    SET( IGNS_VTK_VIDEO_HDR vtkVideoSource2Factory.h
                        vtkVideoSource2.h
                        vtkVideoBuffer.h )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")


#================================
# Check to see if we have want
# to use PointGrey flycapture
#================================
if( ${CMAKE_SYSTEM_NAME} MATCHES "Linux" )
    option( USE_POINT_GREY_CAMERA "On if you want to build interface to PointGrey's Flycapture api (for capture from PTG cameras)" OFF )
    if( USE_POINT_GREY_CAMERA )
        set( IGNS_VTK_VIDEO_SRC ${IGNS_VTK_VIDEO_SRC} vtkPointGreyVideoSource2.cxx )
        set( IGNS_VTK_VIDEO_HDR ${IGNS_VTK_VIDEO_HDR} vtkPointGreyVideoSource2.h )
        set( DEFINE_USE_POINT_GREY_CAMERA 1 )
    endif()
endif()

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    SET( DEFINE_USE_V4L2 1 )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

CONFIGURE_FILE( ${CMAKE_CURRENT_SOURCE_DIR}/vtkVideoConfigure.h.in ${CMAKE_CURRENT_BINARY_DIR}/vtkVideoConfigure.h )

include_directories( ${CMAKE_CURRENT_BINARY_DIR} )  # to include vtkVideoConfigure.h
include_directories( ${VTK_EXTENSIONS_INCLUDE_DIR} )

#================================
# Define sources
#================================
ADD_LIBRARY( vtkVideo ${IGNS_VTK_VIDEO_SRC} ${IGNS_VTK_VIDEO_HDR} )

if( IGNS_INSTALL_LIBS_AND_HEADERS )
    INSTALL_TARGETS(/lib vtkVideo)
    INSTALL_FILES(/include FILES ${IGNS_VTK_VIDEO_HDR})
endif( IGNS_INSTALL_LIBS_AND_HEADERS )

#================================
# Define include dir for
# dependent projects
#================================
SET( VTK_VIDEO_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR};${CMAKE_CURRENT_BINARY_DIR}" PARENT_SCOPE )
