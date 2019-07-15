enable_language(ASM)

if(NOT DEFINED RISCV_GCC)
	message(FATAL_ERROR
		"cannot find RISCV_GCC\n"
		"Fixxed by cmake -DRISCV_GCC=/path/to/compiler\n"
	)
endif()

set(CMAKE_ASM_COMPILER "${RISCV_GCC}")
set(CMAKE_ASM_FLAGS "${CMAKE_ASM_FLAGS} \
	-nostartfiles -nostdlib -static -fvisibility=hidden -mcmodel=medany"
)

set(CMAKE_C_COMPILER "${CMAKE_ASM_COMPILER}")
set(CMAKE_C_FLAGS "${CMAKE_ASM_FLAGS}")

if(NOT DEFINED XLEN)
	message(FATAL_ERROR "cannot find XLEN.")
endif()

if((NOT XLEN EQUAL 32) AND (NOT XLEN EQUAL 64))
	message(FATAL_ERROR "cannot support RV${XLEN}")
endif()

set(env "${CMAKE_SOURCE_DIR}/env")

if(NOT DEFINED RISCV_OBJDUMP)
	message(WARNING
		"cannot find RISCV_OBJDUMP\n"
		"Fixxed by cmake -DRISCV_OBJDUMP=/path/to/objdump\n"
	)
else()
	list(APPEND objdump_option
		--disassemble-all --disassemble-zeroes
		--section=.text --section=.text.startup
		--section=.text.init --section=.data
	)
endif()

if(NOT DEFINED SPIKE)
	message(WARNING
		"cannot find SPIKE\n"
		"Fixxed by cmake -DSPIKE=/path/to/spike\n"
	)
endif()

if(NOT DEFINED ENTROPY)
	string(RANDOM LENGTH 7 ALPHABET 0123456789ABCDEF ENTROPY)
endif()

function(append_compile_flag)
	cmake_parse_arguments(ARGS "" "TARGET;FLAG" "" ${ARGN})
	set_property(TARGET ${ARGS_TARGET}
		APPEND_STRING
		PROPERTY COMPILE_FLAGS " ${ARGS_FLAG}"
	)
	set_property(TARGET ${ARGS_TARGET}
		APPEND_STRING
		PROPERTY LINK_FLAGS " ${ARGS_FLAG}"
	)
endfunction()

function(set_abi_arch)
	cmake_parse_arguments(ARGS "" "TARGET;XLEN" "" ${ARGN})
	if(${ARGS_XLEN} EQUAL 32)
		append_compile_flag(TARGET ${ARGS_TARGET} FLAG "-mabi=ilp32 -march=rv32g")
	elseif(${ARGS_XLEN} EQUAL 64)
		append_compile_flag(TARGET ${ARGS_TARGET} FLAG "-mabi=lp64 -march=rv64g")
	endif()
endfunction()

function(set_ENV)
	cmake_parse_arguments(ARGS "" "TARGET;ENV" "" ${ARGN})
	target_include_directories(${ARGS_TARGET} PUBLIC "${env}/${ARGS_ENV}")
	append_compile_flag(TARGET ${ARGS_TARGET} FLAG "-T${env}/${ARGS_ENV}/link.ld")
	if(${ARGS_ENV} STREQUAL "v")
		target_compile_definitions(${ARGS_TARGET} PUBLIC ENTROPY=0x${ENTROPY})
		file(GLOB v_src ${env}/v/*.c)
		target_sources(${ARGS_TARGET} PUBLIC ${env}/v/entry.S ${v_src})
		append_compile_flag(TARGET ${ARGS_TARGET} FLAG "-std=gnu99 -O2")
	endif()
endfunction()

function(compile_template)
	cmake_parse_arguments(ARGS "" "TVM;XLEN;ENV" "TEST" ${ARGN})
	foreach(test IN LISTS ARGS_TEST)
		set(target ${ARGS_TVM}_${ARGS_ENV}_${test})
		add_executable(${target} ${CMAKE_CURRENT_SOURCE_DIR}/${test}.S)
		set_abi_arch(TARGET ${target} XLEN ${ARGS_XLEN})
		target_include_directories(${target}
			PUBLIC "${CMAKE_SOURCE_DIR}/isa/macros/scalar"
		)
		set_ENV(TARGET ${target} ENV ${ARGS_ENV})
		if(DEFINED RISCV_OBJDUMP)
			add_custom_command(TARGET ${target}
				POST_BUILD
				COMMAND ${RISCV_OBJDUMP} ${objdump_option}
					${target} > ${target}.dump
			)
		endif()
		if(DEFINED SPIKE)
			add_test(NAME ${target}
				COMMAND ${SPIKE} --isa=rv${ARGS_XLEN}gc
					${CMAKE_CURRENT_BINARY_DIR}/${target}
			)
		endif()
		add_dependencies(${ARGS_TVM} ${target})
		add_dependencies(scalar ${target})
	endforeach()
endfunction()

set(TVM mi si ua uc ud uf ui um)

add_custom_target(scalar)
foreach(mode IN LISTS TVM)
	add_subdirectory(rv32${mode})
endforeach()
if(XLEN EQUAL 64)
	foreach(mode IN LISTS TVM)
		add_subdirectory(rv64${mode})
	endforeach()
endif()
