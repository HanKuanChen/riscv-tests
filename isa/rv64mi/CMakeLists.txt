set(rv64mi_sc_tests
	access
	breakpoint
	csr
	mcsr
	illegal
	ma_fetch
	ma_addr
	scall
	sbreak
)

add_custom_target(rv64mi)
compile_template(TVM rv64mi XLEN 64 ENV p TEST ${rv64mi_sc_tests})
