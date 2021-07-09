---
layout: post
title: "About the Testing of My Simple RISC-V Core"
data: 2021-07-09 11:00:00 +800
comments: true
categories: [Experience]
---

Writing one's custom RISC-V core is cool, isn't? And it's not that hard -- like, if stick to RV32I, give up pipeline, use simple MMIO instead of buses, it can be done just in a few days. 

But will it run correctly? Certainly, it passed my less-than-50-line assembly test, but after a case of hours of debugging turning out to be an inadequate understanding of specifications, resulting in mistakes of sign-extension of many `andi`-like instructions, I decided to do some better testing. 

 Official document on RISC-V GitHub like https://github.com/riscv/riscv-tests is quite hard to use and I failed to run them myself. But fortunately, I found [PicoRV32](https://github.com/cliffordwolf/picorv32), on Claire Wolf's old GitHub account, has a ready-to-use test based on riscv-tests. 

The idea is simple, just build a binary from test instruction source codes and run it on hardware or in simulator. The test files are mostly formed by macros that I can't (and don't need) to understand, when something failed or passed, information is printed. So what I need to do is just tell the testing part how to print characters, and of course like normal "ported" programs, specify things like stack location, start address, so on.  

The `tests/riscv_test.h`, aka macro `RVTEST_CODE_BEGIN`, `RVTEST_PASS` and `RVTEST_FAIL` should be customized. They just need to print the test's name, the 'ok' string, and the 'error' string. Quite easy, just the print character part need to be fixed. Just be careful, registers must be restored before the macro ends. Like I need to call other function, so `ra` must be saved. 

```asm
#define RVTEST_CODE_BEGIN		\
	.text;				\
	.global TEST_FUNC_NAME;		\
	.global TEST_FUNC_RET;		\
	.global uart_putchar;	\
TEST_FUNC_NAME:				\
	la sp, 0x200fffff;		\
	sw ra, (sp);			\
	lui	a3,%hi(.test_name);	\
	addi a3,a3,%lo(.test_name);	\
.prname_next:				\
	lb	a0,0(a3);		\
	beq	a0,zero,.prname_done;	\
	jal uart_putchar;	\
	addi	a3,a3,1;		\
	jal	zero,.prname_next;	\
.test_name:				\
	.ascii TEST_FUNC_TXT;		\
	.byte 0x00;			\
	.balign 4, 0;			\
.prname_done:				\
	addi	a0,zero,'.';		\
	jal uart_putchar;		\
	jal uart_putchar;		\
	lw ra, (sp);			\
```

```asm
#define RVTEST_PASS			\
	la sp, 0x200fffff;		\
	sw ra, (sp);			\
	addi a0, zero, 'o';		\
	jal uart_putchar;		\
	addi a0, zero, 'k';		\
	jal uart_putchar;		\
	addi a0, zero, '\r';	\
	jal uart_putchar;		\
	addi a0, zero, '\n';	\
	jal uart_putchar;		\
	lw ra, (sp);			\
	jal zero, TEST_FUNC_RET;

```

`uart_putchar` is my print function, and `0x200fffff` is my stack location. Actually stack really doesn't matter here. 

Then `start.S`, add all tests into it. Again it is copied and pruned from PicoRV32. 

```asm
// This is free and unencumbered software released into the public domain.
//
// Anyone is free to copy, modify, publish, use, compile, sell, or
// distribute this software, either in source code form or as a compiled
// binary, for any purpose, commercial or non-commercial, and by any
// means.
.section .text
.globl _start
.globl uart_putchar

_start:
	/* zero-initialize all registers */

	addi x1, zero, 0
	addi x2, zero, 0
	addi x3, zero, 0
	......
	addi x31, zero, 0
#	j _start

#  define TEST(n) \
	.global n; \
	addi x1, zero, 1000; \
	jal zero,n; \
	.global n ## _ret; \
	n ## _ret:

	#picorv32_timer_insn(zero, x1); \

	TEST(lui)
	TEST(auipc)
	TEST(j)
	......
	TEST(lw)
	TEST(lbu)
	TEST(lhu)


# void uart_putchar(char c)
uart_putchar:
	la t2, 0x93000000
1:
    lw t0, 8(t2)
    beq t0, zero, 1b
    sw a0, 0(t2) # do the real work
2:
    lw t0, 8(t2)
    beq t0, zero, 2b
	ret
```

Then just compile everything together, run objcopy on the ELF, and execute the binary on hardware. Seems every test file in the `riscv_tests` folder requires some specific macro definition, but I could just run `make firmware/firmware.elf` on PicoRV32, copy all the compile commands, and after a bit of regular expression they are modified to my need. And I have this Makefile: 

```makefile
RISCV_PATH=/opt/riscv32i/bin
TESTS_PATH=./riscv_tests
all: 
	$(RISCV_PATH)/riscv32-unknown-elf-gcc -c -march=rv32im  firmware/start.S -o firmware/start.o
	$(RISCV_PATH)/riscv32-unknown-elf-gcc -c -march=rv32im -o $(TESTS_PATH)/addi.o -DTEST_FUNC_NAME=addi \
		-DTEST_FUNC_TXT='"addi"' -DTEST_FUNC_RET=addi_ret $(TESTS_PATH)/addi.S
	$(RISCV_PATH)/riscv32-unknown-elf-gcc -c -march=rv32im -o $(TESTS_PATH)/add.o -DTEST_FUNC_NAME=add \
		-DTEST_FUNC_TXT='"add"' -DTEST_FUNC_RET=add_ret $(TESTS_PATH)/add.S
	$(RISCV_PATH)/riscv32-unknown-elf-gcc -c -march=rv32im -o $(TESTS_PATH)/andi.o -DTEST_FUNC_NAME=andi \
		-DTEST_FUNC_TXT='"andi"' -DTEST_FUNC_RET=andi_ret $(TESTS_PATH)/andi.S
	$(RISCV_PATH)/riscv32-unknown-elf-gcc -c -march=rv32im -o $(TESTS_PATH)/and.o -DTEST_FUNC_NAME=and \

...omitted... \

	$(RISCV_PATH)/riscv32-unknown-elf-gcc -Os -ffreestanding -nostdlib -o firmware/firmware.elf \
		-Wl,-Bstatic,-T,firmware/linker.ld \
		firmware/start.o $(TESTS_PATH)/*.o -lgcc
	$(RISCV_PATH)/riscv32-unknown-elf-objcopy -O binary firmware/firmware.elf firmware/firmware.bin

clean:
	-rm firmware/*.o firmware/*.elf $(TESTS_PATH)/*.o
```

Oh I didn't mention the `linker.ld`. If you are testing your core then you certainly already have available toolchains and linker scripts for your own core. For this small testing, the following would be enough (0x20000000 is where firmware is loaded into my memory): 

```
SECTIONS
{
    . = 0x20000000;
    .text : { *(.text) }
    .data : { *(.data) }
}
```

Let it run, and that's all. 

![rv32test](/MyBlog/images/rv32test.png)

It tells clearly some `andi`-like and division instruction failed while other passed(of course, if `jalr`-like instructions failed, program counter may go wild and nothing will be printed). Then have a look at the failed test assembly, you may comment out them one by one, run again and see which failed. 

This simple test program is far from things like formal verification, but it's enough to give me confidence to go on. Anyway, I don't have pipeline, and there is really not much hard-to-debug mistake to make. Till now, after passed all tests here, I haven't found anything wrong with my core itself yet. 

