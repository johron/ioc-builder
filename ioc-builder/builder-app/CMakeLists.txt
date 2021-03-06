set(target_name "builder-app")

set(src_precompiled
    "src/precompiled.h")

set(src_main
    "src/main.cpp")

set(src_modules
	"src/modules/base.h"
	"src/modules/network.h"
	"src/modules/tracking.h"
	"src/modules/store.h")

set(res_textures
	"res/textures/default.png")

set(res_files
	${res_textures})

set(src_files
    ${src_precompiled}
	${src_main}
	${src_modules}
	${res_files})

add_executable(${target_name} ${src_files})

source_group("src\\precompiled" FILES ${src_precompiled})
source_group("src\\main" FILES ${src_main})
source_group("src\\modules" FILES ${src_modules})
source_group("res\\textures" FILES ${res_textures})

target_link_libraries(${target_name} PUBLIC "compiler_warnings")
target_link_libraries(${target_name} PUBLIC "compiler_features")
target_link_libraries(${target_name} PUBLIC "sanitizers")
target_link_libraries(${target_name} PUBLIC "builder-lib")

target_include_directories(${target_name} PRIVATE "../builder-lib/include")

set_target_properties(${target_name} PROPERTIES FOLDER "ioc-builder")
set_target_properties(${target_name} PROPERTIES LINKER_LANGUAGE CXX)

target_precompile_headers(${target_name} PUBLIC ${src_precompiled})

foreach(file ${res_files})
	add_custom_command(TARGET ${target_name} 
		POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_if_different
		${CMAKE_CURRENT_SOURCE_DIR}/${file}
		${CMAKE_CURRENT_BINARY_DIR}/${file})
endforeach(file ${res_files})