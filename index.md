![20241202_170129](https://github.com/user-attachments/assets/af1ab804-3006-4eb0-94d3-f7bb93cbc67f)---
Layout: home
Title: FPGA VGA Driver Project
Tags: fpga vga verilog
Categories: demo
---
REMEMBER TO PUT MY NAME ON THIS (Nathan Ferry)

Welcome to my System on Chip (Soc) Video Graphics Array (Vga) project. Here, I catalog the timeline of the cumulative work that lead to my final code.

## **Template VGA Design**
### **Project Set-Up**
Summarise the project set-up and design flow. Include a screenshot of your own set-up, for example see the image of my Project Summary window below. Guideline 1 short paragraph.
My setup
![image](https://github.com/user-attachments/assets/ae01aa6d-d668-4c10-a8e2-8007570e6973)

Pinout
![20241130_114624](https://github.com/user-attachments/assets/98b9606a-33e2-4b8b-949f-596ca53b7d3c)


### **Template Code**
Outline the structure and design of the Verilog code templates you were given. What do they do? Include reference to how a VGA interface works. Guideline: 2/3 short paragraphs, consider including screenshot(s).

The templates we were given are comprised of several parts. In VGA Top there is the *clk wizip*, *VGA Sync*, *VGA ColorCylce* (or *VGA ColorStripes* depending on the template) and *Logic assaignment* (See below Fig G). These logical blocks each have their own role to play along with the clock signal (*clk*), reset(*rst*), *h_sync* and *v_sync*.  The clock signal is used for synchronisation for example, some code is executed on a positive clock edge. *VGA Sync* outputs *h_sync* and *v_sync* which stand for vertical and horizontal sync which is used for measuring the time across the horizontal and vertical axes. *VGA ColorCylce* (or *VGA ColorStripes* depending on the template) and *Logic assaignment* are closely intwined as the first outputs the red green and blue signals which is joined with logic to display the colors on output. The signals vga_red, vga_green, vga_blue are the signals for the colors displayed. They are four bits long and together make a 12 bit number to generate a color with the rgb values. The code sets the pixel rgb value for a monitor in the range of 480x640. (See below Fig H) 

Architecture diagram-Fig G
![20241130_120139](https://github.com/user-attachments/assets/593ef348-7df9-4f66-922e-d06a9e8fec59)

Grid-Fig H
![20241130_122917](https://github.com/user-attachments/assets/e1599f77-4ed2-4deb-8e14-11d47ce46faf)

For this project we were given two templates to test. Initially we were given the VGAColorCycle.v file which operated using a state machine logic to transition through a series of colors. The statemachine used twelve bits and set the rgb values to generate the color desired (See below Fig B). The code would use a time delay to show a color for a period and then switch to the next state or color.

Once the initial code was operational, we were given the VGAColorStripes.v file. This was a more complex file, using a series of if statements to check if the program was between a specific range of columns before implementing a specific color for said range. This lead to a series of columns with differing colors depending on the range of the column pixels ie blue for all pixels between pixel column 80 to 160. These templates would serve as the foundation for how I would create my own project.

State machine-Fig B
![20241130_122035](https://github.com/user-attachments/assets/aec0ec85-a29a-446b-ae2a-e0e4238cf312)

### **Simulation**
Explain the simulation process. Reference any important details, include a well-selected screenshot of the simulation. Guideline: 1/2 short paragraphs.

Stripes sim zoomed in
![0fd8f165-b98d-4ca0-8cd5-fdf651650e9a](https://github.com/user-attachments/assets/55609dc2-575c-4cf6-b6f8-b18a807bf37a)

Stipes sim zoomed out
![68ddb1b1-3a91-481f-b227-522b5b28966a](https://github.com/user-attachments/assets/68993b88-a2d1-47f4-a9f8-d260018ceafe)

### **Synthesis**
Describe the synthesis and implementation processes. Consider including 1/2 useful screenshot(s). Guideline: 1/2 short paragraphs.

color cycle synthesis design
![766468aa-b927-4d63-baca-624692be35c6](https://github.com/user-attachments/assets/00a965ec-c7a3-44ac-b5f2-2dfacc41fa93)

color cycle schematic zoomed out
![87cd4dfb-858f-415a-8a0c-6511d9eeacd5](https://github.com/user-attachments/assets/e34fc403-5fca-4747-8b68-e9ceca6848d2)

color cycle schematic zoomed in
![7f0c4be5-aee8-4783-9226-ef4c0420d341](https://github.com/user-attachments/assets/2555145e-f03a-4dcd-abe8-c42a7c65c144)

Color stripes synthesised design
![04d057e9-6cd2-4d71-b65c-e2b78c31e0dc](https://github.com/user-attachments/assets/e4cc833f-6306-4b46-838f-28d61b8bfa0c)

color stripes schematic
![c67055c2-c507-4932-a613-ac40ea863b6a](https://github.com/user-attachments/assets/5c788e3d-b265-4a86-9519-d72f31ffa1c1)

Color stripes schematic zoomed in
![0e486d52-c412-41cb-9792-f1b7a92a6fea](https://github.com/user-attachments/assets/58b82be9-3bcf-43f1-8b0f-1f378b3675a5)
### **Demonstration**
Perhaps add a picture of your demo. Guideline: 1/2 sentences.

The implementation of *VGAColorCycle* showed a different color after a delay using a statemachine (See below Fig A). *VGAColorStripes* was a more complex file and showed a series of stripes based on the pixel range (See below Fig C).

ColorCycle in operation-Fig A
![20241111_160258](https://github.com/user-attachments/assets/48c6f5b4-66bc-4deb-840c-5c1da4161c84)
Stripes in operation-Fig C
![20241126_185816](https://github.com/user-attachments/assets/29e85388-52fe-4d90-bf81-0f0475fa5287)

## **My VGA Design Edit**
Introduce your own design idea. Consider how complex/achievabble this might be or otherwise. Reference any research you do online (use hyperlinks).

My project idea was an even mixture of challenging and achievable given the time frame. My goal was to edit the *VGAColorStripes* code to generate rows instead of columns and within those columns, split them into five squares. This meant I would be able to edit the color of 25 squares to appear in any format I would want (as five squares per row x five rows = 25). The code would use a temporary variable to keep track of the sqaure generation as the range for row and column would be changing based on what square in what column would be drawn. I would use this alomg with the state machine logic from *VGAColorCycle* to show differing images of varying complexity. The goal was to start with a simple black screen, pivot to 5 rows of the same repeating colors using the same temporary variables to keep track of the rows and finally a display of concentric rings of color. 
### **Code Adaptation**
Briefly show how you changed the template code to display a different image. Demonstrate your understanding. Guideline: 1-2 short paragraphs.

I first began editing the code by generating a square, this meant I had to check the values for both row and columns in my if statements.  This was slightly complex to wrap my head around initially but I began to think in terms of co-ordinates quite quickly (See below Fig D). Once a sqaure was generated I wanted to then begin using a temporary variable to increment through the rows. The next step once a row was generated (See below Fig E), was to begin the state machine. The coding wasn't always straight forward as the logic can be difficult to wrap your head around (See below Fig F).

Generating a square using a variable in the columns-Fig D
![20241126_181706](https://github.com/user-attachments/assets/054cad8d-05dd-416d-9f2d-f75b27f3c295)

Generating a column-Fig E
![20241202_151240](https://github.com/user-attachments/assets/5efbb48e-64c7-4e60-b778-1fb9c22f19af)

demon-Fig F
![20241125_172132](https://github.com/user-attachments/assets/3acf5298-f547-43af-baa8-fd58ea9744a7)
### **Simulation**
Show how you simulated your own design. Are there any things to note? Demonstrate your understanding. Add a screenshot. Guideline: 1-2 short paragraphs.

My simulation
![708124b3-573e-487d-88ee-6777b0f7d726](https://github.com/user-attachments/assets/61f49808-f5d0-459b-a66e-def683742b20)

My sim zoomed out
![6db28e66-c5b3-4101-99b2-ab8bfd7eac84](https://github.com/user-attachments/assets/ca2f0fd2-020b-44d0-a2b7-41781f7a86d7)

### **Synthesis**
Describe the synthesis & implementation outputs for your design, are there any differences to that of the original design? Guideline 1-2 short paragraphs.

My synthesis
![92c6e9a3-edde-45a8-bd3f-8e2bda37bde8](https://github.com/user-attachments/assets/7dab268c-cc06-4620-be6d-8576eab58375)

My schematic
![4fb206b0-9305-402a-8780-0662d60138df](https://github.com/user-attachments/assets/bc1d2b0a-1243-43c8-bd11-0002ef899996)

Zoomed in my schematic
![468bd7ff-5015-44d4-86c4-bae092cb0372](https://github.com/user-attachments/assets/054ae6b4-6adc-485d-b6bc-d34f7df2ef8b)

### **Demonstration**
If you get your own design working on the Basys3 board, take a picture! Guideline: 1-2 sentences.

The images below represent a full black screen (See below Fig I), an incrementing display which repeated the same row five times (See below Fig J) and finally, the concentric rings (See below Fig K).

INSERT FINAL IMAGE HERE-I circles
![20241202_170126](https://github.com/user-attachments/assets/73916ce6-403c-4e34-8049-ffb958a2688e)

INSERT FINAL IMAGE HERE-J row
![20241202_170129](https://github.com/user-attachments/assets/ba88b169-e83e-4992-8dec-d793d0fcae39)

INSERT FINAL IMAGE HERE-K diag
![20241202_170135_009](https://github.com/user-attachments/assets/a3795e07-015f-4ccd-afcb-a4629f9ef8a4)

INSERT FINAL IMAGE HERE- GREEN
![20241202_170125](https://github.com/user-attachments/assets/b035d2d3-b791-4f27-a202-e1f009109785)


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
