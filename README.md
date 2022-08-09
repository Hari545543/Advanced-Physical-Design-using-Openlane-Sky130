# Advanced-Physical-Design-using-Openlane-Sky130

A 5 day-workshop where I learned about open source flow named openlane which specializes in RTL2GDS flow.In this workshop, we used PICORV32A RISC-V core design as a example.

# Day 1 Introduction to OpenLANE and Skywater130 pdk


## Invoking Openlane


```bash
$docker
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

[Synthesis](#https://github.com/INPLHA/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/README.md)
