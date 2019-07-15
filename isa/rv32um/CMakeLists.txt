set(rv32um_sc_tests
	div divu
	mul mulh mulhsu mulhu
	rem remu
)

add_custom_target(rv32um)
compile_template(TVM rv32um XLEN 32 ENV p TEST ${rv32um_sc_tests})
compile_template(TVM rv32um XLEN 32 ENV v TEST ${rv32um_sc_tests})
