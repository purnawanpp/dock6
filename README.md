A.	Preparasi Receptor
1.	Semua file hasil simulasi dan script dapat didownload pada link berikut: 
https://github.com/purnawanpp/dock6_2nnq 
2.	Baca artikel yang terkait dengan file Protein Data Bank (PDB) untuk memahami status protonasi, muatan, kondisi lingkungan, dan informasi penting lainnya mengenai reseptor dan ligan.
3.	Buka file pdb menggunakan perangkat lunak chimera dan lihat strukturnya. Identifikasi komponen dari protein tersebut seperti reseptor, ligan, pelarut, surfaktan, dan ion logam
4.	Buka file PDB (2NNQ.pdb) menggunakan Chimera
5.	Pisahkan Ligand dan receptornya dengan select>residue >all non standard
6.	Klik Actions>atom/Bonds>delete
7.	Simpan reseptor sebagai 2nnq_receptor.pdb klik file>save pdb> Pilih lokasi selanjutnya save pdb.
8.	Selanjutnya simpan lagi file, Simpan reseptor sebagai file mol2. Simpan dengan nama 2nnq_rec_noH.mol2
9.	Klik File Close Session Buka kembali file 2nnq_rec_noH.mol2 menggunakan Chimera dan gunakan instruksi berikut untuk menyiapkan file reseptor yang akan digunakan di DOCK.
10.	Klik Tools -> Structure Editing> Klik Dock Prep -> Secara otomatis akan ditambahkan Tambahkan H (Untuk menambahkan atom Hidrogen) dan muatan (Untuk menambahkan muatan, gunakan medan gaya AMBER terbaru untuk residu standar. Di tutorial kali ini menggunakan menggunakan AMBER ff14SB)
11.	Pada Dock Prep Langsung klik “Ok” Selanjutnya 
12.	Pada Add Hydrogen langsung klik “Ok”





























13.	Pilih sesuai pilihan diatas selanjutnya pada Assign Charges for Dock Prep Other Residue Pilih Gasteiger lalu centang Standard Residues, klik “OK”
14.	Klik File Simpan sebagai file mol2. dengan nama (2nnq_rec_withH.mol2)











B.	Preparasi Ligand

1.	Buka lagi file 2nnq.pdb
2.	Klik file > open 2nnq
3.	Klik Select> Residue pilih residu T4B
4.	Klik Select> Klik Invert (Selected models)
5.	Klik Actions>atom/Bonds>delete
6.	Klik Tools> Structure Editing >Klik Dock Prep
7.	Klik Tools -> Structure Editing> Klik Dock Prep -> Secara otomatis akan ditambahkan Tambahkan H (Untuk menambahkan atom Hidrogen) dan muatan (Untuk menambahkan muatan, gunakan medan gaya AMBER terbaru untuk residu standar. Di tutorial kali ini menggunakan menggunakan AMBER ff14SB)
8.	Pada Dock Prep Langsung klik “Ok” Selanjutnya 
9.	Pada Add Hydrogen langsung klik “Ok”



1.	 









10.	Pada bagian label jangan dicentang
11.	Pada bagian specipy net charge pilih -1 klik ok, pastikan charge method nya adalah Gastaiger
12.	Simpan dengan format .mol2 dengan nama 2nnq_lig_withH.mol2 
13.	Klik file close session






 
BAB Preparasi Input dan Simulasi Docking 
A.	Pembuatan receptor surface and spheres (Bola Receptor)
1.	Buka file 2nnq_rec_noH.mol2 menggunakan chimera
2.	Klik Action -> Surface -> Show
3.	Klik Tools -> Structure Editing -> Write DMS
4.	Simpan dengan nama file: 2nnq_rec_noH.dms
5.	Pastikan file 2nnq_rec_noH.dms ada dalam folder kerja
6.	Buat File dengan Nama INSPH dengan cara klik kanan New Text Document.txt lalu rename menjadi INSPH dalam file INSPH berikan perintah berikut: 

2nnq_rec_noH.dms
R
X
0.0
4.0
1.4
2nnq_rec.sph









7.	Buka terminal Ubuntu pastikan terminal ubuntu telah membaca directory anda. Ketik Perintah Berikut pada Terminal
sphgen -i INSPH -o OUTSPH
8.	Jika terjadi eror baca pesan eror tersebut dan hapus file yang exist
9.	Di sini selanjutnya akan dipilih spheres yang merupakan kantong pengikat ligan, kita akan mencoba mengarahkan ligan ke tempat pengikatan ke reseptor. Untuk memilih mengatur sphres tersebut ketik perintah berikut pada terminal: 
sphere_selector 2nnq_rec.sph 2nnq_lig_withH.mol2 10.0

B.	Pembuatan Box
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi showbox.in, dimana file tersebut berisi script:
Y
 8.0
 selected_spheres.sph
 1
 2nnq.box.pdb








2.	Arti script diatas adalah Kita akan membuat kotak dengan panjang persegi 8 Angstrom, Gunakan file selected_spheres di lokasi folder, nama file output pdb yang berisi kotak yang dihasilkan adalah 2nnq.box.pdb
3.	Pada terminal ketikan perintah berikut:
showbox < showbox.in
4.	Jika langkah ini berhasil, Anda akan melihat file baru dengan nama 2nnq.box.pdb

C.	Pembuatan Grid
1.	Pastikan file flex.defn, flex_drive.tbl, dan vdw_AMBER_parm99.defn ada dalam folder kerja, file tersebut dapat didownload pada link berikut: https://github.com/purnawanpp/dock6_2nnq
2.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi grid.in, dimana file tersebut berisi script:

compute_grids                                yes
grid_spacing                                   0.4
output_molecule                             no
contact_score                                 no
energy_score                                  yes
energy_cutoff_distance                   9999
atom_model                                    a
attractive_exponent                        6
repulsive_exponent                       12
distance_dielectric                       yes
dielectric_factor                            4
bump_filter                                   yes
bump_overlap                              0.75
receptor_file                                 2nnq_rec_withH.mol2
box_file                                        2nnq.box.pdb
vdw_definition_file                       vdw_AMBER_parm99.defn
score_grid_prefix                         grid

3.	Pada terminal ketikan perintah berikut:
grid -i grid.in -o gridinfo.out
4.	Jika perintah berhasil, tiga file baru akan dihasilkan seperti (gridinfo.out, grid.nrg, grid.bmp). Buka file gridinfo.out untuk memastikan semua informasi tentang reseptor dalam file sesuai dengan informasi asli dari reseptor. (Misalnya: Total muatan, residu dan muatannya) Jika informasi tidak cocok, itu berarti Anda telah melakukan kesalahan pada salah satu langkah yang Anda ikuti sejauh ini.

D.	Minimisasi Energi
Sebelum melakukan docking, ligan akan mengalami minimisasi energi untuk menghilangkan benturan atom yang tidak menguntungkan. Benturan ini akan mempengaruhi rigid docking karena pada rigid docking ligan akan ditambatkan sebagai ligan yang lengkap, sedangkan pada metode flexible docking dan fixed anchor docking ligan akan dipecah menjadi fragmen dan ligan akan dibangun perbagian dengan mempertimbangkan orientasi dan sudut putar.
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi min.in, dimana file tersebut berisi script:

conformer_search_type                                    rigid
use_internal_energy                                         yes
internal_energy_rep_exp                                 12
internal_energy_cutoff                                     100.0
ligand_atom_file                                              2nnq_lig_withH.mol2
limit_max_ligands                                           no
skip_molecule                                                  no
read_mol_solvation                                          no
calculate_rmsd                                                 yes
use_rmsd_reference_mol                                 yes
rmsd_reference_filename                                2nnq_lig_withH.mol2
use_database_filter                                          no
orient_ligand                                                    no
bump_filter                                                      no
score_molecules                                               yes
contact_score_primary                                     no
contact_score_secondary                                  no
grid_score_primary                                           yes
grid_score_secondary                                       no
grid_score_rep_rad_scale                                 1
grid_score_vdw_scale                                       1
grid_score_es_scale                                          1
grid_score_grid_prefix                                      grid
multigrid_score_secondary                                no
dock3.5_score_secondary                                  no
continuous_score_secondary                              no
footprint_similarity_score_secondary                no
pharmacophore_score_secondary                       no
descriptor_score_secondary                                no
gbsa_zou_score_secondary                                 no
gbsa_hawkins_score_secondary                         no
SASA_score_secondary                                      no
amber_score_secondary                                       no
minimize_ligand                                                  yes
simplex_max_iterations                                      1000
simplex_tors_premin_iterations                          0
simplex_max_cycles                                           1
simplex_score_converge                                     0.1
simplex_cycle_converge                                     1.0
simplex_trans_step                                              1.0
simplex_rot_step                                                 0.1
simplex_tors_step                                               10.0
simplex_random_seed                                        0
simplex_restraint_min                                        yes
simplex_coefficient_restraint                             10.0
atom_model                                                        all
vdw_defn_file                                                    vdw_AMBER_parm99.defn
flex_defn_file                                                    flex.defn
flex_drive_file                                                   flex_drive.tbl
ligand_outfile_prefix                                         2nnq.lig.min
write_orientations                                              no
num_scored_conformers                                    1
rank_ligands                                                	     no

2.	Pada terminal ketikan perintah berikut:
dock6 -i min.in
 
3.	Jika prosesnya berhasil, file baru dengan nama 2nnq.lig.min_scored.mol2.






E. Simulasi Rigid Docking
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi rigid.in, dimana file tersebut berisi script:

conformer_search_type                                     rigid
use_internal_energy                                          yes
internal_energy_rep_exp                                   12
internal_energy_cutoff                                      100.0
ligand_atom_file                                               2nnq.lig.min_scored.mol2
limit_max_ligands                                            no
skip_molecule                                                   no
read_mol_solvation                                           no
calculate_rmsd                                                  yes
use_rmsd_reference_mol                                  yes
rmsd_reference_filename                                  2nnq.lig.min_scored.mol2
use_database_filter                                            no
orient_ligand                                                     yes
automated_matching                                         yes
receptor_site_file                                               selected_spheres.sph
max_orientations                                               1000
critical_points                                                     no
chemical_matching                                            no
use_ligand_spheres                                            no
bump_filter                                                        no
score_molecules                                                yes
contact_score_primary                                      no
contact_score_secondary                                   no
grid_score_primary                                           yes
grid_score_secondary                                        no
grid_score_rep_rad_scale                                   1
grid_score_vdw_scale                                        1
grid_score_es_scale                                           1
grid_score_grid_prefix                                       grid
multigrid_score_secondary                                no
dock3.5_score_secondary                                  no
continuous_score_secondary                             no
footprint_similarity_score_secondary               no
pharmacophore_score_secondary                     no
descriptor_score_secondary                              no
gbsa_zou_score_secondary                               no
gbsa_hawkins_score_secondary                       no
SASA_score_secondary                                   no
amber_score_secondary                                   no
minimize_ligand                                              yes
simplex_max_iterations                                   1000
simplex_tors_premin_iterations                        0
simplex_max_cycles                                         1
simplex_score_converge                                  0.1
simplex_cycle_converge                                  1.0
simplex_trans_step                                           1.0
simplex_rot_step                                              0.1
simplex_tors_step                                            10.0
simplex_random_seed                                     0
simplex_restraint_min                                    no
atom_model                                                    all
vdw_defn_file                                                 vdw_AMBER_parm99.defn
flex_defn_file                                                 flex.defn
flex_drive_file                                                flex_drive.tbl
ligand_outfile_prefix                                      rigid.out
write_orientations                                           no
num_scored_conformers                                 1
rank_ligands                                                    no

2.	Pada terminal ketikan perintah berikut:
dock6 -i rigid.in
jika proses sukses akan keluar hasil berikut:

 


3.	Setelah Simulasi Rigid Docking berhasil, file output baru yaitu rigid.out_scored.mol2 Visualisasikan file output ini menggunakan Chimera dengan mengikuti langkah-langkah untuk memeriksa keberhasilan proses docking
4.	Klik File> open -> 2nnq_rec_withH.mol2 File 
5.	Klik open -> 2nnq_lig_withH.mol2
6.	Klik Tools -> Surface/binding Analysis -> ViewDock -> Pilih file  rigid.out_scored.mol2
7.	Pilih “Dock4,5 or 6” klik OK
8.	Untuk mengatur tampilan dapat mengklik column>show>Pilih yang ingin ditampilkan












F.	Fixed Anchor Docking

1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi fixed.in, dimana file tersebut berisi script:
conformer_search_type                                        flex
write_fragment_libraries                                     no
user_specified_anchor                                         no
limit_max_anchors                                              no
min_anchor_size                                                  5
pruning_use_clustering                                       yes
pruning_max_orients                                          1000
pruning_clustering_cutoff                                    100
pruning_conformer_score_cutoff                         100.0
pruning_conformer_score_scaling_factor           1.0
use_clash_overlap                                                no
write_growth_tree                                                no
use_internal_energy                                             yes
internal_energy_rep_exp                                      12
internal_energy_cutoff                                         100.0
ligand_atom_file                                                  2nnq_lig_withH.mol2
limit_max_ligands                                                no
skip_molecule                                                       no
read_mol_solvation                                              no
calculate_rmsd                                                     yes
use_rmsd_reference_mol                                     yes
rmsd_reference_filename                                    2nnq_lig_withH.mol2
use_database_filter                                             no
orient_ligand                                                       no
bump_filter                                                          no
score_molecules                                                  yes
contact_score_primary                                        no
contact_score_secondary                                    no
grid_score_primary                                             yes
grid_score_secondary                                         no
grid_score_rep_rad_scale                                   1
grid_score_vdw_scale                                         1
grid_score_es_scale                                            1
grid_score_grid_prefix                                       grid
multigrid_score_secondary                                 no
dock3.5_score_secondary                                    no
continuous_score_secondary                               no
footprint_similarity_score_secondary                  no
pharmacophore_score_secondary                        no
descriptor_score_secondary                                  no
gbsa_zou_score_secondary                                   no
gbsa_hawkins_score_secondary                           no
SASA_score_secondary                                         no
amber_score_secondary                                        no
minimize_ligand                                                    yes
minimize_anchor                                                   yes
minimize_flexible_growth                                     yes
use_advanced_simplex_parameters                     no
simplex_max_cycles                                              1
simplex_score_converge                                       0.1
simplex_cycle_converge                                       1.0
simplex_trans_step                                               1.0
simplex_rot_step                                                   0.1
simplex_tors_step                                            10.0
simplex_anchor_max_iterations                      500
simplex_grow_max_iterations                          500
simplex_grow_tors_premin_iterations                0
simplex_random_seed                                          0
simplex_restraint_min                                        no
atom_model                                                        all
vdw_defn_file                                                    vdw_AMBER_parm99.defn
flex_defn_file                                               	flex.defn
flex_drive_file                                              	flex_drive.tbl
ligand_outfile_prefix                                       2nnq_fad
write_orientations                                           no
num_scored_conformers                                100
write_conformations                                        no
cluster_conformations                                     yes
cluster_rmsd_threshold                                  2.0
rank_ligands                                                   no

2.	Pada terminal ketikan perintah berikut:
dock6 -i fixed.in
jika selesai akan menghasilkan score seperti berikut:




3.	Setelah docking selesai file output akan dihasilkan file berikut: 2nnq_fad_scored.mol2
4.	Klik File> open -> 2nnq_rec_withH.mol2 File 
5.	Klik open -> 2nnq_lig_withH.mol2
6.	Klik Tools -> Surface/binding Analysis -> ViewDock -> Pilih file 2nnq_fad_scored.mol2
7.	Pilih “Dock4,5 or 6” klik OK
8.	Untuk mengatur tampilan dapat mengklik column>show>Pilih yang ingin ditampilkan
9.	Perhatikan semua pose 50 yang dihasilkan berada di cluster yang sama dan RMSD standar adalah 0,649. Nilai RMSD yang bagus adalah <2. Ini menunjukkan bahwa docking telah berhasil
10.	Nilai terbaik yaitu nilai yang paling minimal 
 











F.	Flexible Docking dan perhitungan perhitungan Molecular Mechanics Generalized Born Surface Area (MM-GBSA)

1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi flex.in, dimana file tersebut berisi script:
2.	Copy file 2nnq_rec_withH.mol2 lalu paste kembali di folder kerja, ubah nama file tersebut menjadi receptor.mol2

conformer_search_type                                        flex
write_fragment_libraries                                      no
user_specified_anchor                                          no
limit_max_anchors                                               no
min_anchor_size                                                   5
pruning_use_clustering                                        yes
pruning_max_orients                                           1000
pruning_clustering_cutoff                                    100
pruning_conformer_score_cutoff                         100.0
pruning_conformer_score_scaling_factor           1.0
use_clash_overlap                                                no
write_growth_tree                                                no
use_internal_energy                                             yes
internal_energy_rep_exp                                      12
internal_energy_cutoff                                          100.0
ligand_atom_file                                              2nnq.lig.min_scored.mol2
limit_max_ligands                                            no
skip_molecule                                                   no
read_mol_solvation                                          no
calculate_rmsd                                                 yes
use_rmsd_reference_mol                                  yes
rmsd_reference_filename                                 2nnq.lig.min_scored.mol2
use_database_filter                                          no
orient_ligand                                                    yes
automated_matching                                         yes
receptor_site_file                                              selected_spheres.sph
max_orientations                                              1000
critical_points                                                   no
chemical_matching                                            no
use_ligand_spheres                                           no
bump_filter                                                       no
score_molecules                                               yes
contact_score_primary                                     no
contact_score_secondary                                  no
grid_score_primary                                           yes
grid_score_secondary                                        no
grid_score_rep_rad_scale                                 1
grid_score_vdw_scale                                       1
grid_score_es_scale                                          1
grid_score_grid_prefix                                     grid
multigrid_score_secondary                               no
dock3.5_score_secondary                                 no
continuous_score_secondary                            no
footprint_similarity_score_secondary              no
pharmacophore_score_secondary                    no
descriptor_score_secondary                             no
gbsa_zou_score_secondary                               no
gbsa_hawkins_score_secondary                       yes
SASA_score_secondary                                      no
amber_score_secondary                                     no
minimize_ligand                                                 yes
minimize_anchor                                                yes
minimize_flexible_growth                                   yes
use_advanced_simplex_parameters                    no
simplex_max_cycles                                            1
simplex_score_converge                                      0.1
simplex_cycle_converge                                       1.0
simplex_trans_step                                              1.0
simplex_rot_step                                                 0.1
simplex_tors_step                                               10.0
simplex_anchor_max_iterations                        500
simplex_grow_max_iterations                             500
simplex_grow_tors_premin_iterations                 0
simplex_random_seed                                          0
simplex_restraint_min                                         no
atom_model                                                         all
vdw_defn_file                                                      vdw_AMBER_parm99.defn
flex_defn_file                                                      flex.defn
flex_drive_file                                              	      flex_drive.tbl
ligand_outfile_prefix                                          flex.out
write_orientations                                              no
num_scored_conformers                                    1
rank_ligands                                                      no

3.	Pada terminal ketikan perintah berikut:
dock6 -i flex.in
4.	Ketikan perintah masing-masing sesuai dalam kurung secara manual [    ] pada terminal lalu enter. 






5.	Hasil simulasi berikut dibawah ini, nilai Docking/Grid score yaitu -57.144726: 















6.	Nilai keseluruhan GBSA yaitu GBSA Score -38.356567, nilai GBSA Energy Van der walls -53.421894, Energi elektrostatik -96.454689, Energy Generalized Born yaitu 118.059814 dan energy surface area yaitu -6.539795

G.	Docking Senyawa bahan alam dengan protein 2nnq

