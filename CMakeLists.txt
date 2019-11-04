PROJECT(pluCenterfind2D)

# min cmake version
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

SET(CMAKE_CXX_FLAGS "-std=c++11")

# default to debug
SET(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Debug or Release")

# configure debug / release flags
IF (CMAKE_BUILD_TYPE MATCHES Debug)
  # debug define
  ADD_DEFINITIONS(-DDEBUG=1)
  # Add -g for unix
  IF(UNIX)
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
  ENDIF(UNIX)
ELSEIF (CMAKE_BUILD_TYPE MATCHES Release)
  # Add unix optimization flag
  IF(UNIX)
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}  -O2")
  ENDIF(UNIX)
ENDIF (CMAKE_BUILD_TYPE MATCHES Debug)

IF(WIN32)
  IF(NOT IPP_INC_DIR)
    SET(IPP_INC_DIR $ENV{LIBDIR}/ipp/include CACHE STRING "IPP include directory")
  ENDIF(NOT IPP_INC_DIR)
  IF(NOT IPP_LIB_DIR)
    SET(IPP_LIB_DIR $ENV{LIBDIR}/ipp/lib/intel64 CACHE STRING "IPP library directory")
  ENDIF(NOT IPP_LIB_DIR)

  # FreeImage (header is next to library)
  SET(FREEIMG_INC_DIR $ENV{LIBDIR}/FreeImage/lib/x64 CACHE STRING "FreeImage include directory")
  SET(FREEIMG_LIB_DIR ${FREEIMG_INC_DIR} CACHE STRING "FreeImage library directory directory")
  SET(FREEIMG_LIB FreeImage.lib)
  
ELSE(WIN32)
  IF(NOT IPP_INC_DIR)
    SET(IPP_INC_DIR /usr/local/include)
  ENDIF(NOT IPP_INC_DIR)
  IF(NOT IPP_LIB_DIR)
    SET(IPP_LIB_DIR /usr/local/lib)
  ENDIF(NOT IPP_LIB_DIR)

  # FreeImage (this is where apt put it for me)
  SET(FREEIMG_INC_DIR /usr/include)
  SET(FREEIMG_LIB_DIR /usr/lib/x86_64-linux-gnu)
  SET(FREEIMG_LIB freeimage)
ENDIF(WIN32)

LINK_DIRECTORIES(${IPP_LIB_DIR} ${FREEIMG_LIB_DIR})

# Include directories
INCLUDE_DIRECTORIES(
    ${CMAKE_CURRENT_SOURCE_DIR}/source
    ${IPP_INC_DIR}
    ${FREEIMG_INC_DIR})

ADD_EXECUTABLE(pluCenterfind2D 
  ${CMAKE_CURRENT_SOURCE_DIR}/source/data_proc1.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/source/fileops1.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/source/image1.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/source/kernel1.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/source/params1.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/source/plu_2D_centerfind1.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/source/data_proc1.h
  ${CMAKE_CURRENT_SOURCE_DIR}/source/fileops1.h
  ${CMAKE_CURRENT_SOURCE_DIR}/source/image1.h
  ${CMAKE_CURRENT_SOURCE_DIR}/source/kernel1.h
  ${CMAKE_CURRENT_SOURCE_DIR}/source/params1.h
  ${CMAKE_CURRENT_SOURCE_DIR}/source/plu_2D_centerfind1.h
)

TARGET_LINK_LIBRARIES(pluCenterfind2D LINK_PUBLIC
  ippcc
  ippcore
  ippi
  ipps
  ${FREEIMG_LIB}
  )