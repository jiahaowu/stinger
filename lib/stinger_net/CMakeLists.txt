set(sources
	src/mon_handling.cpp
	src/send_rcv.cpp
	src/stinger_alg.cpp
	src/stinger_alg_state.cpp
	src/stinger_alg_state_c.cpp
	src/stinger_mon.cpp
	src/stinger_mon_c.cpp
	src/stinger_mon_state.cpp
	src/stinger_server_state.cpp
	src/stinger_stream.cpp
	src/stinger_local_state_c.cpp
)

set(headers
	inc/mon_handling.h
	inc/send_rcv.h
	inc/stinger_alg.h
	inc/stinger_alg_state.h
	inc/stinger_alg_state_c.h
	inc/stinger_mon.h
	inc/stinger_mon_c.h
	inc/stinger_mon_state.h
	inc/stinger_server_state.h
	inc/stinger_stream.h
	inc/stinger_stream_state.h
	inc/stinger_local_state_c.h
)

add_subdirectory(proto)

publish_headers(headers "${CMAKE_BINARY_DIR}/include/stinger_net")

include_directories("${CMAKE_BINARY_DIR}/")
include_directories("${CMAKE_BINARY_DIR}/stinger_net")
include_directories("${CMAKE_BINARY_DIR}/include/stinger_net")

add_library(stinger_net ${sources} ${headers})
target_link_libraries(stinger_net protomsg _protobuf_library _protobuf_lite_library stinger_core stinger_utils)
