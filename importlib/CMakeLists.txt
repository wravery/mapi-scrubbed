add_custom_command(OUTPUT
		${CMAKE_CURRENT_SOURCE_DIR}/olmapi32.dll.exp
		${CMAKE_CURRENT_SOURCE_DIR}/olmapi32.dll.lib
	COMMAND lib /nologo /machine:x64 /out:olmapi32.dll.lib /def:${CMAKE_CURRENT_SOURCE_DIR}/olmapi32.def
	COMMAND cmake -E copy_if_different olmapi32.dll.exp olmapi32.dll.lib ${CMAKE_CURRENT_SOURCE_DIR}
	DEPENDS olmapi32.def)
add_custom_target(copyimportlib ALL DEPENDS
	${CMAKE_CURRENT_SOURCE_DIR}/olmapi32.dll.exp
	${CMAKE_CURRENT_SOURCE_DIR}/olmapi32.dll.lib)
