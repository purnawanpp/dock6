# Tutorial Simulasi Molecular Docking Menggunakan DOCK6 By Purnawan Pontana Putra
Video Tutorial Dapat dilihat disini: 
1. Part 1: https://youtu.be/m4ZI-UUwoyk
2. Part 2: https://youtu.be/3y_uxZAhbqo
3. Part 3: https://youtu.be/Eefni6cik04
3. Part 4: https://youtu.be/-X_WOuCzRCs

# Part 1 Preparasi Receptor dan Ligand Menggunakan Chimera
**A. Receptor Preparation**
1. All simulation results and scripts can be downloaded at the following link: https://github.com/purnawanpp/dock6_2nnq
2. Please read articles related to the Protein Data Bank (PDB) file to understand information about protonation status, charges, environmental conditions, and other important details about the receptor and ligand.
3. Use Chimera software to open the PDB file and examine its structure. Identify components of the protein, such as the receptor, ligand, solvent, surfactant, and metal ions.
4. Open the PDB file (2NNQ.pdb) using Chimera.
5. Separate the ligand and receptor by selecting "select>residue>all non-standard."
6. Click "Actions>atom/Bonds>delete."
7. Save the receptor as 2nnq_receptor.pdb by clicking "file>save pdb" and choose the location to save it.
8. Next, save the file again as a mol2 file. Save it with the name 2nnq_rec_noH.mol2.
9. Close the file session by clicking "File Close Session." Then, reopen the file 2nnq_rec_noH.mol2 using Chimera and follow the instructions below to prepare the receptor file for use in DOCK.
10. Click "Tools -> Structure Editing" and select "Dock Prep." Hydrogen atoms will be automatically added, and charges will be assigned (use the latest AMBER force field for non-standard residues).
11. Click "Ok" for the Dock Prep process.
12. Click "Ok" again for the Add Hydrogen process.
13. Select the options as described above, and for "Assign Charges for Dock Prep Other Residue," choose Gasteiger and check "Standard Residues." Click "OK."
14. Click "File" and save the file as a mol2 file with the name (2nnq_rec_withH.mol2).


**B.	Ligand Preparation**
1. Open the 2nnq.pdb file again.
2. Select "file > open" and open the 2nnq file.
3. Choose "Select > Residue" and select the T4B residue.
4. After that, select "Select" and click "Invert (Selected models)."
5. Select "Actions > Atom/Bonds > Delete."
6. Navigate to "Tools > Structure Editing" and click "Dock Prep."
7. Under "Tools > Structure Editing," select "Dock Prep." This will automatically add hydrogen atoms and charges. To add charges, use the latest AMBER force field for standard residues. In this tutorial, we are using the General Amber Force Field (GAFF).
8. In the Dock Prep process, simply click "Ok."
9. In the Add Hydrogen process, click "Ok" directly.
10. Do not check the options in the label section.
11. In the "Specify Net Charge" section, select -1 and click "Ok." Make sure the charge method is Gasteiger.
12. Save the file in .mol2 format and name it 2nnq_lig_withH.mol2.
13. Finally, click "File" and select "Close Session."

 
# Part 2: Input File Preparation and Docking Simulation
**A.	Making receptor surfaces and spheres (Bola Receptor)**
1. Go to the working folder and reopen the file 2nnq_rec_noH.mol2 using the Chimera software.
2. Click on "Action -> Surface -> Show."
3. Click on "Tools -> Structure Editing -> Write DMS."
4. Save it with the filename: 2nnq_rec_noH.dms.
5. Ensure that the file 2nnq_rec_noH.dms is in the working folder.
6. Create a file named INSPH. You can see the file here: https://github.com/purnawanpp/dock6_2nnq/blob/main/INSPH
7. Open the terminal again, make sure you're using Ubuntu, and double-check your terminal to ensure it's in the correct directory. Enter the following command in the terminal:
`sphgen -i INSPH -o OUTSPH`
8. Errors may occur; don't panic. Read the error message and delete any existing files as needed.
9. To select spheres that represent the binding pocket where the ligand binds to the receptor, use the following command in the terminal:
`sphere_selector 2nnq_rec.sph 2nnq_lig_withH.mol2 10.0`

**B.	Making a Box**
1. Create a text document file by right-clicking, then rename the file from Nex Text Document.txt to showbox.in. You can find the file here: https://github.com/purnawanpp/dock6_2nnq/blob/main/showbox.in
2. showbox.in is a script to create a cubic box with a size of 8 Angstroms. Use the selected_spheres file located in the folder. The output file in pdb format containing the generated box will be named 2nnq.box.pdb.
3. In the terminal, type the following command: `showbox < showbox.in`
4. If this step is successful, you will see a new file named 2nnq.box.pdb.

**C.	Making a Grid**
1. Check again that the flex.defn, flex_drive.tbl, and vdw_AMBER_parm99.defn files are in the working folder, these files can be downloaded at the following link: https://github.com/purnawanpp/dock6_2nnq
2. Create a new file by right clicking then selecting Nex Text Document.txt change the file to *grid.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/grid.in
3. Type the following command in the terminal: `grid -i grid.in -o gridinfo.out`
4. If the command is successful, three new files will be created, namely gridinfo.out, grid.nrg, and grid.bmp. Open the gridinfo.out file to verify that all receptor-related information in the file matches the original information of the receptor (such as total charges, residues, and individual charges). If the information does not match, it indicates that you may have made a mistake in one of the steps you have followed so far.

**D. Energy Minimization**
1. Before starting the docking process, the initial step is to minimize the energy of the ligand to overcome unwanted atomic conflicts. This conflict will affect different types of docking, such as rigid docking, where the ligand remains intact, as well as flexible docking methods and docking with fixed anchors, where the ligand will be divided into fragments and assembled taking into account orientation and rotation.
2. Create a new file by right clicking then selecting Nex Text Document.txt change the file to *min.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/min.in
3. In the terminal type the following command: `dock6 -i min.in`
4. If the process is successful, a new file with the name 2nnq.lig.min_scored.mol2.

**E. Simulation Rigid Docking**
1. Open the work folder, create a new file by right-clicking with the mouse then selecting Nex Text Document.txt, edit the file name to *rigid.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/rigid. in
2. In the terminal type the following command: `dock6 -i rigid.in`
3. After the command above, Rigid Docking is successful, the new output file is rigid.out_scored.mol2, open this output file using Chimera by following the steps to see whether the process carried out has been successful. The docking process was successful.
4. Click File> open -> 2nnq_rec_withH.mol2 File
5. Click the open section -> 2nnq_lig_withH.mol2
6. Click the Tools section -> Surface/binding Analysis -> ViewDock -> Select the file rigid.out_scored.mol2
7. Select "Dock4,5 or 6" click OK
8. Visualization with an attractive appearance can be done by clicking column>show>Select what you want to display

**F.	Fixed Anchor Docking**
1. Create a new file in the work folder by right-clicking then selecting Nex Text Document.txt, change the file to *fixed.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/fixed.in
2. In the Ubuntu terminal, type the following command: `dock6 -i fixed.in`
3. The docking process is complete. The output file will produce the following file: 2nnq_fad_scored.mol2
4. Click the File section> open -> 2nnq_rec_withH.mol2 File
5. Click the open section -> 2nnq_lig_withH.mol2
6. Click the Tools section -> Surface/binding Analysis -> ViewDock -> Select the file 2nnq_fad_scored.mol2
7. Select "Dock4,5 or 6" click OK
8. Interesting views can be selected by clicking column>show>Select what you want to display
9. Note that all the 50 resulting shapes (poses) are in the same cluster and the standard RMSD is 0.649. A good RMSD value is <2. This shows that the docking software is capable of carrying out simulations similar to the crystallography results and is categorized as valid
10. The best value from the docking results is the minimum value

**G.	Flexible Docking dan perhitungan perhitungan Molecular Mechanics Generalized Born Surface Area (MM-GBSA)**
1. Create a new file by right clicking then selecting Nex Text Document.txt change the file to *flex.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/flex.in
2. Copy the file 2nnq_rec_withH.mol2 then paste it back into the working folder, change the file name to receptor.mol2
3. In the terminal type the following command: `dock6 -i flex.in`
4. Manually type the respective commands in brackets [ ] in the terminal then enter.
5. The following simulation results are below, the Docking/Grid score value is -57.144726:
6. The overall value of GBSA is GBSA Score -38.356567, GBSA Energy Van der Walls value is -53.421894, Electrostatic Energy is -96.454689, Generalized Born Energy is 118.059814 and surface area energy is -6.539795
   
**H. Installation of software for docking virtual screening results or natural materials**
1. Install Gnina by downloading the software at the following link: https://github.com/gnina/gnina/releases/download/v1.0.2/gnina
2. Install open babel with the command in the Linux terminal: `sudo apt-get install -y openbabel`
3. Install vina_split with the command in the Linux terminal: `sudo apt-get install -y autodock-vina`
4. Create a Gnina file path in the Linux terminal

**I. Docking Simulation with Virtual Screening Results Compounds or natural materials**
1. Copy the previous working folder and paste it in the same place, name the folder as needed
2. Draw the molecular structure first using Marvin Sketch, an example of the structure drawn is **curcumin** This structure text can be copied on notepad and pasted directly into Marvin Sketch software
3. Save the structure file in the .pdb file format with the file name: *example.pdb* click yes or ok
4. Structures can also be drawn manually
5. Run Gnina software to obtain ligands with X,Y and Z coordinates
6. Download the 2nnq PDB here https://files.rcsb.org/download/2NNQ.pdb and separate the protein and ligand with the commands in the Linux terminal one by one:
`grep ATOM 2nnq.pdb > rec.pdb` next
`grep T4B 2nnq.pdb > lig.pdb`
7. Run the following command in one terminal line:
`gnina -r rec.pdb -l example.pdb --autobox_ligand lig.pdb -o docked.sdf --seed 0`
8. Run the following command to change the file format:
`obabel docked.sdf -O docked.pdbqt*`
9. Next, extract the file using vina_split with the command:
`vina_split --input docked.pdbqt`
10. Next you will get a file with the name docked_ligand_1.pdbqt then change the name and format of the file to mol2 format with the command:
`obabel docked_ligand_1.pdbqt -O 2nnq_lig_withH.mol2`
11. Open the 2nnq_lig_withH.mol2 file using Avogadro
12. Perform geometry optimization using Avogadro
13. Click File, find the working folder, open the file 2nnq_lig_withH.mol2
14. Add hydrogen using Build, add Hydrogen
15. Perform geometry optimization with Avogadro by clicking Extensions, Molecular Mechanics, Setup Force Field, select GAFF (General Amber Force Field)
16. Next, click Extensions, click optimize geometry
17. Click the next file, save as, make sure the file format is .mol2 and overwrite the previous file or you can immediately click save
18. The docking process is carried out the same as in step # Part 2: Input File Preparation and Docking Simulation in Part A point 9 until the Box, Grid, Minimization, Rigid docking and Flexible docking are created

**J. Footprint analysis of minimization results**
1. Download the files footprint.in https://github.com/purnawanpp/dock6_2nnq/blob/main/footprint.in and plot.py https://github.com/purnawanpp/dock6_2nnq/blob/main/plot.py and save it in the work folder
2. Run the following command: `dock6 -i footprint.in`
3. Next run the following command: `python plot.py fps.min.output_footprint_scored.txt 50`
4. You will get a file in the form of file: https://github.com/purnawanpp/dock6_2nnq/blob/main/fps.min.output_footprint_scored.txt.pdf
5. The results will be different if you use the flexible docking footprint

## Notes
This tutorial is suitable for the DOCK6.9 version, there are many updates to DOCK6.10, especially GBSA calculations which can only be done in a rigid state. An example of a GBSA calculation script on DOCK6.10 can be read here: https://github.com/purnawanpp/dock6_2nnq/blob/main/hawkins_gbsa1.dockin

# Reference
1. https://ringo.ams.stonybrook.edu/index.php/2018_DOCK_tutorial_1_with_PDBID_2NNQ#VI._Virtual_Screen
