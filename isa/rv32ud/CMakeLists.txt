set(rv32ud_sc_tests
	fadd fdiv fclass fcmp fcvt fcvt_w fmadd fmin
	ldst recoding
)

add_custom_target(rv32ud)
compile_template(TVM rv32ud XLEN 32 ENV p TEST ${rv32ud_sc_tests})
compile_template(TVM rv32ud XLEN 32 ENV v TEST ${rv32ud_sc_tests})
