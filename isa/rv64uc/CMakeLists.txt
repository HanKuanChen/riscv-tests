set(rv64uc_sc_tests
	rvc
)

add_custom_target(rv64uc)
compile_template(TVM rv64uc XLEN 64 ENV p TEST ${rv64uc_sc_tests})
compile_template(TVM rv64uc XLEN 64 ENV v TEST ${rv64uc_sc_tests})
