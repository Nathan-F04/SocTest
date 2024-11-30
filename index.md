---
layout: home
title: FPGA VGA Driver Project
tags: fpga vga verilog
categories: demo
---

Welcome to my System on Chip (Soc) Video Graphics Array(Vga) project. Here I catalog the timeline of the cumulative work that lead to my final code.

## **Template VGA Design**
### **Project Set-Up**
Summarise the project set-up and design flow. Include a screenshot of your own set-up, for example see the image of my Project Summary window below. Guideline 1 short paragraph.
My setup
![image](https://github.com/user-attachments/assets/ae01aa6d-d668-4c10-a8e2-8007570e6973)

Pinout
![20241130_114624](https://github.com/user-attachments/assets/98b9606a-33e2-4b8b-949f-596ca53b7d3c)


### **Template Code**
Outline the structure and design of the Verilog code templates you were given. What do they do? Include reference to how a VGA interface works. Guideline: 2/3 short paragraphs, consider including screenshot(s).

The templates we were given are comprised of several parts. In VGA Top there is the *clk wizip*, *VGA Sync*, *VGA ColorCylce* (or *VGA ColorStripes* depending on the template) and *Logic assaignment* (See below Fig G). These logical blocks each have their own role to play along with the clock signal (*clk*), reset(*rst*), *h_sync* and *v_sync*. The signals vga_red, vga_green, vga_blue are the signals for the colors displayed. They are four bits long and together make a 12 bit number to generate a color with the rgb values. The code sets the pixel rgb value for a monitor in the range of 640x480 (See below Fig H) 

Architecture diagram-Fig G
![20241130_120139](https://github.com/user-attachments/assets/593ef348-7df9-4f66-922e-d06a9e8fec59)

Grid
![20241130_122917](https://github.com/user-attachments/assets/e1599f77-4ed2-4deb-8e14-11d47ce46faf)

For this project we were given two templates to test. Initially we were given the VGAColorCycle.v file which operated using a state machine logic to transition through a series of colors. The statemachine used twelve bits and set the rgb values to generate the color desired (See below Fig B). The code would use a time delay to show a color for a period and then switch to the next state or color.

Once the initial code was operational, we were given the VGAColorStripes.v file. This was a more complex file, using a series of if statements to check if the program was between a specific range of columns before implementing a specific color for said range. This lead to a series of columns with differing colors depending on the range of the column pixels ie blue for all pixels between pixel column 80 to 160. This code would serve as the foundation for how I would create my own project.

State machine-Fig B
![20241130_122035](https://github.com/user-attachments/assets/aec0ec85-a29a-446b-ae2a-e0e4238cf312)

### **Simulation**
Explain the simulation process. Reference any important details, include a well-selected screenshot of the simulation. Guideline: 1/2 short paragraphs.
### **Synthesis**
Describe the synthesis and implementation processes. Consider including 1/2 useful screenshot(s). Guideline: 1/2 short paragraphs.
### **Demonstration**
Perhaps add a picture of your demo. Guideline: 1/2 sentences.

The implementation of VGAColorCylce showed a different color after a delay using a statemachine (See below Fig A). VGAColorStripes was a more complex file and showed a series of stripes based on the pixel range (See below Fig C).
ColorCycle in operation-Fig A
![20241111_160258](https://github.com/user-attachments/assets/48c6f5b4-66bc-4deb-840c-5c1da4161c84)
Stripes in operation-Fig C
![20241126_185816](https://github.com/user-attachments/assets/29e85388-52fe-4d90-bf81-0f0475fa5287)

## **My VGA Design Edit**
Introduce your own design idea. Consider how complex/achievabble this might be or otherwise. Reference any research you do online (use hyperlinks).

My project idea is an even mixture of challenging and achievable given the time frame. My goal is to edit the VGAColorStripes code to generate rows instead of columns and within those columns, split them into five squares. This means I can edit the color of 25 squares that appear in any format I would want. The code would use a temporary variable to keep track of the sqaure generation as the range for row and column would be changing based on what square in what column would be drawn.
### **Code Adaptation**
Briefly show how you changed the template code to display a different image. Demonstrate your understanding. Guideline: 1-2 short paragraphs.

I first began editing the code by generating a square, this meant I had to check the values for both row and columns in my if statements.  This was slightly complex to wrap my head around initially but I began to think in terms of co-ordinates quite quickly (See below Fig D). Once a sqaure was generated I wanted to then begin using a temporary variable to increment through the columns as I had five if statements to check the range for columns with the same row value to test one column. The next step once a column was generated (See below Fig E), was to change both rows and columns if time wasn't an issue. If time became an issue I would then repeat the first five if statements with the rows manually edited. The coding wasn't always straight forward as the logic can be difficult to wrap your head around (See below Fig F).  

Generating a square using a variable in the columns-Fig D
![20241126_181706](https://github.com/user-attachments/assets/054cad8d-05dd-416d-9f2d-f75b27f3c295)

Generating a column-Fig E
INSERT IMAGE HERE

demon-Fig F
![20241125_172132](https://github.com/user-attachments/assets/3acf5298-f547-43af-baa8-fd58ea9744a7)
### **Simulation**
Show how you simulated your own design. Are there any things to note? Demonstrate your understanding. Add a screenshot. Guideline: 1-2 short paragraphs.
### **Synthesis**
Describe the synthesis & implementation outputs for your design, are there any differences to that of the original design? Guideline 1-2 short paragraphs.
### **Demonstration**
If you get your own design working on the Basys3 board, take a picture! Guideline: 1-2 sentences.

Detail here how far you got

INSERT FINAL IMAGE HERE

## **More Markdown Basics**
This is a paragraph. Add an empty line to start a new paragraph.

Font can be emphasised as *Italic* or **Bold**.

Code can be highlighted by using `backticks`.

Hyperlinks look like this: [GitHub Help](https://help.github.com/).

A bullet list can be rendered as follows:
- vectors
- algorithms
- iterators

Images can be added by uploading them to the repository in a /docs/assets/images folder, and then rendering using HTML via githubusercontent.com as shown in the example below.

<img src="https://raw.githubusercontent.com/melgineer/fpga-vga-verilog/main/docs/assets/images/VGAPrjSrcs.png">
