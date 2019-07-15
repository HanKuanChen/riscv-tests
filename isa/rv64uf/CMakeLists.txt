set(rv64uf_sc_tests
	fadd fdiv fclass fcmp fcvt fcvt_w fmadd fmin
	ldst move recoding
)

add_custom_target(rv64uf)
compile_template(TVM rv64uf XLEN 64 ENV p TEST ${rv64uf_sc_tests})
compile_template(TVM rv64uf XLEN 64 ENV v TEST ${rv64uf_sc_tests})
