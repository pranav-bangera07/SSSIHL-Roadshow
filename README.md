# SSSIHL-Roadshow
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
