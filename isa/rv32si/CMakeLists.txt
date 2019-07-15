set(rv32si_sc_tests
	csr
	dirty
	ma_fetch
	scall
	sbreak
	wfi
)

add_custom_target(rv32si)
compile_template(TVM rv32si XLEN 32 ENV p TEST ${rv32si_sc_tests})
