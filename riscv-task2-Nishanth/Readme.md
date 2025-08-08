# WEEK-2: 
## TASK-2:Proving our local riscv setup (Run,assembly ,disassembly)
### 🎯Objectives:
Run 3–4 RISC-V C programs on your own PC, show their assembly output, manually decode a few instructions, and make sure the output proves it was run on your unique machine using your username, hostname, machine ID, and time.

# 🛠️ PREREQUISITES:
### 1️⃣ RISCV-TOOLCHAIN :INSTALL ✅ (check riscv-task1-Nishanth )
# STEPS : 
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
#### 📌 Why it's used:
To verify your local build in labs like VSD / RISC-V tasks.

No cheating. No copying. Just your own setup 💯

#### ✏️ Before starting with the compilation of our programmes:
Run the following : 

~~~ bash 
spike --version
riscv64-unknown-elf-gcc -v
~~~

 #### Note : 
 While running `spike --version ` you get an output as 
 ```  bash
 spike: unrecognized option --version
Try 'spike --help' for more information.
```

because the spike which we use doesn't include the spike --version command in the source code so try running

~~~ bash 
spike --help
~~~

the first line of the output shows the version of the spike,that has been installed.

~~~ bash
monkey@HP-Victus:~/riscv_toolchain$ spike --help
Spike RISC-V ISA Simulator 1.1.1-dev
~~~

# 📷OUTPUT:
![Spike Output Screenshot](photos/riscv_output.png)


### 2️⃣Uniqueness mechanism :(Run before compiling program)
Before compiling, set the following identity variables in your Linux shell to make each build specific to your user and machine:
~~~  bash
export U=$(id -un)                        # Username
export H=$(hostname -s)                  # Hostname
export M=$(cat /etc/machine-id | head -c 16)  # Machine ID (first 16 chars)
export T=$(date -u +%Y-%m-%dT%H:%M:%SZ)  # UTC Timestamp (ISO 8601)
export E=$(date +%s)                     # Epoch Timestamp
~~~
### 3️⃣PROGRAMS: Crate the following programs : 

### `❗Note: The following code /program prints system specific details and does the intended function .`

#### A : Factorial:
~~~ c
#include "unique.h"
static unsigned long long fact(unsigned n){ return (n<2)?1ULL:n*fact(n-1); }
int main(void)
{
uniq_print_header("factorial");
unsigned n = 12;
printf("n=%u, n!=%llu\n", n, fact(n));
return 0;
}
~~~
Prints the factorial of 12 using recursion. 

Expected output : `n=12, n!=479001600`

#### B : Max_array:
``` c
#include "unique.h"
int main(void)
{
uniq_print_header("max_array");
int a[] = {42,-7,19,88,3,88,5,-100,37};
int n = sizeof(a)/sizeof(a[0]), max=a[0];
for(int i=1;i<n;i++) if(a[i]>max) max=a[i];
printf("Array length=%d, Max=%d\n", n, max);
return 0;
}
```
Displays the maximum value in an array of 9 integers.

Expected Output : `Array length=9, Max=88`
 
#### C : Bitops:
``` c
#include "unique.h"
int main(void)
{
uniq_print_header("bitops");
unsigned x=0xA5A5A5A5u, y=0x0F0F1234u;
printf("x&y=0x%08X\n", x&y);
printf("x|y=0x%08X\n", x|y);
printf("x^y=0x%08X\n", x^y);
printf("x<<3=0x%08X\n", x<<3);
printf("y>>2=0x%08X\n", y>>2);
return 0;
}
```
Displays results of bitwise operations like AND, OR, XOR, left shift, and right shift on two hex values.
 
#### D :Bubble_sort:

``` c
#include "unique.h"
void bubble(int *a,int n)
{ for(int i=0;i<n-1;i++) 
    for(int j=0;j<n-1-i;j++) 
    if(a[j]>a[j+1])
    {
        int t=a[j];a[j]=a[j+1];a[j+1]=t;
    } 
}
int main(void)
{
uniq_print_header("bubble_sort");
int a[]={9,4,1,7,3,8,2,6,5}, n=sizeof(a)/sizeof(a[0]);
bubble(a,n);
printf("Sorted:"); for(int i=0;i<n;i++) printf(" %d",a[i]); puts("");
return 0;
}
```
Sorts the array {9,4,1,7,3,8,2,6,5} using Bubble Sort and prints the sorted array.

Output : 1 2 3 4 5 6 7 8 9 

### Now after creating all the files compile the programs using the prebuilt spike and the toolchain 
### FOR factorial.c 
### 1. Compile the code : 

Using the prebuilt toolchain compile the code:
 
 ``` bash
 riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
factorial.c -o factorial
```

Compiles the C program factorial.c into a RISC-V executable (factorial) with identity variables embedded for user, host, machine ID, and build timestamp.

### 2. Run the code using spike : 
 Run the code using previously installed spike (ISA-simulator)

``` bash
spike pk ./factorial
```
Runs the compiled RISC-V program factorial using the Spike ISA simulator and the Proxy Kernel (pk).

## 📸OUTPUT:
![spike output](factorial_output.png)

### 3. Prodcue assembly and disassembly 

Generates human-readable RISC-V assembly (.s file) and extracts only the disassembly of the main function to analyze and decode the actual machine instructions executed.
 
~~~  bash

riscv64-unknown-elf-gcc -O0 -S factorial.c -o factorial.s #produces assembly file(.s)
riscv64-unknown-elf-objdump -d ./factorial | sed -n '/<main>:/,/^$/p' | tee factorial_main_objdump.txt #contains only the main function 

~~~
✅ What is factorial_main_objdump.txt:
It is a text file containing the disassembled RISC-V machine code only for the main function of the compiled factorial program, used for manual instruction decoding and analysis.

## OUTPUT:
 
 This is what we get after disassemblying the function : 
 ![assemblyop](factorial_main_asm.png)

 That is the final output for the factorial.c file with assembly (only the main section and the op ).

 #### Now follow the same for the rest of the program : 
   1️⃣ Compile the code,

   2️⃣ Run the code,
   
   3️⃣ Produce the assembly code,
   
   4️⃣ Extract the disassembly of the main function for analysis .

# Bitops function output:
 
### 1. 📷 Ouput: 
 ![assemblyop](bitops_output.png)

### 2. ⚙️ Assembly code  and the disassembled main section:
 ![assemblyop](bitops_main_asm-1.png)
 ![assemblyop](bitops_main_asm_2.png)

 # Max_array Function Output : 

 ### 1. 📷 Output:
 
 ![assemblyop](max_array_output.png)
  
### 2. ⚙️ Assembly code and the disassembled main section : 

 ![assemblyop](max_array_main_asm-1.png)

  ![assemblyop](max_array_main_asm-2.png)

# Bubble sort function
  
### 1. 📷 Ouput:

 ![assemblyop](bubble_sort_output.png)

 ### 2. ⚙️ Assembly code and the disassembled main section : 

 ![assemblyop](bubble_sort_main_asm.png)

 
















