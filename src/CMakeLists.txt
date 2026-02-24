if(BUILD_UNIT_TESTS)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DUNIT_TEST")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DUNIT_TEST")
elseif(NOT DEBUG)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DNDEBUG")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DNDEBUG")
endif()

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/braft)

message("braft SOURCES_1=${SOURCES}")
#SET(SOURCES /home/ldy/project/mobiusdb/mysql-8.0.30/extra/braft/src/braft)
message("braft SOURCES_2=${SOURCES}")
add_library(BRAFT_OBJ_LIB OBJECT ${SOURCES})
#message("BRAFT_PROTOBUF_LITE_LIB=${BRAFT_PROTOBUF_LITE_LIB}")
#add_dependencies(RAFT_OBJ_LIB ${BRAFT_PROTOBUF_LITE_LIB})
#add_dependencies(RAFT_OBJ_LIB ${INCUBATOR_BRPC_LIBRARY})
#add_dependencies(RAFT_OBJ_LIB ${TDC_PROTOBUF_LITE_LIB})

set_property(TARGET ${BRAFT_OBJ_LIB} PROPERTY POSITION_INDEPENDENT_CODE 1)
add_library(braft-shared SHARED $<TARGET_OBJECTS:BRAFT_OBJ_LIB>)
add_library(braft-static STATIC $<TARGET_OBJECTS:BRAFT_OBJ_LIB>)
target_link_libraries(braft-shared ${DYNAMIC_LIB})
target_link_libraries(braft-static ${DYNAMIC_LIB})

SET_TARGET_PROPERTIES(braft-static PROPERTIES OUTPUT_NAME braft CLEAN_DIRECT_OUTPUT 1)
SET_TARGET_PROPERTIES(braft-shared PROPERTIES OUTPUT_NAME braft CLEAN_DIRECT_OUTPUT 1)

install(TARGETS braft-static
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib${LIBSUFFIX}
        ARCHIVE DESTINATION lib${LIBSUFFIX}
        )

install(TARGETS braft-shared
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib${LIBSUFFIX}
        ARCHIVE DESTINATION lib${LIBSUFFIX}
        )
