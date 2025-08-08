# WEEK-2:
#### TASK-2:Proving our local riscv setup (Run,assembly ,disassembly)

## 🛠️ PREREQUISITES:
#### 1️⃣ RISCV-TOOLCHAIN :INSTALL ✅ (check riscv-task1-Nishanth )
## STEPS : 
### 1️⃣Create a unique.h 
#### What it does ❓:
It prints who, when, where, and how the program was compiled.

Used to prove you ran the program on your own RISC-V setup.
#### What it shows
1. 👤 Username
2. 🖥️ Hostname
3. 🧬 Machine ID
4. 🕒 Build time (UTC + Epoch)
5. 🛠️ GCC version
6. 📦 Program name
7. 🔐 ProofID & RunID (unique numbers)
~~~ c
#ifndef UNIQUE_H
#define UNIQUE_H

#include <stdio.h>
#include <stdint.h>

// Declare these as extern if passed via -D flag in GCC
#ifndef USERNAME
#define USERNAME "unknown"
#endif

#ifndef HOSTNAME
#define HOSTNAME "unknown"
#endif

#ifndef MACHINE_ID
#define MACHINE_ID "unknown"
#endif

#ifndef BUILD_UTC
#define BUILD_UTC "unknown"
#endif

#ifndef BUILD_EPOCH
#define BUILD_EPOCH 0
#endif

static inline void uniq_print_header(const char *program_name, uint64_t proof, uint64_t runid) {
    printf("User    : %s\n", USERNAME);
    printf("Host    : %s\n", HOSTNAME);
    printf("Machine : %s\n", MACHINE_ID);
    printf("UTC     : %s\n", BUILD_UTC);
    printf("Epoch   : %lu\n", (unsigned long)BUILD_EPOCH);
    printf("GCC     : %s\n", __VERSION__);
    printf("Program : %s\n", program_name);
    printf("ProofID : 0x%016llx\n", (unsigned long long)proof);
    printf("RunID   : 0x%016llx\n", (unsigned long long)runid);
}


#endif
~~~
### 📌 Why it's used:
To verify your local build in labs like VSD / RISC-V tasks.

No cheating. No copying. Just your own setup 💯

### 2️⃣ Before starting with the compiling our programmes
 