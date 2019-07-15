set(rv32uc_sc_tests
	rvc
)

add_custom_target(rv32uc)
compile_template(TVM rv32uc XLEN 32 ENV p TEST ${rv32uc_sc_tests})
compile_template(TVM rv32uc XLEN 32 ENV v TEST ${rv32uc_sc_tests})
