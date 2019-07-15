set(rv64ua_sc_tests
	amoadd_d amoand_d amomax_d amomaxu_d amomin_d amominu_d amoor_d amoxor_d amoswap_d
	amoadd_w amoand_w amomax_w amomaxu_w amomin_w amominu_w amoor_w amoxor_w amoswap_w
	lrsc
)

add_custom_target(rv64ua)
compile_template(TVM rv64ua XLEN 64 ENV p TEST ${rv64ua_sc_tests})
compile_template(TVM rv64ua XLEN 64 ENV v TEST ${rv64ua_sc_tests})
