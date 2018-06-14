cmake_minimum_required(VERSION 3.0)
project(pubbydb_proto)
set(CMAKE_CXX_STANDARD 11)
include(FindProtobuf)
find_package(Protobuf REQUIRED)
PROTOBUF_GENERATE_CPP(PUBBYDB_PROTO_SRC PUBBYDB_PROTO_HEADER
        request.proto
        response.proto)
set(PUBBYDB_PROTO_LIBRARY pubbydb_proto CACHE STRING "The name of the PubbyDB Protobuf library.")
add_library(${PUBBYDB_PROTO_LIBRARY} STATIC ${PUBBYDB_PROTO_HEADER} ${PUBBYDB_PROTO_SRC})
target_include_directories(${PUBBYDB_PROTO_LIBRARY} PUBLIC ${PROTOBUF_INCLUDE_DIR})