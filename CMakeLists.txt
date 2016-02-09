CMAKE_MINIMUM_REQUIRED(VERSION 2.6 FATAL_ERROR)
CMAKE_POLICY(VERSION 2.6)

SET(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake" "${CMAKE_MODULE_PATH}")

# FIND_PACKAGE(Torch REQUIRED)
# FIND_PACKAGE(Protobuf REQUIRED)

# MESSAGE(STATUS Torch)
# MESSAGE(STATUS Protobuf)
# MESSAGE(STATUS ${Torch_INSTALL_INCLUDE})

add_definitions(-std=c++11)
add_definitions(-stdlib=libc++)
INCLUDE_DIRECTORIES("../../lib")
INCLUDE_DIRECTORIES("../../protobuf/protobuf/include")

# INCLUDE_DIRECTORIES("${Torch_INSTALL_INCLUDE}/TH" ${PROTOBUF_INCLUDE_DIRS})
# LINK_DIRECTORIES("${Torch_INSTALL_LIB}")

# PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS caffe.proto)

SET(src loadcaffe.cpp ${PROTO_SRCS})

FILE(GLOB luasrc *.lua)

ADD_TORCH_PACKAGE(loadcaffe "${src}" "${luasrc}" "Loadcaffe")

# ADD_LIBRARY(loadcaffe MODULE ${src})
TARGET_LINK_LIBRARIES(loadcaffe TH ${PROTOBUF_LIBRARIES})
TARGET_LINK_LIBRARIES(loadcaffe "../../protobuf/protobuf/platform/arm64-ios/lib-protobuf-lite.a")

### Torch packages supposes libraries prefix is "lib"
SET_TARGET_PROPERTIES(loadcaffe PROPERTIES
  PREFIX "lib"
  IMPORT_PREFIX "lib")
    
# INSTALL(TARGETS loadcaffe
#   RUNTIME DESTINATION "${Torch_INSTALL_LUA_CPATH_SUBDIR}"
#   LIBRARY DESTINATION "${Torch_INSTALL_LUA_CPATH_SUBDIR}")
# 
# INSTALL(
#   FILES
#   ${luasrc}
#   DESTINATION "${Torch_INSTALL_LUA_PATH_SUBDIR}/loadcaffe")
