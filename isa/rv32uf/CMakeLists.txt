set(rv32uf_sc_tests
	fadd fdiv fclass fcmp fcvt fcvt_w fmadd fmin
	ldst move recoding
)

add_custom_target(rv32uf)
compile_template(TVM rv32uf XLEN 32 ENV p TEST ${rv32uf_sc_tests})
compile_template(TVM rv32uf XLEN 32 ENV v TEST ${rv32uf_sc_tests})
