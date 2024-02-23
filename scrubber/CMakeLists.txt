find_program(TTEXE TextTransformCore.exe)
add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/ScrubHeaders.cpp
	COMMAND cmake -E copy_if_different ${CMAKE_SOURCE_DIR}/scrubber/ScrubHeaders.tt ${CMAKE_CURRENT_SOURCE_DIR}
	COMMAND cmake -E chdir ${CMAKE_CURRENT_SOURCE_DIR} ${TTEXE} ScrubHeaders.tt
	DEPENDS ${CMAKE_SOURCE_DIR}/scrubber/ScrubHeaders.tt ${TTEXE})

add_executable(scrubber ${CMAKE_CURRENT_SOURCE_DIR}/ScrubHeaders.cpp)
target_include_directories(scrubber PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../include)
