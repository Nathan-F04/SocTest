---
Layout: Home
Title: FPGA VGA Driver Project
Tags: FPGA VGA Verilog
Categories: Demo
---

Welcome to my System on Chip (Soc) Video Graphics Array (Vga) project. My name is Nathan Ferry and here, I catalog the timeline of the cumulative work that lead to my final code.

## **Project Summary**
### **Tools used**
The goal of my project was to adapt template code to develop an FPGA VGA driver to display unique designs on a 640x480 display. Fristy, there is no point going any further without explaining the project set up and components. An FPGA is a Field Programmable Gate Array which is an integrated circuit (IC) comprising of logical blocks that can be reprogrammed [1]. This is a useful competitor to ASICs which are Applications-species integrated circuits. These ICs are customised and have only one use in contrast to FPGAs which is why ASICS aren't reprogrammable [2]. A video graphics array "is an analog interface standard for computer video output" [3]. This means the VGA sends separate signals such as red, green and blue for the color displayed on for example a computer monitor along with signals such as vertical and horizontal signal sync [3]. Grouping these key terms we can see that the project was developed by testing new FPGA configurations which used the VGA to communicate with the monitor and display information. For the project intself, I used AMD Vivado Design Suite which can be used for "synthesis and analysis"[4] to, simulate, synthesise and generate a bitstream for my design and the design templates which I then uploaded to the board I used.  I used the Basys 3 Artix 7 FPGA Trainer Board [5] to upload my bitstream file to and used verilog to program.


## **Template VGA Design**
### **Project Set-Up**
Summarise the project set-up and design flow. Include a screenshot of your own set-up, for example see the image of my Project Summary window below. Guideline 1 short paragraph.

The project setup on hardware was simple, it simply involved connecting the Artix-7 board to a computer monitor via a VGA to VGA connector. The computer was connnected to the board for code upload via a USB-A to USB-C connection. The project software setup was implemented on AMD Vivado Design Suite. I began by creating a new project. I downloaded source code, which included a template for *VGAColorCycle.v* which was one of two template designs. Upon creating the new project I added into *Design sources* *VGASync.v*, *VGAColorCycle.v* and *VGATop.v* files (See Fig 1 below). I edited the clock wizard at the creation of the project to run at 25MHz. This was so that the screen would display without any flickering[6]. I added the *Basys3_Master.xdc* file to the *constraints* folder. In the *Simulation Sources* folder there was *Testbench.v* and *VGATop.v* which as in *Design Sources* contained the clock, *VGASync.v* and *VGAColorCycle.v* (See Fig 2 below). An image of the project summary can be seen below (See Fig 3 below).

Fig 1 - Project design sources and constraints files 
![20241202_153315](https://github.com/user-attachments/assets/cd1a9935-ea37-4a49-81bc-7d2b90a6f2d8)

Fig 2 - Project simulation sources fules 
![20241202_153318](https://github.com/user-attachments/assets/11e7bf16-7b93-4358-9410-29b94616b16d)

Fig 3 - Project summary
![image](https://github.com/user-attachments/assets/ae01aa6d-d668-4c10-a8e2-8007570e6973)

### **Template Code**
Outline the structure and design of the Verilog code templates you were given. What do they do? Include reference to how a VGA interface works. Guideline: 2/3 short paragraphs, consider including screenshot(s).

The templates we were given are comprised of several parts. In VGA Top there is the *clk wizip*, *VGA Sync*, *VGA ColorCylce* (or *VGA ColorStripes* depending on the template) and *Logic assaignment* (See below Fig 4). These logical blocks each have their own role to play along with the clock signal (*clk*), reset(*rst*), *h_sync* and *v_sync*.  The clock signal is used for synchronisation for example, some code is executed on a positive clock edge. *VGA Sync* outputs *h_sync* and *v_sync* which stand for vertical and horizontal sync which is used for measuring the time across the horizontal and vertical axes of the monitor. "The sync signals ensure the display knows when to start a new line or frame" [3]. *VGA ColorCylce* (or *VGA ColorStripes* depending on the template) and *Logic assaignment* are closely intwined as the first outputs the red green and blue signals which is joined with logic to display the colors on output. The signals *vga_red*, *vga_green* and *vga_blue* are the signals for the colors displayed. They are four bits long and together make a 12 bit number to generate a color with the rgb values. The code sets the pixel rgb value for a monitor in the range of 480x640. (See below Fig 5) 

Fig 4 - Architecture diagram
![20241130_120139](https://github.com/user-attachments/assets/593ef348-7df9-4f66-922e-d06a9e8fec59)

Fig 5 - Pixel grid
![20241130_122917](https://github.com/user-attachments/assets/e1599f77-4ed2-4deb-8e14-11d47ce46faf)

For this project we were given two templates to test. Initially we were given the *VGAColorCycle.v* file which operated using a state machine logic to transition through a series of colors. The statemachine used twelve bits and set the rgb values to generate the color desired (See below Fig 6). The code would use a time delay (more in depth on the variable and count) to show a color for a period and then switch to the next state or color.
The final result would be a loop of colors displayed on a monitor each for a fixed period of time.

Once the initial code was operational, we were given the VGAColorStripes.v file. This was a more complex file, using a series of if statements to check if the program was between a specific range of columns, before implementing a specific color for said range. This lead to a series of columns with differing colors depending on the range of the column pixels .i.e. blue for all pixels between pixel column 80 to 160. These templates would serve as the foundation for how I would create my own project.

Fig 6 - State machine
![20241130_122035](https://github.com/user-attachments/assets/aec0ec85-a29a-446b-ae2a-e0e4238cf312)

### **Simulation**
Explain the simulation process. Reference any important details, include a well-selected screenshot of the simulation. Guideline: 1/2 short paragraphs.

Once the project was set up, the simulation would be ran in order to see if everything worked as intended. It can also be used as a debugger which uses false signals to see how the design would react [7]. The signals won't be the actual ones expected but a scaled down version. Here I will analyse the signals generated from the *ColorStripes.v* code but the signals operate in the same way for both my final design and in the *ColorCycle.v* code. Analysing the image below for the color stripes code, (See below Fig 7) it can be seen that the signals explained earlier such as *clk*, *rst*, *h_sync* and *v_sync* are present. They behave as expected as the clock is made of periodic pulses and the reset is kept low after initialising high. The clock is keeping everything synchronised and the reset wasn't called. The horizontal signal is low more often than vertical sync as the the vertical sync is brought low once the entire screen is written but the horizontal is for  when the program makes it to the end of rows. The rows and columns are incremented and we can see row increasing from 0 to 5 before repeating the pattern which matches the *h_sync* signal as expected. The rows going from 0 to 5 and not the entire screen rows from 0 to 479 is an example of how the simulation scales down the actual design to save time as generating an accurate simulation would take too long. Columns are a green bar as they are incremented too fast to be seen in the image below since they are incremented while row is kept at a number while the entire span of columns is updated for said row ie row is 1 and column goes from 0 to 640. The color signals are seen next, they are either 0 or f and some overlap to generate different colors ie at one point there is three fs on top of each other to make the color white. They could be other letters or numbers depending on the colors needed to be generated .i.e. for a light orange the hexadecimal code would be e94. Since the hexadecimal values for the colors in color stripes seen were either full of red, green or blue or had none that is why only 0 or f is seen.

Fig 7 - Stipes simulation
![68ddb1b1-3a91-481f-b227-522b5b28966a](https://github.com/user-attachments/assets/68993b88-a2d1-47f4-a9f8-d260018ceafe)

### **Synthesis**
Describe the synthesis and implementation processes. Consider including 1/2 useful screenshot(s). Guideline: 1/2 short paragraphs.

Synthesis and implementation are used to first convert your code to a netlist (this is the synthesis), and then the placement and routing (this is the implementation). [8] Below you can see the synthesised design (See below Fig 8) for *VGAColorCycle.v* and the schematic showing the logic of the hardware (See below Fig 9). The schematic shows the same blocks from the template code section of this page, which is exactly what was predicted. The same logical blocks of the clock the VGA sync and the color cycle are seen.  Those logical blocks were explained earlier but the buffers at the end are representing each bit of red, green and blue output (as there are 4 bits for each color) and the vertical and horizontal sync have each a buffer of their own to represent those signals. There are also buffers for the clock and reset which feed into the logical blocks. The signals for the logical blocks can be seen in Fig 10.

The same amount of buffers and logical blocks are present in the schematic for the color stripes design (See below Fig 11) and the synthesis looks similar (See below Fig 12), but the logical block *u_vga_sync* outputs more signals due to the complexity of the program (See below Fig 13). 


Fig 8 - Color cycle synthesis design
![766468aa-b927-4d63-baca-624692be35c6](https://github.com/user-attachments/assets/00a965ec-c7a3-44ac-b5f2-2dfacc41fa93)

Fig 9 - Color cycle full schematic
![87cd4dfb-858f-415a-8a0c-6511d9eeacd5](https://github.com/user-attachments/assets/e34fc403-5fca-4747-8b68-e9ceca6848d2)

Fig 10 - Color cycle schematic enhanced on logical blocks 
![7f0c4be5-aee8-4783-9226-ef4c0420d341](https://github.com/user-attachments/assets/2555145e-f03a-4dcd-abe8-c42a7c65c144)

Fig 11 - Color stripes full schematic
![c67055c2-c507-4932-a613-ac40ea863b6a](https://github.com/user-attachments/assets/5c788e3d-b265-4a86-9519-d72f31ffa1c1)

Fig 12 - Color stripes synthesised design
![04d057e9-6cd2-4d71-b65c-e2b78c31e0dc](https://github.com/user-attachments/assets/e4cc833f-6306-4b46-838f-28d61b8bfa0c)

Fig 13 - Color stripes schematic enhanced on logical blocks
![0e486d52-c412-41cb-9792-f1b7a92a6fea](https://github.com/user-attachments/assets/58b82be9-3bcf-43f1-8b0f-1f378b3675a5)
### **Demonstration**
Perhaps add a picture of your demo. Guideline: 1/2 sentences.

The implementation of *VGAColorCycle* showed a different color after a delay using a statemachine (See below Fig 14). *VGAColorStripes* was a more complex file and showed a series of stripes based on the pixel range (See below Fig 15).

Fig 14 - ColorCycle in operation
![20241111_160258](https://github.com/user-attachments/assets/48c6f5b4-66bc-4deb-840c-5c1da4161c84)
Fig 15 - Stripes in operation
![20241126_185816](https://github.com/user-attachments/assets/29e85388-52fe-4d90-bf81-0f0475fa5287)

## **My VGA Design Edit**
Introduce your own design idea. Consider how complex/achievabble this might be or otherwise. Reference any research you do online (use hyperlinks).

My project idea was an even mixture of challenging and achievable given the time frame. My goal was to edit the *VGAColorStripes* code to generate rows instead of columns and within those columns, split them into five squares. This meant I would be able to edit the color of 25 squares to appear in any format I would want (as five squares per row x five rows = 25). The code would use a temporary variable to keep track of the sqaure generation as the range for row and column would be changing based on what square in what column would be drawn. I would use this alomg with the state machine logic from *VGAColorCycle* to show differing images of varying complexity. The goal was to start with a simple black screen, pivot to 5 rows of the same repeating colors using the same temporary variables to keep track of the rows and finally a display of concentric rings of color. 

### **Code Adaptation**
Briefly show how you changed the template code to display a different image. Demonstrate your understanding. Guideline: 1-2 short paragraphs.

I first began editing the code by generating a square, this meant I had to check the values for both row and columns in my if statements.  This was slightly complex to wrap my head around initially but I began to think in terms of co-ordinates quite quickly (See below Fig 16). Once a sqaure was generated I wanted to then begin using a temporary variable to increment through the rows. The next step once a row was generated (See below Fig 17), was to begin the state machine. The coding wasn't always straight forward as the logic can be difficult to wrap your head around (See below Fig 18).

Fig 16 - Generating a square using a variable in the columns
![20241126_181706](https://github.com/user-attachments/assets/054cad8d-05dd-416d-9f2d-f75b27f3c295)

Fig 17 - Generating a column
![20241202_151240](https://github.com/user-attachments/assets/5efbb48e-64c7-4e60-b778-1fb9c22f19af)

Fig 18 - demon
![20241125_172132](https://github.com/user-attachments/assets/3acf5298-f547-43af-baa8-fd58ea9744a7)
### **Simulation**
Show how you simulated your own design. Are there any things to note? Demonstrate your understanding. Add a screenshot. Guideline: 1-2 short paragraphs.

Once my project was set up, the simulation was ran (See below Fig 19). The first simulation seen below is zoomed in to pico-seconds which lead to the seemingly static signals. Upon zooming out we see the simulation as we would expect it to behave (See below Fig 20). Analysing the simulation the clock signal, vertical sync, reset and horizontal sync behave in more or less the exact same manner as in the other simulation for *ColorStripe.v*. The row and column registers do too as well as the colors and this is because the logic for this code is very much the same as the "ColorCycle.v" state machine. The colors are updated more often as the screen is split into 25 squares for each screen but this change is not visible in the color signals on such a small excerpt from the simulation. 

Fig 19 - Simulation of my code
![708124b3-573e-487d-88ee-6777b0f7d726](https://github.com/user-attachments/assets/61f49808-f5d0-459b-a66e-def683742b20)

Fig 20 - Simulation of my code enhanced on logic blocks
![6db28e66-c5b3-4101-99b2-ab8bfd7eac84](https://github.com/user-attachments/assets/ca2f0fd2-020b-44d0-a2b7-41781f7a86d7)

### **Synthesis**
Describe the synthesis & implementation outputs for your design, are there any differences to that of the original design? Guideline 1-2 short paragraphs.

(See below Fig 21) (See below Fig 22) (See below Fig 23)

Fig 21 - Synthesis of my code
![92c6e9a3-edde-45a8-bd3f-8e2bda37bde8](https://github.com/user-attachments/assets/7dab268c-cc06-4620-be6d-8576eab58375)

Fig 22 - Schematic of my code
![4fb206b0-9305-402a-8780-0662d60138df](https://github.com/user-attachments/assets/bc1d2b0a-1243-43c8-bd11-0002ef899996)

Fig 23 - Schematic of my code enhanced on logic blocks
![468bd7ff-5015-44d4-86c4-bae092cb0372](https://github.com/user-attachments/assets/054ae6b4-6adc-485d-b6bc-d34f7df2ef8b)

### **Demonstration**
If you get your own design working on the Basys3 board, take a picture! Guideline: 1-2 sentences.

The images below represent a full green screen (See below Fig 24), an incrementing display  the same row (See below Fig 25) a diagonal design (See below Fig 26) and finally, the concentric rings (See below Fig 27).

Fig 24 - GREEN
![20241202_170125](https://github.com/user-attachments/assets/b035d2d3-b791-4f27-a202-e1f009109785)

Fig 25 - Row generated with variables
![20241202_170129](https://github.com/user-attachments/assets/ba88b169-e83e-4992-8dec-d793d0fcae39)

Fig 26 - Diagonal state
![20241202_170135_009](https://github.com/user-attachments/assets/a3795e07-015f-4ccd-afcb-a4629f9ef8a4)

Fig 27 - Circles
![20241202_170126](https://github.com/user-attachments/assets/73916ce6-403c-4e34-8049-ffb958a2688e)

REFERENCES 
[1] arm, "What Is an FPGA?" [Online] Available: https://www.arm.com/glossary/fpga 

[2]  Wikipedia, "Application-specific integrated circuit" [Online] Available: https://en.wikipedia.org/wiki/Application-specific_integrated_circuit

[3] M. Wilson, "What is VGA? A Comprehensive Guide to Video Graphics Array" [Online] Available: https://www.hp.com/us-en/shop/tech-takes/what-is-vga-comprehensive-guide-video-graphics-array

[4] Wikipedia, "Vivado" [Online] Available:https://en.wikipedia.org/wiki/Vivado

[5] Diligent, "Basys 3 Artix-7 FPGA Trainer Board: Recommended for Introductory Users" [Online] Available:https://digilent.com/shop/basys-3-artix-7-fpga-trainer-board-recommended-for-introductory-users/

[6] M. Lynch, “System on Chip”, Lecture, ATU, Galway, 2024.

[7] Diligent Reference, "Using the Simulator in Vivado" [Online] Available:https://digilent.com/reference/programmable-logic/guides/simulation#:~:text=The%20simulation%20environment%20itself%20is,to%20generate%20elaborate%20test%20conditions.

[8] AMD, "Adaptive SoC & FPGA Support" [Online] Available: https://adaptivesupport.amd.com/s/question/0D52E00006hpkc2SAA/the-difference-between-implementation-and-synthesize?language=en_US
