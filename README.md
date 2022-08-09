# Advanced-Physical-Design-using-Openlane-Sky130

A 5 day-workshop where I learned about open source flow named openlane which specializes in RTL2GDS flow.In this workshop, we used PICORV32A RISC-V core design as a example.

# Day 1 Introduction to OpenLANE and Skywater130 pdk


## Invoking Openlane


```bash
docker
```  
After docker command
```bask
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
```
<img width="885" alt="no3" src="https://user-images.githubusercontent.com/98028428/183568566-db208827-a044-4ae6-bf1e-35a568ea71e3.PNG">

## Synthesis

Use Command 
```bash
run_synthesis
```
<img width="831" alt="no4" src="https://user-images.githubusercontent.com/98028428/183568632-cc061187-f502-45a2-a027-4beaaa2e22e0.PNG">
<img width="436" alt="no5" src="https://user-images.githubusercontent.com/98028428/183568676-6a63ed4b-3142-4314-b9f2-181c95fcbfa4.PNG">
<img width="528" alt="no6" src="https://user-images.githubusercontent.com/98028428/183568695-bdf8bdf9-41ed-4fff-8362-87a2443f0fb8.PNG">

#### Number of cells - 14876
#### Total Number of flops - 1613
#### Flop count = Total Number of flops/ Number of cells = 0.1084
#### 10.84% of flops used .
#### Buffer count - 1656
#### Buffer count = No. of buffer/Number of cells = 0.1113
#### 11.13 % buffer used.

# Day-2 Floorplan
 ## Chip Floorplanning
   Chip Floorplanning is the arrangement of logical block, library cells, pins on silicon chip. It makes sure that every module has been assigned an appropriate area and aspect ratio, every pin of the module has connection with other modules or periphery of the chip and modules are arranged in a way such that it consumes lesser area on a chip.
   
 ### Utilization Factor and Aspect Ratio
   Utilization Factor is ratio of the area of core used by standard cells to the total core area. The utilization factor is generally kept in the range of 0.5-0.6 i.e. 50% - 60%. Maintaining a proper utilization factor facilitates placement and routing optimization.
   
 ### Power Planning
   Power planning is a step in which power grid network is created to distribute power to each part of the design equally. This step deals with the unwanted voltage drop and ground bounce. Steady state IR Drop is caused by the resistance of the metal wires comprising the power distribution network. By reducing the voltage difference between local power and ground, steady-state IR Drop reduces both the speed and noise immunity of the local cells and macros.
   
 ### Pin Placement
   Pin placement is a important part of floorplanning as the timing delays and number of buffers required is dependent on the position of the pin. There are multiple pin placement option available such as equidistant placement, high-density placement.
 
 ### Floorplan using OpenLANE
   Floorplanning in OpenLANE is done using the following command. 
   ```bash
   run_floorplan
   ```
   Successful floorplan shows
   <img width="945" alt="no9" src="https://user-images.githubusercontent.com/98028428/183570971-1c8fca79-afa5-4ffa-8e2e-6e8abc6ea55d.PNG">
   
   Also a `.def` file is created
  ### This file contains the die area and placement of standard cells.
  ### DIEAREA in microns is 554.570 x 565.290 = 313492.8753
   <img width="563" alt="no13" src="https://user-images.githubusercontent.com/98028428/183571970-13e9877e-69d5-4381-b8d1-5d5f51e0a86e.PNG">

 ### Review Floorplan Layout in Magic
   Magic Layout Tool is used for visualizing the layout after floorplan. In order to view floorplan in Magic, following three files are required:
   #### 1. Technology File (`sky130A.tech`)
   #### 2. Merged LEF file (`merged.lef`)
   #### 3. DEF File
   Type the command from the following image to review the `floorplan` layout
   <img width="960" alt="no10" src="https://user-images.githubusercontent.com/98028428/183571353-31284fad-adce-4d31-b481-614aefe54e2b.PNG">
<img width="960" alt="no11" src="https://user-images.githubusercontent.com/98028428/183571582-21905101-fc5b-4338-b093-be611de296bd.PNG">

### Placement
After successfull synthesis and floorplan. In the Openlane terminal type
```bash
run_placement
```
<img width="945" alt="no12" src="https://user-images.githubusercontent.com/98028428/183571738-e3c4f745-3273-442c-b4ff-8d9c5a607fd1.PNG">

#### Placement Layout using Magic

<img width="957" alt="no14" src="https://user-images.githubusercontent.com/98028428/183572444-a054c234-5ec2-4717-823e-c6db06ec125c.PNG">

# DAY 3 library cell in Magic Layout and ngspice characterization

Git cloning the inverter cell design
```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
### Review cell Layout using Magic
<img width="578" alt="no15" src="https://user-images.githubusercontent.com/98028428/183573226-86999b34-e84a-40bf-98fc-d6dd66c24de8.PNG">
<img width="522" alt="no16" src="https://user-images.githubusercontent.com/98028428/183573562-14e91d6e-2100-495d-85c1-30924770c2dd.PNG">

### In the tkcon terminal of magic
Type
```bash
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
<img width="567" alt="no18" src="https://user-images.githubusercontent.com/98028428/183575886-174115ff-7e18-4624-a821-4f826df91a14.PNG">

<img width="324" alt="no1111" src="https://user-images.githubusercontent.com/98028428/183573969-5916b06d-f1a6-42a5-a182-8b79504ef25a.PNG">

<img width="946" alt="no2222" src="https://user-images.githubusercontent.com/98028428/183574347-b319f5c3-68ff-4d9e-93c7-c999d552c203.PNG">

### Rise transition = 64ps
### Fall transision = 43ps

# Day 4 - Pre-layout timing analysis and importance of good clock tree
### Things to note before routing
### The inverter cloned from github earlier has already made the ports but still this can be verified by following the necessary steps
#### 1. Input & Outut port must lie on the intersection of vertical & horizontal tracks.
#### 2. Widths of standard cell must be odd multiple of track pitch.
#### 3. Height of standard cell must be odd multiple of track verticle pitch.
#### Ensuring the grid are matched according the metal pitch and offset.

<img width="712" alt="no17" src="https://user-images.githubusercontent.com/98028428/183576530-ae7c0ef3-21b8-4980-95c3-18a22ed412fa.PNG">

### LEF file extracts as pin (ports) which are then usful in routing.
In the magic terminal
```bash
write sky130_vsdinv.lef
```
In magic to view layout only the LEF prompt.
```bash
expand
```
Invoke the openlane run the same synthesis on existing file created before by
```bash
docker
./flow.tcl interactive
package require openlane 0.9
prep -design picorv32a -tag <workingfolder> -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
## STA analysis

In the openlane , open openroad to run the sta with real clock and edit by adding higher order buffer for less slack
```bash
replace_cell <cell number> <celltype_>
```
example
```bash
replace_cell _13361_ sky130_fd_sc_hd__buf_2
```
replaces buffer of any size to 2 report

slack can be reduced by increasing the area and by changing the same in the base.sdc file

# Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

OpenLANE uses TritonRoute, an open source router for modern industrial designs. The router consists of several main building blocks, including pin access analysis, track assignment, initial detailed routing, search and repair, and a DRC engine. The routing process is implemented in two stages:

`Global Routing` - Routing guides are generated for interconnects.

`Detailed Routing` - Tracks are generated interatively. TritonRoute 14 ensures there are no DRC violations after routing.

The following command is used for routing.
```bash
run_routing
```

## `SPEF` File Generation
Standard Parasitic Exchange Format (SPEF) is an IEEE standard for representing parasitic data of wires in a chip in ASCII format. Non-ideal wires have parasitic resistance and capacitance that are captured by `SPEF`. OpenLANE consists of a tool named, SPEF_EXTRACTOR for generation of `SPEF` file. It is a python based parser which takes the `LEF` and `DEF` files as input arguments and generates the SPEF file. The following command is used for invoking the `SPEC_EXTRACTOR`.
```bash
cd <path-to-SPEF_EXTRACTOR-tool-directory>
python3 main.py <path-to-LEF-file> <path-to-DEF-file-created-after-routing>
```

# Acknowledgements

- [Nickson Jose](https://github.com/nickson-jose) - VSD VLSI Engineer
- [Kunal Ghosh](https://github.com/kunalg123) - Co-founder (VSD Corp. Pvt. Ltd)
