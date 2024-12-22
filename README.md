# Readme
Lox interpreter written entirely in Miniscript.

Since the performance of maps in miniscript is low, most objects are represented by list.

The main branch only implemented official lox standard.

To use extended features, such as list and map, check out the [extended](https://github.com/wh0wfg/LoxInMiniscript/tree/extended) branch.

For features based on greyscript extensions, check out the [greyscript-extended](https://github.com/wh0wfg/LoxInMiniscript.git) branch.

## **Benchmarks**

**Note**: Benchmark parameters differ from official tests.

### **Binary Trees**
| Parameter       | Value   |
|-----------------|---------|
| Min Depth       | 2       |
| Max Depth       | 4       |

**Results**:
- Stretch tree of depth 8: **-1**
- Long-lived tree of depth 7: **-1**
- Elapsed: **7.798s**

---

### **Equality**
| Parameter       | Value   |
|-----------------|---------|
| While Times     | 100     |

**Results**:
- Loop: **0.111s**
- Elapsed: **1.202s**
- Equals: **1.091s**

---

### **Fibonacci**
| Parameter       | Value   |
|-----------------|---------|
| Fibonacci Number| 15      |

**Results**:
- Elapsed: **0.596s**

---

### **Instantiation**
| Parameter       | Value   |
|-----------------|---------|
| While Times     | 100     |

**Results**:
- Elapsed: **0.388s**

---

### **Method Call**
| Parameter       | Value   |
|-----------------|---------|
| While Times     | 100     |

**Results**:
- Elapsed: **1.345s**

---

### **String Equality**
| Parameter       | Value   |
|-----------------|---------|
| While Times     | 100     |

**Results**:
- Elapsed: **0.589s**

---

### **Trees**
| Parameter       | Value   |
|-----------------|---------|
| Tree Depth      | 4       |
| No For Loop     | True    |

**Results**:
- Elapsed: **0.232s**

---

### **Zoo**
| Parameter       | Value   |
|-----------------|---------|
| While Times     | 100     |

**Results**:
- Elapsed: **0.024s**

---

### **Zoo Batch**
| Parameter       | Value   |
|-----------------|---------|
| While Limit     | 1       |
| For Times       | 100     |

**Results**:
- Batches: **4200**
- Elapsed: **1.069s**

---

## **System Specifications**
Benchmarks were run on the following machine:
- **CPU**: AMD Ryzen 9 5900HX
- **RAM**: 64 GiB
- **OS**: Windows 10
- **Miniscript Interpreter**: [MiniScript](https://github.com/JoeStrout/miniscript)

---

## **References**
- [Crafting Interpreters](https://craftinginterpreters.com/)
- [jlox](https://github.com/geertguldentops/jlox)
- [MiniScript](https://github.com/JoeStrout/miniscript)
