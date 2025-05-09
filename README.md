#  Solution-Mixture-Analysis
This repository contains analysis scripts related to the methods described in our recent paper. The provided codes facilitate the analysis of polymer bulk structures and solution-phase behavior.

The 3 conjugated polymers (P-2O, P-SO2, PBDTTSO) invested here are the materials for organic hydrogen evolution reaction, as we mentioned in the paper.

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
-  Reads the **tpr** and **gro** files of the solution structure.
-  Computes the density profile of **polymer, AA, NMP, and water**.
-  Determines the interface position based on the density distribution.
###  2.  $\pi-\pi$ Stacking Analysis (```pi-pi-stacking.ipynb```)
-  Reads the bulk structure data (```bulk.tpr``` and ```bulk_pbc.gro```)
-  Analyzes the number of **$\pi-\pi$ stacking interactioins** and classifies them into different stacking types.
-  Generates the **backbone structure** for validation of atom selection groups. (```backbone.gro```)
-  Outputs a **gro file** containing the identified $\pi-\pi$ stacking structures. (```PSO2-pipi.gro```)
-  Saves the stacking classification results in a **text file** for further analysis. (```PSO2-pipi.txt```)
###  3.  Functional Group's Solution Environment Analysis (```sol_RDF.ipynb```, ```sol_env.ipynb```)
-  Reads the **tpr** and **xtc** files from the trajectory.
-  Selects atoms corresponding to **functional groups** for analysis.
-  Computes the **volume fraction** of different solution molecules near functional groups and averages the results over the trajectory.
-  Plots the **RDF** of solution moleucules around functional groups.
###  4.  Water Penetration Depth Analysis (```water_depth.ipynb```)
-  Computes the **water density profile** for solutioin systems of three different polymers.
-  Determines the **water interface positions** and generates a comparative plot of water penetration depth.

