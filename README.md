Here’s the fully updated Computer Architecture Specification Document, incorporating the latest design decisions and refinements from our discussion:

---

# 📐 Computer Architecture Specification Document

**Version:** Draft 2

---

## 1. 🧠 Word and Bus Widths

* **Word size:** 16 bits
* **Data bus width:** 16 bits
* **Address bus width:** 16 bits
* **Max addressable memory:** 64 KB (2¹⁶)

---

## 2. 🧮 Register Set

| Name  | Width             | Purpose                                   |
| ----- | ----------------- | ----------------------------------------- |
| R0–R7 | 16-bit            | General-purpose registers                 |
| PC    | 16-bit            | Program Counter                           |
| SP    | 16-bit            | Stack Pointer                             |
| SR    | 8-bit             | Status Register (flags)                   |
| IR    | 16-bit (optional) | Instruction Register (used for debugging) |

* **Stack direction:** ☑ Down
* **Stack base address:** `0x7FFF`
* **Calling convention:** Caller-saved registers; return address is pushed onto stack

---

## 3. 🧾 Instruction Set Architecture (ISA)

### 3.1 Instruction Format

* **Instruction length:** ☑ Fixed 16-bit
* **Encoding format(s):**

  * `5-bit opcode + 3-bit reg1 + 3-bit reg2 + 5-bit unused/imm` for `reg, reg` operations
  * `5-bit opcode + 3-bit reg + 8-bit imm` for `reg, imm8` and `reg, port`
  * `5-bit opcode + 11-bit imm` for control flow instructions

### 3.2 Addressing Modes

* ☑ Immediate
* ☑ Direct
* ☑ Indirect (via register)
* ☑ Register

### 3.3 Instruction Categories

| Category      | Examples                          |
| ------------- | --------------------------------- |
| Data Movement | `MOV`, `MOVI`, `LD`, `ST`         |
| Arithmetic    | `ADD`, `SUB`, `CMP`               |
| Logic         | `AND`, `OR`, `XOR`, `SHL`, `SHR`  |
| Control Flow  | `JMP`, `CALL`, `RET`, `JZ`, `JNZ` |
| Stack         | `PUSH`, `POP`                     |
| I/O           | `IN`, `OUT`                       |
| Special       | `NOP`, `HALT`, `IRET`             |

---

## 🔢 Opcode Assignment Table

| Format         | Bits         |
| -------------- | ------------ |
| Opcode         | 5 bits       |
| Reg1 (if used) | 3 bits       |
| Reg2 (if used) | 3 bits       |
| Imm8 / imm11   | 8 or 11 bits |

### 📥 Data Movement

| Mnemonic        | Opcode | Pattern     | Description            |
| --------------- | ------ | ----------- | ---------------------- |
| `MOV R1, R2`    | `0x00` | `reg, reg`  | Copy R2 into R1        |
| `MOVI R1, imm8` | `0x01` | `reg, imm8` | Load immediate into R1 |
| `LD R1, R2`     | `0x02` | `reg, reg`  | R1 ← mem\[R2]          |
| `ST R1, R2`     | `0x03` | `reg, reg`  | mem\[R1] ← R2          |

### ➕ Arithmetic/Logic

| Mnemonic     | Opcode | Pattern    | Description         |
| ------------ | ------ | ---------- | ------------------- |
| `ADD R1, R2` | `0x04` | `reg, reg` | R1 ← R1 + R2        |
| `SUB R1, R2` | `0x05` | `reg, reg` | R1 ← R1 - R2        |
| `CMP R1, R2` | `0x06` | `reg, reg` | Set flags for R1-R2 |
| `AND R1, R2` | `0x07` | `reg, reg` | Bitwise AND         |
| `OR R1, R2`  | `0x08` | `reg, reg` | Bitwise OR          |
| `XOR R1, R2` | `0x09` | `reg, reg` | Bitwise XOR         |
| `SHL R1`     | `0x0A` | `reg`      | Logical shift left  |
| `SHR R1`     | `0x0B` | `reg`      | Logical shift right |

### 🔁 Control Flow

| Mnemonic    | Opcode | Pattern | Description            |
| ----------- | ------ | ------- | ---------------------- |
| `JMP addr`  | `0x0C` | `imm11` | Unconditional jump     |
| `JZ addr`   | `0x0D` | `imm11` | Jump if zero           |
| `JNZ addr`  | `0x0E` | `imm11` | Jump if not zero       |
| `CALL addr` | `0x0F` | `imm11` | Call subroutine        |
| `RET`       | `0x10` | —       | Return from subroutine |

### 📦 Stack

| Mnemonic  | Opcode | Pattern | Description      |
| --------- | ------ | ------- | ---------------- |
| `PUSH R1` | `0x11` | `reg`   | Push R1 to stack |
| `POP R1`  | `0x12` | `reg`   | Pop from stack   |

### 🌐 I/O

| Mnemonic       | Opcode | Pattern     | Description    |
| -------------- | ------ | ----------- | -------------- |
| `IN R1, port`  | `0x13` | `reg, imm8` | Read from port |
| `OUT port, R1` | `0x14` | `imm8, reg` | Write to port  |

### 🛑 Special

| Mnemonic | Opcode | Pattern | Description    |
| -------- | ------ | ------- | -------------- |
| `HALT`   | `0x1F` | —       | Stop execution |

---

## 4. 🗺️ Memory Map

| Address Range   | Size  | Description            |
| --------------- | ----- | ---------------------- |
| `0x0000–0x7FFF` | 32 KB | RAM                    |
| `0x8000–0xBFFF` | 16 KB | ROM                    |
| `0xC000–0xC0FF` | 256 B | I/O Registers          |
| `0xC100–0xC1FF` | 256 B | Interrupt Vector Table |
| `0xC200–0xFFFF` | 14 KB | Reserved / Future use  |

---

## 5. 🧯 Status Register (Flags)

| Flag | Bit | Meaning               |
| ---- | --- | --------------------- |
| Z    | 0   | Result was zero       |
| N    | 1   | Result was negative   |
| C    | 2   | Carry/borrow occurred |
| V    | 3   | Signed overflow       |
| I    | 4   | Enable interrupts     |

---

## 6. 🔁 Control Flow

* **Jump types:** `JMP`, `JZ`, `JNZ`, `CALL`, `RET`
* **Call behavior:** Push return address to stack
* **Return behavior:** Pop return address from stack
* **Interrupt return:** `IRET` (restores PC and flags)

---

## 7. 🧯 Interrupt System

* **Interrupt types:**

  * ☑ Maskable
  * ☑ Non-maskable (NMI)
* **Interrupt Vector Table:**

  * Location: `0xC100–0xC1FF`
  * Format: 16-bit addresses per interrupt
* **Trigger types:** ☑ Edge / ☑ Level (configurable)
* **Context saved on interrupt:**

  * ☑ PC
  * ☑ Flags
  * ☐ General registers (optional via software)

---

## 8. 🔌 I/O Interface

* **Method:** ☑ Memory-mapped
* **Address range:** `0xC000–0xC0FF`
* **Standard Devices:**

| Device   | Address  | Notes                              |
| -------- | -------- | ---------------------------------- |
| Keyboard | `0xC000` | Read-ready bit + data register     |
| SSD      | `0xC010` | Command/data interface             |
| Display  | `0xC020` | Optional text buffer or pixel port |
| Serial   | `0xC030` | Optional RS232-style UART          |

---

## 9. 🕹️ Boot & Reset Behavior

* **Reset vector address:** `0x8000`
* **Boot sequence:**

  * ☑ Jump to reset vector
  * ☑ Clear registers
  * ☑ Enable interrupts

---

## 10. 🧱 Physical Implementation Notes

* **Technology:** CMOS (4000 series logic or custom gates)
* **Clock frequency:** \~1 MHz (initial target)
* **Clock generation:** Crystal oscillator or RC timer
* **Reset circuit:** Push-button + capacitor + Schmitt trigger inverter

---

Let me know if you'd like a PDF or printable version next!
