# SSSIHL-Roadshow
RISC-V and VLSI Chip Design Roadshow was a workshop, conducted by Kunal Ghosh (Co-founder, VSD Corp. Pvt. Ltd.) along with his interns Nahusha and Vignesh, where we went through the process of designing a chip by translating High level logic descriptions into a physical chip design.

Here by using vsdsquadron.vdi file, we launched an Ubuntu environment with the help of Oracle VirtaulBox software. Ubuntu is commonly used for VLSI chip design because it is a Linux distribution that offers excellent compatibility with most Electronic Design Automation (EDA) tools used in the industry, like Cadence and Synopsys.
![1](https://github.com/user-attachments/assets/c86b4623-4c7e-4f95-a30e-68c06d1cbafd)
After Ubuntu was launched, the terminal was opened and gedit was installed.
```
sudo apt install gedit
```
Then a gedit editor was opened and an example code of summing the numbers from 1 to a user given number is coded and is saved as a .c file.
```
#include<stdio.h>  
int main(){  
    int sum = 0, i, n;  
    printf("Enter the value of n = ");  
    scanf("%d",&n);  
    for(i = 1;i <= n;i++){  
       sum = sum + i;  
    }  
    printf("The Sum of numbers from 1 to %d is %d\n",n,sum);  
    return 0;  
}
```
![2](https://github.com/user-attachments/assets/f096833a-5b47-41fd-8785-7284cdfeb30d)
Then the program was executed by the using the following command in terminal,
```
./a.out
```
, and the program was executed.
![3](https://github.com/user-attachments/assets/7c615072-fe36-4f12-8d25-7d0aeff31f09)

Then the following code is executed in the terminal which compiles a C program for a RISC-V 64-bit processor that adheres to the RV64I instruction set and uses the LP64 ABI. The result is an object file (world.o), which can later be linked to create an executable or used in further compilation steps.
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o world.o world.c
```
![4](https://github.com/user-attachments/assets/455e4f6d-5d2e-45d7-91f8-05ac34deff73)
and the next commmand disassembles the object file world.o to produce human-readable assembly code
```
riscv64-unknown-elf-objdump -d world.o
```
![5](https://github.com/user-attachments/assets/0e08eb2a-3a80-4d2a-b0b6-77d99a3a7674)
Now, we change the directory is changed to openplane flow directory.
```
cd Desktop/work/tools/openlane_working_dir/openlane
```



#include<stdio.h> int main(){ int sum = 0, i, n; printf("Enter the value of n = "); scanf("%d",&n); for(i = 1;i <= n;i++){ sum = sum + i; } printf("The Sum of numbers from 1 to %d is %d\n",n,sum); return 0; }

./a.out

riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o hello.o hello.c

riscv64-unknown-elf-objdump -d hello.o
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

docker

./flow.tcl -interactive

package require openlane 0.9

run_placement

prep -design picorv32a

run_synthesis

run_floorplan

eog designs/picorv32a/runs/13-12_07-00/results/floorplan/picorva32a.floorplan.def.png

run_placement

CLR C 

eog designs/picorv32a/runs/13-12_07-00/results/placement/picorva32a.placement.def.png

run_cts

run_routing
