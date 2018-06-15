cmake_minimum_required(VERSION 3.0)
project(pubbydb_proto)
set(CMAKE_CXX_STANDARD 11)
include(FindProtobuf)
find_package(Protobuf REQUIRED)
PROTOBUF_GENERATE_CPP(PUBBYDB_PROTO_SRC PUBBYDB_PROTO_HEADER
        record.proto
        request.proto
        response.proto)
set(PUBBYDB_PROTO_DIR ${CMAKE_CURRENT_LIST_DIR} CACHE FILEPATH "The path to PubbyDB Protobuf definitions.")
set(PUBBYDB_PROTO_HEADERS ${PUBBYDB_PROTO_HEADER} CACHE LIST "The headers to include to use PubbyDB Protobufs.")
set(PUBBYDB_PROTO_LIBRARY pubbydb_proto CACHE STRING "The name of the PubbyDB Protobuf library.")
add_library(${PUBBYDB_PROTO_LIBRARY} STATIC ${PUBBYDB_PROTO_HEADER} ${PUBBYDB_PROTO_SRC})
target_include_directories(${PUBBYDB_PROTO_LIBRARY} PUBLIC ${PROTOBUF_INCLUDE_DIR})

# Include the path for each Protobuf include
macro(target_include_pubbydb_protobuf target)
    target_include_directories(${target} PUBLIC ${PROTOBUF_INCLUDE_DIR})
    target_link_libraries(${target} ${PROTOBUF_LIBRARY})
    foreach (PROTO_FILE ${PUBBYDB_PROTO_HEADERS})
        get_filename_component(PROTO_DIR ${PROTO_FILE} DIRECTORY)
        target_include_directories(${target} PUBLIC ${PROTO_DIR})
    endforeach ()
endmacro()