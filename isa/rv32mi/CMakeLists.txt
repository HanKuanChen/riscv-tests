set(rv32mi_sc_tests
	breakpoint
	csr
	mcsr
	illegal
	ma_fetch
	ma_addr
	scall
	sbreak
	shamt
)

add_custom_target(rv32mi)
compile_template(TVM rv32mi XLEN 32 ENV p TEST ${rv32mi_sc_tests})
