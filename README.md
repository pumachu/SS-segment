#  Solution-Mixture-Analysis
This repository contains analysis scripts related to the methods described in our recent paper. The provided codes facilitate the analysis of polymer bulk structures and solution-phase behavior.

The three conjugated polymers investigated in this study—**P-2O**, **P-SO2**, and **PBDTTSO**—serve as materials for the organic hydrogen evolution reaction (HER), as described in our paper. To simulate the solution environment during the HER process, **AA**, **NMP**, and **H2O** molecules were added into the system during the solution-phase molecular dynamics (MD) simulations.


##  Project Structure
- `Analysis_and_run/`: Contains all scripts used for data analysis.
- `FORCEFIELD/`: Includes topology files and forcefield parameters required for running GROMACS simulations.
- `QM_GAS_PHASE/`: Contains Gaussian log files generated from neutral ground-state optimization calculations.
- `BULK_MD/`: Provides sequential `mdp` and `gro` files used for generating bulk polymer structures for molecular dynamics (MD) simulations.
- `SOLUTION_MD/`: Provides sequential `mdp` and `gro` files used for simulating the polymer-solvent interface in solution MD simulations.
- `TRAJECTORY/`: Contains trajectory files resulting from the NPT simulations of the P-2O polymer solution.

##  MD_SIMULATIONS
This section details the workflow and files involved in our molecular dynamics simulations. The flowchart of the entire MD simulation procedure is provided below:
![image](https://github.com/user-attachments/assets/ef838ae4-7732-4d2c-894c-b8540ab32544)
###  BULK_MD
This directory includes step-by-step processes for generating bulk polymer structures for all three polymers studied (P-2O, P-SO2, PBDTTSO). The bulk generation process is divided into two main stages:

1. **Amorphous bulk structure generation**
2. **Smooth polymer-solution interface generation**

Each step is organized according to the folder naming sequence, which contains the final structure `.gro` files and corresponding `.mdp` files. To perform the MD simulations, users should also incorporate the forcefield information available in the `FORCEFIELD` directory.

###  SOLUTION_MD
After generating the polymer-solution interface, this section covers additional MD simulations where solvent molecules are explicitly added to the polymer system. These simulations are used to model the solvent penetration process within the polymer matrix.


##  Analysis scripts
###  1. Density Profile Analysis (```density_profile.ipynb```)
-  Reads the `.tpr` and `.gro` files of the solution structure.
-  Computes the density profile of **polymer, AA, NMP, and water**.
-  Determines the interface position based on the density distribution.

![image](https://github.com/user-attachments/assets/e86a166e-29f9-416d-86e4-1b3971601f39)

###  2.  $\pi-\pi$ Stacking Analysis (```pi-pi-stacking.ipynb```)
-  Reads the bulk structure data (```bulk.tpr``` and ```bulk_pbc.gro```)
quantifies the number of **π–π stacking interactions** and classifies them into distinct stacking types based on geometric criteria. A π–π stacking interaction is defined by satisfying all of the following conditions:
    1. The angle between the normal vectors of the two planar segments is **less than 10°**.
    2. The interplanar distance ($D_{\pi\text{--}\pi}$) is **less than 15 Å**.
    3. Both the **horizontal** and **vertical distances** are **less than 5 Å**.
The definitions of $D_{\pi\text{--}\pi}$ and horizontal displacement are illustrated in the schematic diagram below.
![image](https://github.com/user-attachments/assets/1cfccffb-885e-40bf-840e-a99718516694)

-  Polymer segments are classified into 3 categories, and $\pi-\pi$ stacking interactions are identified based on pairwise combinations of these segments. The number of interactions for each stacking type and the total number of $\pi-\pi$ stackings are then calculated.
![image](https://github.com/user-attachments/assets/25160077-ec2a-44b4-99dc-9d2d07d34e6b)

-  Generates the **backbone structure** for validation of atom selection groups. (```backbone.gro```)
-  Outputs a **gro file** containing the identified $\pi-\pi$ stacking structures. (```PSO2-pipi.gro```)
-  Saves the stacking classification results in a **text file** for further analysis. (```PSO2-pipi.txt```)

###  3.  Functional Group's Solution Environment Analysis (```sol_RDF.ipynb```, ```sol_env.ipynb```)
-  Reads the `.tpr` and `.xtc` files from the `TRAJECTORY\` directory.
-  Selects atoms corresponding to **functional groups** for analysis. The functinal groups used for each polymer are defined as follows:
![image](https://github.com/user-attachments/assets/091acd77-1b2d-476b-bc1a-c2263021722f)
####    P-SO2 functionl groups
```
S-Main:  resname PT* and name S2
S-Side:  resname PT* and name S1 S3
2O:      resname P2O and name O3 O4
DBZ-SO2: resname PT* and name S5 O3 O4, resname P2O and name S1 O1 O2
TSO-SO2: resname PT* and name S4 O1 O2
```
####    P-2O functional groups
```
S-Main:  resname PT* and name S2
S-Side:  resname PT* and name S1 S3
SO2:     resname SO2 and name S2 O3 O4, resname SO2 and name S3 O5 O6
DBZ-SO2: resname PT* and name S5 O3 O4, resname SO2 and name S1 O1 O2
TSO-SO2: resname PT* and name S4 O1 O2
```
####    PTO functional groups
```
S-Main:  resname PT* and name S2
S-Side:  resname PT* and name S1 S3
DBZ-SO2: resname PT* and name S5 O3 O4
TSO-SO2: resname PT* and name S4 O1 O2
```
-  Calculates the center of mass**(COM)** of each functional group and of the solution molecules.
-  Uses a **cutoff radius of 7.5Å to count solution molecules near each functional group:
```python
indices, distances = capped_distance(target_atoms, mol_com, max_cutoff=7.5, box=u.dimensions)
```
-  Computes the **volume fraction** of different solution molecules near functional groups and averages the results over the trajectory.
![image](https://github.com/user-attachments/assets/1a349bda-7bfd-4861-a8c5-edf6ee4a650c)
-  Plots the **RDF** of solution moleucules around functional groups.
![image](https://github.com/user-attachments/assets/c2f0d9f7-da5f-4f5d-a33d-60881f41a62b)

###  4.  Water Penetration Depth Analysis (```water_depth.ipynb```)
-  Reads the `.tpr` and `.gro` files from the `SOLUTION/` directory and computes the **water density profile** for solution systems containing three different polymers.
-  Fits the water density profile using an **error function (erf)** curve:
```python
def error_function(x, A, g, x0):
    return A * 0.5 * (special.erf(g * (x - x0) ) + 1)
```
-    Determines the **water interface positions** by solving the fitted curve for the points corresponding to **1%** and **99%** of the maximum water density, which represents the interface positions:
```python
def f(x, *p):
    return p[0] * 0.5 * (special.erf(p[1] * (x - p[2])) + 1) - p[0]*0.01
p0 = [310, 250,  250]
root = fsolve(f, x0 = p0[n], args = p)- p[2]
```
-  Generates a comparative plot of water penetration depth and water density profiles across the three polymers.
![image](https://github.com/user-attachments/assets/321526ca-708d-45c8-a2f4-a5e0f24d86ec)

