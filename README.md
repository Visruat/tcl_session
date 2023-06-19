# Comprehensive TCL Workshop: From Introduction to Advanced Scripting Techniques in Design and Synthesis
--------------------------------------------------------------------------------------------------------
## Introduction
Tool Command Language (Tcl) is a scripting language commonly used in various domains, including software development, system administration, and electronic design automation (EDA). Tcl is known for its simplicity, flexibility, and ease of integration with other programming languages and tools. Tcl scripting involves writing scripts in the Tcl language to automate tasks, execute commands, and manipulate data.

links for easy navigation:
1. [DAY-1](https://github.com/Visruat/VSD-TCL/blob/main/README.md#day-1)
2. [DAY-2](https://github.com/Visruat/VSD-TCL/blob/main/README.md#day-2)
3. [DAY-3](https://github.com/Visruat/VSD-TCL/blob/main/README.md#day-3) 
4. [DAY-4](https://github.com/Visruat/VSD-TCL/blob/main/README.md#day-4) 
5. [DAY-5](https://github.com/Visruat/VSD-TCL/blob/main/README.md#day-5) 


## DAY-1

__Create Command (panda) and pass csv file from UNIC shell to Tcl script__

The command was created with the following algorithm:
1) letting the system know that its a UNIX script

```
#!/bin/tcsh -f  
```

2) Creating the logo

```
echo " *********        ****       ***      **     ****         ****                "
echo " **     **       **  **      ****     **     ** **       **  **               "
echo " **     **      **    **     ** **    **     **  **     **    **              " 
echo " *********     **      **    **  **   **     **   **   **      **             " 
echo " **            **********    **   **  **     **  **    **********             "
echo " **            **      **    **    ** **     ** **     **      **             "
echo " **            **      **    **     ****     ****      **      **             "
echo 
echo " a tcl command created by VISRUAT T R (its pronounced as Vishruth)            "
```

3) verifying three general scenarios for a user POV
  - user doesnt enter the csv file

![Screenshot from 2023-06-15 11-23-50](https://github.com/Visruat/VSD-TCL/assets/125136551/6503b789-2fbe-461e-9f81-99a4e564f794)


  - user enters the wrong csv file/ file doesnt exist


![Screenshot from 2023-06-15 11-35-33](https://github.com/Visruat/VSD-TCL/assets/125136551/53b2f085-f63a-4b68-9c16-5e56df8e23de)

  - user enters __-help__

![Screenshot from 2023-06-15 11-37-11](https://github.com/Visruat/VSD-TCL/assets/125136551/68c64691-8eef-47df-918f-241469033199)


4) source the Unix shell to the Tcl script by passing the required csv file 

```
tclsh pandabro.tcl $argv[1] 
```
Note : Make sure the file is executable by using the command ``` chmod -R 777 panda ``` 

## DAY-2
__Converting inputs to format[1] and feeding it to yosys for synthesis__

  __- Create Variables__

![Screenshot from 2023-06-15 15-11-10](https://github.com/Visruat/VSD-TCL/assets/125136551/1a039045-a2f5-4bc9-84c5-14f170450bd8)

  __- Checking if the directories exist or not ( done to prevent the script from breaking )__

![Screenshot from 2023-06-15 15-29-57](https://github.com/Visruat/VSD-TCL/assets/125136551/17c7398b-593d-40f9-a267-35bc6e7afc9a)

Displays an error when the required file is not in the needed directory

![Screenshot from 2023-06-15 16-50-05](https://github.com/Visruat/VSD-TCL/assets/125136551/a82bf99c-b7bf-4288-b77a-5766d2ac8112)


## DAY-3

__Read Constraints.csv file and convert to sdc format__

  - getting clock details from csv file and writing it the sdc file in the required format
  - getting input details from csv file and writing it in the required format
  - getting output details from csv file and writing it in the required format
 
Note: need to identify bussed and non bussed inputs and outputs before entering in the required format.

The script writing the sdc constraints.

![Screenshot from 2023-06-18 14-19-37](https://github.com/Visruat/VSD-TCL/assets/125136551/8d0cb933-cbc5-4aae-93dd-bc5819c33a3a)

A snip of the sdc file 

![Screenshot from 2023-06-18 14-16-25](https://github.com/Visruat/VSD-TCL/assets/125136551/d80f44f6-45ce-418a-b067-bebf900f1217)

## DAY-4

### YOSYS (Yosys Open SYnthesis Suite)

YOSYS is an open-source RTL synthesis and formal verification framework for digital circuits. It takes RTL descriptions (e.g., Verilog) as input and performs synthesis to generate a gate-level netlist. YOSYS supports technology mapping, optimization, and formal verification. It has a scripting interface, integrates with other EDA tools, and is widely used in academia and industry for digital design tasks.

__Creating scripts for synthesis and running it on yosys__

  - creating script for Hierarchy check
  - running hierarchy check

  Case 1: When all the modules are present and called correctly 

![Screenshot from 2023-06-18 16-51-32](https://github.com/Visruat/VSD-TCL/assets/125136551/f0220dd8-4868-4a44-8b11-fe8a37ae9ca8)

  Case 2: referenced in module error due to calling incorrectly or module doesnt exist

![Screenshot from 2023-06-18 16-53-40](https://github.com/Visruat/VSD-TCL/assets/125136551/2ea42db4-f89a-4bc6-b4fb-345ab93d1637)

  - after the hierarchy check is passed . The main synthesis script is written.
  - running synthesis if there are no error in hierarchy check.

  Case 1: No error in hierarchy

![Screenshot from 2023-06-18 17-10-49](https://github.com/Visruat/VSD-TCL/assets/125136551/916f0c46-9f45-4812-8ab6-a7f234ed71f5)

  Case 2: error encountered in hierarchy

![Screenshot from 2023-06-18 17-12-06](https://github.com/Visruat/VSD-TCL/assets/125136551/24095315-680a-4905-960c-6da157f034d3)


## DAY-5

__create script for OpenTimer and run STA analysis followed by generation of Quality of Results (QoR)__

_An introduction to procs_
  - To generate a script for OpenTimer I will be making use of procs. Procs are an external tcl file that perform an operation that is specified in it when sourced to the main tcl file. It works similar to how a function works in Python Programming. An example of a proc would be read_liberty <args> where options _like -lib, -late, -early and /or <filename>_ can be passed as an arguememt to the proc. Once the proc is sourced in the main tcl script the read_liberty command will be executed by referring to the proc and mapping the arguements to the external tcl script(proc script). At the end of the proc command, the main tcl script will be left with the output of the proc.
  
Refer to the image for pictorial undertanding

<img src = "https://github.com/Visruat/VSD-TCL/assets/125136551/a4d8166c-5026-4e26-b7de-499626926123" width ="350" height ="350">  <img src = "https://github.com/Visruat/VSD-TCL/assets/125136551/c33f56bb-99fe-42f5-ba1f-d21e86795f96" width ="600" height ="350">



 










