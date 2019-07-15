set(rv32ua_sc_tests
	amoadd_w amoand_w amomax_w amomaxu_w amomin_w amominu_w amoor_w amoxor_w amoswap_w
	lrsc
)

add_custom_target(rv32ua)
compile_template(TVM rv32ua XLEN 32 ENV p TEST ${rv32ua_sc_tests})
compile_template(TVM rv32ua XLEN 32 ENV v TEST ${rv32ua_sc_tests})
