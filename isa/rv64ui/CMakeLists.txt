set(rv64ui_sc_tests
	add addi addiw addw
	and andi
	auipc
	beq bge bgeu blt bltu bne
	simple
	fence_i
	jal jalr
	lb lbu lh lhu lw lwu ld
	lui
	or ori
	sb sh sw sd
	sll slli slliw sllw
	slt slti sltiu sltu
	sra srai sraiw sraw
	srl srli srliw srlw
	sub subw
	xor xori
)

add_custom_target(rv64ui)
compile_template(TVM rv64ui XLEN 64 ENV p TEST ${rv64ui_sc_tests})
compile_template(TVM rv64ui XLEN 64 ENV v TEST ${rv64ui_sc_tests})
