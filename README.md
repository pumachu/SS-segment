# SS-segment
This repository contains analysis scripts related to the methods described in our recent paper. The provided codes facilitate the analysis of polymer bulk structures and solution-phase behavior.

##  Project Structure
-  ```Analysis_and_run/```: Contains all analysis scripts.
-  ```bulk/```: Includes an example of bulk structure analysis using the P-SO2 polymer bulk system.
-  ```solution/```: Provides three polymer solution systems as examples for solution-phase analysis.
-  ```trajectory/```: Contains the trajectory files for the **NPT** simulation of the P-2O polymer solution

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

