# Mac OS X
IF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    SET( VIDEO_SRC vtkVideoSource2Factory.cxx
                        vtkVideoSource2.cxx
                        vtkVideoBuffer.cxx )

    SET( VIDEO_HDR vtkVideoSource2Factory.h
                        vtkVideoSource2.h
                        vtkVideoBuffer.h )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

# Linux
IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    SET( VIDEO_SRC vtkVideoSource2Factory.cxx
                        vtkV4L2VideoSource2.cxx
                        vtkVideoSource2.cxx
                        vtkVideoBuffer.cxx )

    SET( VIDEO_HDR vtkVideoSource2Factory.h
                        vtkV4L2VideoSource2.h
                        vtkVideoSource2.h
                        vtkVideoBuffer.h )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

# Windows
IF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    SET( VIDEO_SRC vtkVideoSource2Factory.cxx
                        vtkVideoSource2.cxx
                        vtkVideoBuffer.cxx )

    SET( VIDEO_HDR vtkVideoSource2Factory.h
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
        set( VIDEO_SRC ${VIDEO_SRC} vtkPointGreyVideoSource2.cxx )
        set( VIDEO_HDR ${VIDEO_HDR} vtkPointGreyVideoSource2.h )
        set( DEFINE_USE_POINT_GREY_CAMERA 1 )
    endif()
endif()

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    SET( DEFINE_USE_V4L2 1 )
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

CONFIGURE_FILE( ${CMAKE_CURRENT_SOURCE_DIR}/vtkVideoConfigure.h.in ${CMAKE_CURRENT_BINARY_DIR}/vtkVideoConfigure.h )

#================================
# Define sources
#================================
ADD_LIBRARY( vtkVideo ${VIDEO_SRC} ${VIDEO_HDR} )
target_link_libraries( vtkVideo vtkExtensions )
target_include_directories( vtkVideo PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} PRIVATE ${CMAKE_CURRENT_BINARY_DIR} )  # to include vtkVideoConfigure.h