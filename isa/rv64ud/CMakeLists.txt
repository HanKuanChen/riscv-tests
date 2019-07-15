set(rv64ud_sc_tests
	fadd fdiv fclass fcmp fcvt fcvt_w fmadd fmin
	ldst move structural recoding
)

add_custom_target(rv64ud)
compile_template(TVM rv64ud XLEN 64 ENV p TEST ${rv64ud_sc_tests})
compile_template(TVM rv64ud XLEN 64 ENV v TEST ${rv64ud_sc_tests})
