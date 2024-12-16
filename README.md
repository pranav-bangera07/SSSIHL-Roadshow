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
Now, we change the directory is changed to openplane flow directory. OpenLane is typically set up in a working directory that contains all the necessary files for physical design flows.
```
cd Desktop/work/tools/openlane_working_dir/openlane
```
Then we open Docker. OpenLane runs inside a Docker container to ensure a consistent environment across different systems.
```
docker
```
Then we launch the launch the interactive mode of the OpenLane toolchain.he interactive mode is particularly useful for debugging specific steps in the flow, fine-tuning parameters for individual stages, exploring the design at intermediate stages of the process.
![6](https://github.com/user-attachments/assets/6b6b7457-b768-4379-b050-46715abe7a07)
Then OpenLane is packaged as a Tcl module.
```
package require openlane 0.9

```
Next we prepare the PicoRV32 design for further processing in an FPGA or ASIC workflow.
```
prep -design picorv32a
```
![7](https://github.com/user-attachments/assets/53dad896-8ea8-4cd2-9ce3-f906dd4fcbae)
By the following command we synthesized netlist that represents the design in terms of basic digital components such as logic gates, flip-flops, multiplexers, etc.
```
run_synthesis
```
![8](https://github.com/user-attachments/assets/dcc2a1dd-6bff-46a6-be31-08f971cc1f25)
Floorplanning is a crucial step in the physical design flow, where the logical components are assigned specific locations on the chip or FPGA fabric. The next command initiates floorplanning.
```
run_floorplan
```
![9](https://github.com/user-attachments/assets/8aafce90-cbb3-4cd1-b025-bda0cacdac27)

Next we see our Floorplan
```
eog designs/picorv32a/runs/13-12_07-00/results/floorplan/picorva32a.floorplan.def.png
```
![10](https://github.com/user-attachments/assets/62ad4be2-ceff-4688-808f-480845478138)

Next we place our chips.
```
run_placement
```
We can also have a visual of our placement by the following commannd.
```
eog designs/picorv32a/runs/13-12_07-00/results/placement/picorva32a.placement.def.png
```
![11](https://github.com/user-attachments/assets/85b12618-89d7-48ef-8953-d5615b95caf3)

Next we do the Clock Tree Synthesis to reduce the delay and skew.
```
run_cts
```
![12](https://github.com/user-attachments/assets/674cab43-47ed-4c57-8b56-3095e20ecc5d)

Next we do the Routing where it establishes the I/O connecttions at the chip boundary pads.
```
run_routing
```
![13](https://github.com/user-attachments/assets/7acae4cb-e6df-4573-b445-8641e99bef35)

Our chip is ready!!!

We can connect the RISC-V board to our systems and do basic LED fading by the board.
```
#include "debug.h"
#define TIME_PERIOD 1000
#define PRESC       0
#define PULSE       632
#define STEP_SIZE   10
volatile u16 val;
volatile u8 dir;
void TIM1_PWMOut_Init(void){
    GPIO_InitTypeDef GPIO_InitStructure={0};
    TIM_OCInitTypeDef TIM_OCInitStructure={0};
    TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure={0};
    //RCC_APB2PeriphClockCmd( RCC_APB2Periph_GPIOC | RCC_APB2Periph_TIM1, ENABLE );
    RCC_APB2PeriphClockCmd( RCC_APB2Periph_GPIOD | RCC_APB2Periph_AFIO, ENABLE );
    RCC_APB1PeriphClockCmd( RCC_APB1Periph_TIM2, ENABLE );
    GPIO_PinRemapConfig(GPIO_FullRemap_TIM2, ENABLE);
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init( GPIOD, &GPIO_InitStructure);
    TIM_TimeBaseInitStructure.TIM_Period = TIME_PERIOD;
    TIM_TimeBaseInitStructure.TIM_Prescaler = PRESC;
    TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;
    TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up;
    TIM_TimeBaseInit( TIM2, &TIM_TimeBaseInitStructure);
    TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM2;
    TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;
    TIM_OCInitStructure.TIM_Pulse = PULSE;
    TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;
    TIM_OC3Init( TIM2, &TIM_OCInitStructure );
    TIM_CtrlPWMOutputs(TIM2, ENABLE );
    //TIM_OC3PreloadConfig( TIM1, TIM_OCPreload_Disable );
    //TIM_ARRPreloadConfig( TIM1, ENABLE );
    TIM_Cmd( TIM2, ENABLE );
}
int main(void)
{
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
    SystemCoreClockUpdate();
    Delay_Init();
    TIM1_PWMOut_Init();
    val = 0;
    dir = 0;
    // Loop
    while(1){
        val += (dir) ? -STEP_SIZE : STEP_SIZE;
        TIM_SetCompare3(TIM2, val);
        dir ^= (val == 1000 || val == 0) ? 1 : 0;
        Delay_Ms(15);
    }
}

```
https://github.com/user-attachments/assets/3d53a106-b7ec-4d01-911a-b4afb5494f08
