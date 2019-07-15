set(rv64si_sc_tests
	csr
	dirty
	ma_fetch
	scall
	wfi
	sbreak
)

add_custom_target(rv64si)
compile_template(TVM rv64si XLEN 64 ENV p TEST ${rv64si_sc_tests})
