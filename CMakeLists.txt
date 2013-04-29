cmake_minimum_required(VERSION 2.8)

PROJECT( hesaff )

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/../g/external/cmake-find_tbb/")

find_package( OpenCV 2.3 REQUIRED )

find_package( TBB 4.0 )
if(TBB_FOUND)
  include_directories(${TBB_INCLUDE_DIRS})
  link_directories(${TBB_LIBRARY_DIRS})
endif(TBB_FOUND)
if(TBB_FOUND AND WIN32)
  install (FILES "${TBB_INSTALL_DIR}/tbb.dll" "${TBB_INSTALL_DIR}/tbb_debug.dll"
           DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
endif(TBB_FOUND AND WIN32)

include_directories( ${CMAKE_CURRENT_SOURCE_DIR} )


add_library (pyramid pyramid.cpp)
add_library (affine affine.cpp)
add_library (siftdesc siftdesc.cpp)
add_library (helpers helpers.cpp)

# Executables and launchers
#

# Main

ADD_EXECUTABLE( hesaff hesaff.cpp )
if(TBB_FOUND)
  TARGET_LINK_LIBRARIES( hesaff  ${TBB_LIBRARIES} )
endif(TBB_FOUND)
TARGET_LINK_LIBRARIES( hesaff pyramid affine siftdesc helpers ${OpenCV_LIBS} )


