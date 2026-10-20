# Лабораторная работа 2: SCR1

## Вариант 4
- **Исключение:** Breakpoint (тест `isa/rv32mi/sbreak.S`).
- **Reset Vector:** `0x800`.
- **Trap Vector:** `0x6c0`.

## Внесенные изменения
1. **`src/includes/scr1_arch_description.svh`**: 
   - `SCR1_ARCH_RST_VECTOR` = `'h800`
   - `SCR1_ARCH_MTVEC_BASE` = `'h6c0`
2. **`sim/tests/common/link.ld`**:
   - Секция `.text.init` (Trap Vector) размещена по адресу `0x6c0`.
   - Секция `.text.start` (Reset Vector) размещена по адресу `0x800`.
3. **`sim/tests/common/riscv_macros.h`**:
   - Удален сдвиг `.org 0xC0`.
   - Добавлена директива `.section .text.start` перед меткой `_start`.
   - Добавлен вывод строки `"Breakpoint detected!\n"` в обработчике исключений (запись по адресу UART `0x10000000`).
4. **`sim/tests/riscv_isa/rv32_tests.inc`**: оставлен только тест `sbreak.S`.
5. **Makefile**: удалены флаги `_zicsr_zifencei` для совместимости с кросс-компилятором GCC 10.2.0.

## Доказательство записи строки в память
Согласно дампу файла `sbreak.dump`, строка `msg_breakpoint` расположена по адресу `0x920`:
```text
00000920 <msg_breakpoint>:
 920:	7242                	flw	ft4,48(sp)   # байты 42 72 -> 'B' 'r'
 922:	6165                	addi	sp,sp,112   # байты 65 61 -> 'e' 'a'
