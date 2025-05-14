# 📐 Computer Architecture Specification Document  
**Version:** Draft 1  

---

## 1. 🧠 Word and Bus Widths
- **Word size:** 16 bits  
- **Data bus width:** 16 bits  
- **Address bus width:** 16 bits  
- **Max addressable memory:** 64 KB (2¹⁶)

---

## 2. 🧮 Register Set
| Name | Width | Purpose |
|------|-------|---------|
| R0–R7 | 16-bit | General-purpose registers |
| PC | 16-bit | Program Counter |
| SP | 16-bit | Stack Pointer |
| SR | 8-bit or 16-bit | Status Register (flags) |
| IR | 16-bit (optional) | Instruction Register |

- **Stack direction:** ☐ Up / ☑ Down  
- **Stack base address:** `0x[____]`  
- **Calling convention:** (e.g., caller-saved, callee-saved?)

---

## 3. 🧾 Instruction Set Architecture (ISA)

### 3.1 Instruction Format
- **Instruction length:** ☐ Variable / ☑ Fixed 16-bit  
- **Encoding format(s):**  


### 3.2 Addressing Modes
- ☐ Immediate  
- ☐ Direct  
- ☐ Indirect  
- ☐ Register  
- ☐ Base + Offset / Indexed (optional)

### 3.3 Instruction Categories
| Category | Examples |
|----------|----------|
| Data Movement | `MOV`, `LOAD`, `STORE` |
| Arithmetic | `ADD`, `SUB`, `INC`, `DEC` |
| Logic | `AND`, `OR`, `XOR`, `NOT` |
| Control Flow | `JMP`, `CALL`, `RET`, `JZ`, `JNZ` |
| Stack | `PUSH`, `POP` |
| I/O | `IN`, `OUT` |
| Special | `NOP`, `HALT`, `IRET` |

---

## 4. 🗺️ Memory Map
| Address Range | Size | Description |
|---------------|------|-------------|
| `0x0000–0x7FFF` | 32 KB | RAM |
| `0x8000–0xBFFF` | 16 KB | ROM |
| `0xC000–0xC0FF` | 256 B | I/O Registers |
| `0xC100–0xC1FF` | 256 B | Interrupt Vector Table |
| `0xC200–0xFFFF` | 14 KB | Reserved / Future use |

---

## 5. 🧯 Status Register (Flags)
| Flag | Bit | Meaning |
|------|-----|---------|
| Z (Zero) | 0 | Result was zero |
| N (Negative) | 1 | Result was negative |
| C (Carry) | 2 | Carry/borrow occurred |
| V (Overflow) | 3 | Signed overflow |
| I (Interrupt Enable) | 4 | Enable maskable interrupts |

---

## 6. 🔁 Control Flow
- **Jump types:** `JMP`, `JZ`, `JNZ`, `JC`, `JNC`, etc.  
- **Call behavior:** Push return address to stack  
- **Return behavior:** Pop return address from stack  
- **Interrupt return:** `IRET` (restores PC and flags)

---

## 7. 🧯 Interrupt System
- **Interrupt types:**  
- ☐ Maskable  
- ☐ Non-maskable (NMI)  
- **Interrupt Vector Table:**  
- Location: `0xC100–0xC1FF`  
- Format: 16-bit addresses per interrupt
- **Trigger types:** ☐ Edge / ☐ Level  
- **Context saved on interrupt:**  
- ☐ PC  
- ☐ Flags  
- ☐ General registers?

---

## 8. 🔌 I/O Interface
- **Method:** ☐ Memory-mapped / ☐ Port-mapped  
- **Address range:** `0xC000–0xC0FF`  
- **Standard Devices:**
| Device | Address | Notes |
|--------|---------|-------|
| Keyboard | `0xC000` | Read-ready bit + data register |
| SSD | `0xC010` | Command/data interface |
| Display | `0xC020` | Optional |
| Serial | `0xC030` | Optional |

---

## 9. 🕹️ Boot & Reset Behavior
- **Reset vector address:** `0x8000`  
- **Boot sequence:**
- ☐ Jump to reset vector
- ☐ Clear registers
- ☐ Enable interrupts?

---

## 10. 🧱 Physical Implementation Notes
- **Technology:** CMOS (4000 series or custom logic)  
- **Clock frequency:** ~1 MHz (initial)  
- **Clock generation:** Crystal oscillator or timer circuit  
- **Reset circuit:** Push-button + capacitor + Schmitt trigger

---
