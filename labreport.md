# Mustafa Idris lab report

[Lab 1](#lab-1)
[Lab 2](#lab-2)
[Lab 3](#lab-3)
[Lab 4](#lab-4)
[Lab 5](#lab-5)
[Lab 6](#lab-6)

## Lab 1

### Lab 1 Objectives
1. [Setting up Quartis](#setting-up-quartis)
1. [7 Segment LED display](#7-segment-led-display)
1. [Programming the FPGA](#programming-the-fpga)
1. [Performing the Hex BCD Decoding](#performing-hex-bcd-decoding)
1. [Testing the Decoder Design](#testing-the-decoder-design)

#### Setting up Quartis

The first step to setting up quartis is downloading and installing it using [this link](https://www.intel.com/content/www/us/en/software-kit/665990/intel-quartus-prime-lite-edition-design-software-version-18-1-for-windows.html)

The directory structure that I've setup follows the same structure that was shown in [readme.md](./lab1/README.md#Creating-a-good-directory-structure)


#### 7 segment LED display

Four slide switches (SW3 to SW0) will be used as an input for the DE10. And a 4-bit binary number will be display on the right-most 7 segment display.

Before this is implemented, a quick initial test will be carried out to ensure that everything works as expected. the precompiled file [lab1task1_sol.sof](./lab1/src/lab1task1_sol.sof) will be used for this test.

To run the program, the following steps are followed:

Tools > Programmer > Hardware Setup > USB Blaster

Next, press AddFile, select the [lab1task1_sol.sof](./lab1/src/lab1task1_sol.sof) file. Then press the start button. If it works as expected, the last hex digit being displayed can be controlled using the least significant four switches.

#### Programming the FPGA

First the [task1 folder](./lab1/task1) is created.

Then, in Quartis, click File > New Project Wizard. Use task1 as the project name and task1_top as the top file. Select finish.

Click assignments -> Device > 10M50DAF484C7G (this is the device code)

Now, create a design file by clicking File > New > Verilog HDL.

Now, I will write a module to decode HEX to BCD

#### Performing hex BCD decoding

This code that will convert hex to 7seg is shown below.

```sv

```

The syntax can be checked by clicking Process > Analyze current file.

To include this module in the project, we press Project > Add current file to project

The next step is to create a top-level design file.

A new task1_top.v file is created, but this time we must specify that it is the top file by pressing Project >  Set as top-level entity

The following is the code used in the top file

```sv


```

To verify that all files are working, press Process > Start > Start analysis & elaboration.

#### testing the decoder design

To test the full design, first the pins need to be assigned. This can simply be done by inserting a text file with the pin information directly into the .qsf file.

File > Open > task1.qsf

Then, press edit > Insert file then insert the pin_assignment.txt file to the top of the .qsf file.

The last step required is to compile the project by clicking Process > Start compilation. If it compiles successfully, the code can be tested by following the last 2 steps in [the second task](./labreport.md/#7-segment-LED-display)

To gain more insight into how verilog is turned into FPGA hardware, you can access the following tools

Tools > Netlist Views > RTL Viewer

Tools > Netlist Views > Technology Map viewer

Tools > Timing Analyzer

#### Displaying the output of all 10 bits

The only change that needs to be made to output all 10 bits is made to the top file.

We instantiate the 7seg to hex module 3 times, since there are only 10 input bits, the 2 MSF bits in the MSF hex digit are 0s.

## Lab 2

### Lab 2 Objectives
1. [Designing a NIOS II System](#designing-a-nios-ii-system)
1. [Implementing the design on an FPGA](#Implementing-the-design-on-an-FPGA)
1. [Displaying a message on the terminal](#displaying-a-message-on-the-terminal)
1. [Exploring capabilities of the design](#exploring-capabilities-of-the-design)

#### Designing a NIOS II System 

NIOS 2 is a soft=core processor that is provided by intel to target interl FPGA devices. Following the steps in the [manual]() between pages 11 and 26 allows me to print hello world.


#### Implementing the design on an FPGA



#### Displaying a message on the terminal

#### Exploring capabilites of the design


## Lab 3

### Lab 3 Objectives
1. [Interfacing with the accelerometer](#interfacing-with-the-accelerometer)
1. [Understanding the design process](#understanding-the-design-process)
1. [Reading acceleration values](#read-acceleration-values)
1. [Designing a Low pass FIR filter](#design-lowpass-FIR-filter)
1. [Investigating the impact of precision on performance](#Investiging-the-impact-of-precision-on-performance)

#### Interfacing with the accelerometer

The DE10-lite comes with a G-sensor (digital accelerometer sensor module). The digitalized output is 16 bits and is accessed through the SPI and I2C digital interfaces.

SPIs are among the most common interfaces between a microprocessor and a peripheral.

#### Understanding the design process

Open [DE10_LITE_Golden_top project](lab3/src/Golden_Top/) and launch platform design.

Add the following IPs:
- NIOS 2 Processor
- On Chip memory (66536 size)
- JTAG UART
- PIO for LED (10 bit output)
- Interval timer
- Accelerometer SPI Mode

Once you've added the IPs, follow the diagram below to connect everything

![connections](./lab3/images/Qsys.jpg)



#### Reading acceleration values

System > Assign base address > set reset and exception vectory memory to onchip_memory.s1

Save as nios_accelerometer.qsys

Generate HDL

In Quartis, add the generated file to your project

Paste the code from nios_accelerometer_inst.v into the top file.

The diagram below shows how the instantiation should look like.

![top file](lab3/images/top_level_inst.png)

Compile > Launch Eclipse > New > NIOS 2 Application and BSP from template

Select .sopcinfo > Hello world small as template

The code used will be the provided [led accelerometer C file](lab3/src/led_acceleromter_main.c)

```convert_read()``` takes the reading and determines how the LEDs will light up.

```sys_timer_isr()``` is an interrupt service routine that executed when an interrupt is received.

Calling this function everytime we want to use ```convert_read()``` ensures that the processor efficient switched between the while loop code and the convert_read().

```alt_print()``` is the funciton used to print values onto the terminal

Build the project in Eclipse. Program the device using terminal commands.

The LEDs indicate the tilting position

#### Design a low pass FIR Filter



#### Investigating the impact of precision on performance

## Lab 4

### Lab 4 Objectives
1. [UART Communication](#UART-communication)
1. [Extending the lab3 system](#extending-lab3-system)
1. [Updating Coefficients](#updating-coefficients)
1. [Plotting the accelerometer data](#plotting-accelerometer-data)

#### UART communication

#### Extending lab3 system

#### Updating coefficients

#### Plotting accelerometer data


## Lab 5

### Lab 5 Objectives
1. [Setting up an EC2 Instance](#setting-ec2-instance)
1. [Communicating through SSH](#communicating-through-ssh)
1. [Communicating using TCP](#communicating-using-tcp)

#### Setting up EC2 Instance

#### Communicating through SSH

#### Communicating using TCP

## Lab 6

### Lab 6 Objectives
1. [Creating a DynamoDB database](#creating-a-dynamodb-database)
1. [Loading sample data](#loading-sample-data)
1. [Using Dynamo commands](#using-dynamo-commands)

#### Creating a DynamoDB Database

#### loading sample data

#### using dynamo commands



