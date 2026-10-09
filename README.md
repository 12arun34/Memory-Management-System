# Memory Management System with Paging and Virtual Memory

A comprehensive C++ implementation of a memory management system that simulates paging, virtual memory, and process management using the Least Recently Used (LRU) page replacement algorithm. This is first change by me(tanu).This is merge related changes.

this is arun lazy work.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Data Structures](#data-structures)
- [Commands](#commands)
- [Installation](#installation)
- [Usage](#usage)
- [Input Format](#input-format)
- [Output](#output)
- [Algorithm Details](#algorithm-details)
- [Example](#example)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This project implements a complete memory management system that handles:
- **Paging**: Divides memory into fixed-size pages for efficient allocation
- **Virtual Memory**: Extends available memory using secondary storage simulation
- **Process Management**: Loads, executes, and manages multiple processes
- **Page Replacement**: Uses LRU algorithm for optimal memory utilization
- **Memory Protection**: Validates memory addresses and prevents invalid access

## ✨ Features

- **Multi-process Support**: Handle multiple processes simultaneously
- **Dynamic Memory Allocation**: Allocate and deallocate memory as needed
- **Page Table Management**: Maintain logical-to-physical address mappings
- **LRU Page Replacement**: Efficient swapping between main and virtual memory
- **Command-line Interface**: Easy-to-use command system
- **Error Handling**: Comprehensive error checking and reporting
- **Memory Validation**: Prevents access to invalid memory addresses

## 🏗️ System Architecture

```
┌─────────────────┐    ┌─────────────────┐
│   Main Memory   │    │ Virtual Memory  │
│   (32 KB)       │◄──►│   (32 KB)       │
│                 │    │                 │
│ ┌─────────────┐ │    │ ┌─────────────┐ │
│ │   Page 0    │ │    │ │   Page 0    │ │
│ │   Page 1    │ │    │ │   Page 1    │ │
│ │     ...     │ │    │ │     ...     │ │
│ └─────────────┘ │    │ └─────────────┘ │
└─────────────────┘    └─────────────────┘
         ▲                       ▲
         │                       │
    ┌─────────┐            ┌─────────┐
    │Process 1│            │Process 2│
    │Process 3│            │Process 4│
    └─────────┘            └─────────┘
```

## 📊 Data Structures

### Process Structure
```cpp
struct Process {
    int pid;                    // Process ID
    string processFilename;     // Source file name
    int size;                   // Size in KB
    vector<PageTableEntry> pageTable;  // Page table
    bool inMainMemory;          // Memory location flag
};
```

### Memory Page Structure
```cpp
struct MemoryPage {
    int pid;                    // Process ID
    int pageId;                 // Page ID
    vector<int> data;           // Page data
    time_t lastAccessTime;      // For LRU algorithm
};
```

### Page Table Entry
```cpp
struct PageTableEntry {
    int logicalPageNumber;      // Logical page number
    int physicalPageNumber;     // Physical page number
};
```

## 🎮 Commands

| Command | Description | Syntax |
|---------|-------------|--------|
| `load` | Load executable files into memory | `load <filename1> <filename2> ...` |
| `run` | Execute a process by PID | `run <pid>` |
| `kill` | Terminate a process and release memory | `kill <pid>` |
| `listpr` | List all processes in memory | `listpr` |
| `pte` | Print page table entries for a process | `pte <pid> <output_file>` |
| `pteall` | Print all page table entries | `pteall <output_file>` |
| `swapout` | Swap process to virtual memory | `swapout <pid>` |
| `swapin` | Swap process to main memory | `swapin <pid>` |
| `print` | Print memory values | `print <memloc> <length>` |
| `exit` | Exit the system | `exit` |

## 🚀 Installation

### Prerequisites
- C++ compiler (g++ recommended)
- Linux/Unix environment
- Make utility (optional)

### Compilation
```bash
# Compile the program
g++ -o memory_manager main.cpp

# Or with optimization flags
g++ -O2 -o memory_manager main.cpp
```

## 💻 Usage

### Basic Usage
```bash
./memory_manager <main_memory_size> <virtual_memory_size> <page_size> <input_file>
```

### Parameters
- `main_memory_size`: Size of main memory in KB (default: 32)
- `virtual_memory_size`: Size of virtual memory in KB (default: 32)
- `page_size`: Size of each page in bytes (default: 512)
- `input_file`: File containing commands to execute

### Example
```bash
./memory_manager 32 32 512 commands.txt
```

## 📝 Input Format

### Executable File Format
```
<size_in_KB>
<data_value_1> <data_value_2> ... <data_value_N>
```

### Command File Format
```
load process1.txt process2.txt
run 1
add 0 1 2
sub 3 4 5
print 0 10
kill 1
exit
```

### Process Commands (within executable files)
- `add <addr1> <addr2> <addr3>`: Add values at addr1 and addr2, store in addr3
- `sub <addr1> <addr2> <addr3>`: Subtract addr2 from addr1, store in addr3
- `print <addr>`: Print value at memory address
- `load <value> <addr>`: Load value into memory address

## 📤 Output

The system provides detailed output including:
- Process loading status
- Memory allocation information
- Command execution results
- Error messages for invalid operations
- Page table information
- Memory access results

### Sample Output
```
Process 1 is loaded in main memory
Command: add
Result: Value in addr 0 = 10, addr 1 = 20, addr 2 = 30
Process 1 swapped out page 0 from main memory
```

## 🔄 Algorithm Details

### LRU Page Replacement
The system uses the Least Recently Used algorithm for page replacement:

1. **Track Access Time**: Each page maintains a timestamp of last access
2. **Find LRU Page**: When memory is full, find the page with oldest access time
3. **Swap Operation**: Move LRU page to virtual memory, bring required page to main memory
4. **Update Page Tables**: Modify logical-to-physical mappings accordingly

### Memory Allocation Strategy
1. **First Fit**: Allocate pages in first available slot
2. **Page Division**: Split processes into fixed-size pages
3. **Overflow Handling**: Move excess pages to virtual memory
4. **Dynamic Swapping**: Swap pages as needed during execution

## 📋 Example

### Input File (commands.txt)
```
load process1.txt process2.txt
run 1
listpr
pte 1 page_table.txt
swapout 1
swapin 1
kill 1
exit
```

### Executable File (process1.txt)
```
2
10 20 30 40
add 0 1 2
sub 3 2 1
print 0
```

### Execution
```bash
./memory_manager 32 32 512 commands.txt
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow C++ coding standards
- Add comments for complex algorithms
- Test with various memory configurations
- Ensure proper error handling

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Arun Kumar**
- College Project - Memory Management System
- Implementation of paging and virtual memory concepts

## 📚 References

- Operating System Concepts by Silberschatz, Galvin, and Gagne
- Computer Organization and Design by Patterson and Hennessy
- Modern Operating Systems by Tanenbaum

---

**Note**: This is an educational project demonstrating memory management concepts. For production use, consider additional security measures and optimization techniques.
