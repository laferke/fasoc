# Memory-Gen: Synthesizable SRAM macro generator. 
Memory-Gen is a technology agnostic python-based memory design platform that automates the generation of the synthesizable SRAM macros that are optimized to meet constraints set by system level. It enables iterative memory macro design space exploration to facilitate optimal circuit-architecture co-design for memories.

# Version Details:
```
Latest Version : Beta-0.1
Date           : May 21, 2020
```
# What's in the release
- First Beta version of the tool.
- Supports the FinFET process in addition to all the features of the alpha-1.0 version. Refer below for alpha-1.0 details.
```
Previous Version : Alpha-1.0                                                             
Date    : Apr 26, 2019 
# What's in the release
- First version of the tool. 
- Generates the SRAM memory macro and its associated views (GDS, LEF, LIB, DB and CDL) for given user specs.
- This release includes:
  - Synthesizable SRAM verilog generation.
  - Synthesis and PNR of the SRAM.
  - Performing the physical verification (PV) check.
  - Generating the associated views.
```
# Tool Setup:
The tool is heavily dependent on the EDA tools as well as the technology PDK. Hence, ensure that all the required tools are available and enabled in the environment. For the list of the tools, technology info and setting up the environment, refer https://github.com/idea-fasoc/fasoc/blob/master/README.md

# Tool Usage:

## Tool directory structure:
```
        memory-gen
	|--- apr
	|--- bin
	|--- Makefile   
	|--- Readme.md        
	|--- SRAM_input_spec.json 		Generated
	|--- runfiles				Generated
	|--- outputs				Generated
	|--- MemGen.log				Generated 

```
  __apr__

   - Folder with the synthesis and APR scripts that runs the CADRE flow. Due to the NDA dependencies, the folder is moved under  **../../private/generators/memory-gen/**. To access this folder, Please contact ajayi@umich.edu.
```
        apr
        |--- include.mk
	|--- Makefile
	|--- scripts                           Placeholder for PDK specific apr scripts
	|--- src                               Placeholder for design files.
```
  __bin__

  - Directory with source scripts of the tool that encapsulates the memory macro generation process. 

  __Makefile__

  - Makefile to run the memory generator.

  __Readme.md__

  - Read me file with the instructions for running the tool.

  __SRAM_input_spec.json__

  - Sample SRAM input specification file in json format. It has to be provided as command line argument to the 

  __runfiles__

  - Placeholder directory for all the intermediate generated files like verilog files etc. This directory is created during the run. 

  __outputs__

  - The tool will create this directory on the go and will have all the output files that tool generates.

  __MemGen.log__

  - The log file captures all the run info and any errors during the run and helps us de-bugging the issues. A log file **"MemGen.log”** will be created during the run in the top-level run directory. The current version of the tool does not capture the log of the EDA tools. 

## Inputs:
MemGen accepts the following inputs to generate a memory macro.
```
- Memory specifications
  - Specifications of the memory to be generated. 
- Output directory
  - Directory to store the SRAM outputs.
- Technology platform
  - PDK technology info the SRAM has to be generated. 
```
The inputs to the memory generator are passed as command line arguments to the exectuable **MemGen**. Use the following command line options to provide the inputs to the tool. 
- **--specfile**  : Pass the input spec file with this argument.
- **--outputDir** : Specify the outputs directory with this option.
- **--platform**  : Indicate the technology PDK node with this argument.

The Memory specifications are given through json file. Sample spec file is below. 
```
  **Sample input spec json file**
  {
  "instance_name": "SRAM_16KB",
  "generator": "memory-gen",
  "specifications": {
    "word_size": 32,
    "capacity": 134672
  }
  }
```
Update only the following variables according to the user requirements. 
  - Instance name: 
    - It is the design name of the SRAM. The Verilog module name and all the outputs will be named by the instance name. 
  - Memory specifications:
    - Capacity: Size of the SRAM in bits. 
      - Ex: 65536 for 8KB SRAM.
    - Word Size: Width of the Data bus.Fixed at 32 for this release. 
      - Ex: 32
    - Technology (PDK): TSMC 65nm for now 
      - Ex: tsmc65lp/gf12lp

## Outputs:
 - The tool generates the multi-bank memory macro and its asssociated views (GDS, Verilog, LIB, LEF, DB, & CDL). All the outputs will be under the output directory specified by the user.
   
## Running the tool:
1. The tool runs in two modes. Refer below for the details on modes.
   __Verilog__
     - In this mode, the tool outputs only the synthesizable verilog files.
     - To run the tool in this mode, add the option **-m verilog** to the MemGen executable. 
     - This is the default mode of the MemGen.
   __Macro__
     - In this mode, the tool generates the synthesizable verilog files and performs the synthesis and pnr to generate the SRAM hard macro and its associtated views.
     - To run the tool in this mode, add the option **-m macro** to the MemGen executable.
2. Once the tool setup is completed, update the input specification file **./SRAM_input_spec.json** as per the requirements. Refer the inputs section above for the details of spec file. 
3. Execute either of the following commands in the **memory-gen** folder to run the generator to produce a multi-bank memory macro.
3. After the run is complete, all the outputs will be under the user speicifed output directory. 
```
                                    make mem_<platform>_<mode>
```
 - **platform** is the PDK technology memory needs to be generated.  
   - For TSMC 65nm, platform is *tsmc65lp*. 
   - For GF 12nm, it is *gf12lp*.
 - **mode** is the operating mode of memory.
   - For verilog generation, mode is *verilog*.
   - For macro generation, mode is *macro*.
  - For example: To generate memory macro in TSMC 65nm, execute **make mem_tsmc65lp_macro** 
 
# Known Issues/Limitations:
1. Capacity is in multiples of 2KB. 
2. Word size is 32.
3. Supports only GF12LP and TSMC 65nm PDK.

# Contact Details:
For further questions, please feel free to contact us at idea-uva@virginia.edu.

# APPENDIX
## Bibliography
1. S. Kamineni, S. Gupta and B. H. Calhoun, "MemGen: An Open-Source Framework for Autonomous Generation of Memory Macros," 2021 IEEE Custom Integrated Circuits Conference (CICC), 2021, pp. 1-2, doi: 10.1109/CICC51472.2021.9431501.


