set(rv64um_sc_tests
	div divu divuw divw
	mul mulh mulhsu mulhu mulw
	rem remu remuw remw
)

add_custom_target(rv64um)
compile_template(TVM rv64um XLEN 64 ENV p TEST ${rv64um_sc_tests})
compile_template(TVM rv64um XLEN 64 ENV v TEST ${rv64um_sc_tests})
