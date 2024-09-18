# README

## Overview
This workflow is designed to process VASP (Vienna Ab initio Simulation Package) relaxed jobs and modify the resulting `CONTCAR` file into a lower triangular structure. Several scripts are used to convert VASP files for LAMMPS, replace atom types based on folder names, and apply strain for compression and tension analysis.

## Workflow Instructions

### Step 1: Convert VASP Files to LAMMPS and Back

- After the relaxation job in VASP, you need to modify the `CONTCAR` into a lower triangular structure. Use the following command to copy necessary Python scripts to each folder and perform the conversions:

```bash
for i in */; do
    cp lammps_vasp.py vasp_lammps.py ${i};
    cd ${i};
    python3 vasp_lammps.py;
    python3 lammps_vasp.py;
    cd ..;
done
```
- This command will:

- Copy ```lammps_vasp.py``` and ```vasp_lammps.py``` into each subdirectory.
- Run the scripts in each subdirectory:
```vasp_lammps.py```: Converts VASP files to LAMMPS format.
```lammps_vasp.py```: Converts LAMMPS files back to VASP format.
### Step 2: Replace Atom Types
- Next, run the ```replace_types.py``` script to update atom types based on folder names:
```python3 replace_types.py```
- This script will ensure that the atom types in the POSCAR files are replaced with the corresponding folder names.

### Step 3: Cold Curve Strain Analysis
Run the ```coldcurve.py``` script to apply strain and create input directories for further VASP calculations:
```
python3 coldcurve.py
```
# The script will:
- Generate ```input_*``` folders inside each subdirectory.
- Each ```input_*``` folder contains VASP input files representing different strain conditions (compression and tension).
# Strain Parameters:
- Compression percentage: 41.43%
- Tension percentage: 48.22%
# Folder Structure
After running the workflow, the directory structure will resemble:

- present_directory/
    ├── 01_Ni_FCC/
    │   ├── all_POSCARS/
    │   │   ├── POSCAR_1
    │   │   ├── POSCAR_2
    │   │   └── POSCAR_3
    │   ├── input_1/
    │   │   ├── POSCAR (copied from POSCAR_1)
    │   │   ├── INCAR
    │   │   ├── KPOINTS
    │   │   └── POTCAR
    │   ├── input_2/
    │   ├── input_3/
    │   └── test_POSCAR
    └── 02_Ti_BCC/

- all_POSCARS/: Contains original ```POSCAR``` files from relaxed VASP calculations.
- input_1/, input_2/, input_3/: These directories contain strained versions of the system with corresponding VASP input files (```POSCAR, INCAR, KPOINTS, POTCAR```).
# Notes
- Review the input parameters in INCAR, KPOINTS, and POTCAR files to ensure they are appropriate for your simulations.
- The strain percentages for compression (41.43%) and tension (48.22%) are automatically applied, but you can modify the script for custom values.
