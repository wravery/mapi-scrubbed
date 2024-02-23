find_program(TTEXE TextTransformCore.exe)
add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/ScrubHeaders.cpp
	COMMAND cmake -E copy_if_different ${CMAKE_SOURCE_DIR}/scrubber/ScrubHeaders.tt ${CMAKE_CURRENT_SOURCE_DIR}
	COMMAND cmake -E chdir ${CMAKE_CURRENT_SOURCE_DIR} ${TTEXE} ScrubHeaders.tt
	DEPENDS ${CMAKE_SOURCE_DIR}/scrubber/ScrubHeaders.tt ${TTEXE})

add_executable(scrubber ${CMAKE_CURRENT_SOURCE_DIR}/ScrubHeaders.cpp)
target_include_directories(scrubber PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../include)

add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/MAPIConstants.h
	COMMAND scrubber > ${CMAKE_CURRENT_BINARY_DIR}/MAPIConstants.h VERBATIM
	DEPENDS scrubber)

file(GLOB MAPI_HEADERS LIST_DIRECTORIES false "${CMAKE_CURRENT_SOURCE_DIR}/../include/*.h")
add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/copied_headers
	COMMAND cmake -E copy_if_different ${MAPI_HEADERS} ${CMAKE_CURRENT_BINARY_DIR}/MAPIConstants.h ${CMAKE_SOURCE_DIR}/include
	COMMAND cmake -E touch ${CMAKE_CURRENT_BINARY_DIR}/copied_headers
	DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/MAPIConstants.h)
add_custom_target(copyheaders ALL DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/copied_headers)
