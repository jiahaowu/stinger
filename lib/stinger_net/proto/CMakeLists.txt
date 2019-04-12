set(proto
	stinger-alg.proto
	stinger-batch.proto
	stinger-connect.proto
	stinger-monitor.proto
	)

set(_protobuf_proto_path "--proto_path=${CMAKE_CURRENT_LIST_DIR}")
set(_protobuf_include_path "-I=${CMAKE_SOURCE_DIR}/lib")
set(_protobuf_output_path "--cpp_out=${CMAKE_BINARY_DIR}")

#message("Protobuf protopath: ${_protobuf_proto_path}")
#message("Protobuf protopath: ${_protobuf_include_path}")
#message("Protobuf protopath: ${_protobuf_output_path}")

foreach(file ${proto})
	get_filename_component(_file_name ${file} NAME_WE)
	get_filename_component(_abs_file_name ${file} ABSOLUTE)

	add_custom_command(
		OUTPUT "${CMAKE_BINARY_DIR}/stinger_net/proto/${_file_name}.pb.cc" "${CMAKE_BINARY_DIR}/stinger_net/proto/${_file_name}.pb.h"
		COMMAND ${PROTOBUF_PROTOC_EXECUTABLE} ${_protobuf_include_path} ${_protobuf_proto_path} ${_protobuf_output_path} ${_abs_file_name}
		DEPENDS protobuf ${_abs_file_name}
		WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/bin
	)

	set(_generated_files ${_generated_files} "${CMAKE_BINARY_DIR}/stinger_net/proto/${_file_name}.pb.cc" "${CMAKE_BINARY_DIR}/stinger_net/proto/${_file_name}.pb.h")
endforeach()

#message("Files: ${_generated_files}")
include_directories(
  ${PROTOBUF_INCLUDE_DIR}
  "${CMAKE_BINARY_DIR}/"
  )
add_library(protomsg ${_generated_files})
target_link_libraries(protomsg _protobuf_library _protobuf_lite_library)
