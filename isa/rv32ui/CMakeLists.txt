set(rv32ui_sc_tests
	simple
	add addi
	and andi
	auipc
	beq bge bgeu blt bltu bne
	fence_i
	jal jalr
	lb lbu lh lhu lw
	lui
	or ori
	sb sh sw
	sll slli
	slt slti sltiu sltu
	sra srai
	srl srli
	sub
	xor xori
)

add_custom_target(rv32ui)
compile_template(TVM rv32ui XLEN 32 ENV p TEST ${rv32ui_sc_tests})
compile_template(TVM rv32ui XLEN 32 ENV v TEST ${rv32ui_sc_tests})
