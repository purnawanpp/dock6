# Tutorial Simulasi Molecular Docking Menggunakan DOCK6
Video Tutorial Dapat dilihat disini: 
1. Part 1: https://youtu.be/m4ZI-UUwoyk
2. Part 2: https://youtu.be/3y_uxZAhbqo
3. Part 3: https://youtu.be/Eefni6cik04
3. Part 4: https://youtu.be/-X_WOuCzRCs

# Part 1 Preparasi Receptor dan Ligand Menggunakan Chimera
**A.	Preparasi Receptor**
1.	Semua hasil simulasi dan skrip dapat diunduh di tautan berikut: https://github.com/purnawanpp/dock6_2nnq
2.	Silakan baca artikel yang terkait dengan file Protein Data Bank (PDB) untuk memahami informasi mengenai status protonasi, muatan, kondisi lingkungan, dan detail penting lainnya tentang reseptor dan ligan.
3.	Gunakan perangkat lunak Chimera untuk membuka file PDB dan periksa strukturnya. Identifikasi komponen dari protein tersebut, seperti reseptor, ligan, pelarut, surfaktan, dan ion logam.
4.	Buka file PDB (2NNQ.pdb) dengan menggunakan Chimera.
5.	Pisahkan ligan dan reseptornya dengan memilih "select>residue>all non standard".
6.	Klik "Actions>atom/Bonds>delete".
7.	Simpan reseptor sebagai 2nnq_receptor.pdb dengan cara klik "file>save pdb" dan pilih lokasi untuk menyimpannya.
8.	Selanjutnya, simpan kembali file sebagai file mol2. Simpan dengan nama 2nnq_rec_noH.mol2.
9.	Tutup sesi file dengan mengklik "File Close Session". Kemudian, buka kembali file 2nnq_rec_noH.mol2 menggunakan Chimera dan ikuti petunjuk berikut untuk menyiapkan file reseptor yang akan digunakan dalam DOCK.
10.	Klik "Tools -> Structure Editing" dan pilih "Dock Prep". Secara otomatis, atom hidrogen akan ditambahkan, dan muatan akan diberikan (gunakan medan gaya AMBER terbaru untuk residu non standar).
11.	Klik "Ok" pada proses Dock Prep.
12.	Klik "Ok" lagi pada proses Add Hydrogen.
13.	Pilih opsi sesuai dengan yang dijelaskan di atas, lalu pada "Assign Charges for Dock Prep Other Residue," pilih Gasteiger dan centang "Standard Residues". Klik "OK".
14.	Klik "File" dan simpan file sebagai file mol2 dengan nama (2nnq_rec_withH.mol2).


**B.	Preparasi Ligand**
1.	Buka kembali file 2nnq.pdb.
2.	Pilih "file > open" dan buka file 2nnq.
3.	Pilih "Select > Residue" dan pilih residu T4B.
4.	Setelah itu, pilih "Select" dan klik "Invert (Selected models)".
5.	Pilih "Actions > Atom/Bonds > Delete".
6.	Navigasi ke "Tools > Structure Editing" dan klik "Dock Prep".
7.	Di bawah "Tools > Structure Editing", pilih "Dock Prep". Ini akan secara otomatis menambahkan atom hidrogen dan muatan. Untuk menambahkan muatan, gunakan medan gaya AMBER terbaru untuk residu standar. Dalam tutorial ini, kami menggunakan General Amber Force Field (GAFF).
8.	Dalam proses Dock Prep, cukup klik "Ok".
9.	Dalam proses Add Hydrogen, langsung klik "Ok".
10.	Jangan centang opsi pada bagian label.
11.	Pada bagian "Specify Net Charge," pilih -1 dan klik "Ok". Pastikan metode muatan adalah Gasteiger.
12.	Simpan file dengan format .mol2 dan beri nama 2nnq_lig_withH.mol2.
13.	Terakhir, klik "File" dan pilih "Close Session".

 
# Part 2: Preparasi Input File dan Simulasi Docking
**A.	Pembuatan receptor surface and spheres (Bola Receptor)**
1.	Buka file 2nnq_rec_noH.mol2 menggunakan chimera
2.	Klik Action -> Surface -> Show
3.	Klik Tools -> Structure Editing -> Write DMS
4.	Simpan dengan nama file: 2nnq_rec_noH.dms
5.	Pastikan file 2nnq_rec_noH.dms ada dalam folder kerja
6.	Buat File dengan Nama *INSPH* https://github.com/purnawanpp/dock6_2nnq/blob/main/INSPH
7.	Buka terminal Ubuntu pastikan terminal ubuntu telah membaca directory anda. Ketik Perintah Berikut pada Terminal: 
`sphgen -i INSPH -o OUTSPH`
9.	Jika terjadi eror baca pesan eror tersebut dan hapus file yang exist
10.	Di sini selanjutnya akan dipilih spheres yang merupakan kantong pengikat ligan, kita akan mencoba mengarahkan ligan ke tempat pengikatan ke reseptor. Untuk memilih mengatur sphres tersebut ketik perintah berikut pada terminal:
`sphere_selector 2nnq_rec.sph 2nnq_lig_withH.mol2 10.0`

**B.	Pembuatan Box**
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *showbox.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/showbox.in
2.	Arti script diatas adalah Kita akan membuat kotak dengan panjang persegi 8 Angstrom, Gunakan file selected_spheres di lokasi folder, nama file output pdb yang berisi kotak yang dihasilkan adalah 2nnq.box.pdb
3.	Pada terminal ketikan perintah berikut: `showbox < showbox.in`
4.	Jika langkah ini berhasil, Anda akan melihat file baru dengan nama 2nnq.box.pdb

**C.	Pembuatan Grid**
1.	Pastikan file flex.defn, flex_drive.tbl, dan vdw_AMBER_parm99.defn ada dalam folder kerja, file tersebut dapat didownload pada link berikut: https://github.com/purnawanpp/dock6_2nnq
2.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *grid.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/grid.in
3.	Pada terminal ketikan perintah berikut: `grid -i grid.in -o gridinfo.out`
4.	Jika perintah berhasil, tiga file baru akan dihasilkan seperti (gridinfo.out, grid.nrg, grid.bmp). Buka file gridinfo.out untuk memastikan semua informasi tentang reseptor dalam file sesuai dengan informasi asli dari reseptor. (Misalnya: Total muatan, residu dan muatannya) Jika informasi tidak cocok, itu berarti Anda telah melakukan kesalahan pada salah satu langkah yang Anda ikuti sejauh ini.

**D.	Minimisasi Energi**
1. Sebelum melakukan docking, ligan akan mengalami minimisasi energi untuk menghilangkan benturan atom yang tidak menguntungkan. Benturan ini akan mempengaruhi rigid docking karena pada rigid docking ligan akan ditambatkan sebagai ligan yang lengkap, sedangkan pada metode flexible docking dan fixed anchor docking ligan akan dipecah menjadi fragmen dan ligan akan dibangun perbagian dengan mempertimbangkan orientasi dan sudut putar.
2. Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *min.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/min.in
3.	Pada terminal ketikan perintah berikut: `dock6 -i min.in`
4.	Jika prosesnya berhasil, file baru dengan nama 2nnq.lig.min_scored.mol2.

**E. Simulasi Rigid Docking**
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *rigid.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/rigid.in
2.	Pada terminal ketikan perintah berikut: `dock6 -i rigid.in`
3.	Setelah Simulasi Rigid Docking berhasil, file output baru yaitu rigid.out_scored.mol2 Visualisasikan file output ini menggunakan Chimera dengan mengikuti langkah-langkah untuk memeriksa keberhasilan proses docking
4.	Klik File> open -> 2nnq_rec_withH.mol2 File 
5.	Klik open -> 2nnq_lig_withH.mol2
6.	Klik Tools -> Surface/binding Analysis -> ViewDock -> Pilih file  rigid.out_scored.mol2
7.	Pilih “Dock4,5 or 6” klik OK
8.	Untuk mengatur tampilan dapat mengklik column>show>Pilih yang ingin ditampilkan

**F.	Fixed Anchor Docking**
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *fixed.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/fixed.in
2.	Pada terminal ketikan perintah berikut: `dock6 -i fixed.in`
3.	Setelah docking selesai file output akan dihasilkan file berikut: 2nnq_fad_scored.mol2
4.	Klik File> open -> 2nnq_rec_withH.mol2 File 
5.	Klik open -> 2nnq_lig_withH.mol2
6.	Klik Tools -> Surface/binding Analysis -> ViewDock -> Pilih file 2nnq_fad_scored.mol2
7.	Pilih “Dock4,5 or 6” klik OK
8.	Untuk mengatur tampilan dapat mengklik column>show>Pilih yang ingin ditampilkan
9.	Perhatikan semua pose 50 yang dihasilkan berada di cluster yang sama dan RMSD standar adalah 0,649. Nilai RMSD yang bagus adalah <2. Ini menunjukkan bahwa docking telah berhasil
10.	Nilai terbaik yaitu nilai yang paling minimal

**G.	Flexible Docking dan perhitungan perhitungan Molecular Mechanics Generalized Born Surface Area (MM-GBSA)**
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *flex.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/flex.in
2.	Copy file 2nnq_rec_withH.mol2 lalu paste kembali di folder kerja, ubah nama file tersebut menjadi receptor.mol2
3.	Pada terminal ketikan perintah berikut: `dock6 -i flex.in`
4.	Ketikan perintah masing-masing sesuai dalam kurung secara manual [    ] pada terminal lalu enter. 
5.	Hasil simulasi berikut dibawah ini, nilai Docking/Grid score yaitu -57.144726: 
6.	Nilai keseluruhan GBSA yaitu GBSA Score -38.356567, nilai GBSA Energy Van der walls -53.421894, Energi elektrostatik -96.454689, Energy Generalized Born yaitu 118.059814 dan energy surface area yaitu -6.539795

**H. Penginstalan software untuk Docking hasil Virtual Screening atau bahan alam**
1. Instalasi Gnina dengan cara download Perangkat lunak pada link berikut: https://github.com/gnina/gnina/releases/download/v1.0.2/gnina
2. Instalasi open babel dengan perintah di terminal linux: `sudo apt-get install -y openbabel`
3. Instalasi vina_split dengan perintah di terminal linux: `sudo apt-get install -y autodock-vina`
4. Buatkan path file gnina pada terminal linux

**I. Simulasi Docking dengan Senyawa Hasil Virtual Screening atau bahan alam**
1. Copy folder kerja sebelumnya dan paste di tempat yang sama, beri nama folder tersebut sesuai kebutuhan
2. Gambar Struktur molekul terlebih dahulu menggunakan Marvin Sketch, contoh struktur yang digambar adalah **curcumin** tulisan struktur ini dapat di copy pada notepad dan dipaste langsung pada software marvin sketch
3. Simpan file struktur tersebut dengan format file .pdb dengan nama file: *contoh.pdb* klik yes atau ok
4. Struktur juga dapat digambar secara manual
5. Jalankan perangkat lunak Gnina untuk memperoleh ligan dengan Koordinat X,Y dan Z
6. Download PDB 2nnq di sini https://files.rcsb.org/download/2NNQ.pdb dan Pisahkan protein dan ligand dengan perintah di terminal linux satu persatu:
`grep ATOM 2nnq.pdb > rec.pdb` selanjutnya
`grep T4B 2nnq.pdb > lig.pdb`
7. Jalankan perintah berikut dalam satu baris terminal:
`gnina -r rec.pdb -l contoh.pdb --autobox_ligand lig.pdb -o docked.sdf --seed 0`
8. Jalankan perintah berikut untuk merubah format file: 
`obabel docked.sdf -O docked.pdbqt*`
9. Selanjutnya extract file tersebut menggunakan vina_split dengan perintah:
`vina_split --input docked.pdbqt`
10.	Selanjutnya nanti akan diperoleh file dengan nama docked_ligand_1.pdbqt selanjutnya ubah nama dan format file tersebut dengan format mol2 dengan perintah:
`obabel docked_ligand_1.pdbqt -O 2nnq_lig_withH.mol2`
11.	Buka file  2nnq_lig_withH.mol2 menggunakan Avogadro
12.	Lakukan optimasi geometri dengan menggunakan Avogadro
13.	Klik File, cari folder kerja, buka file 2nnq_lig_withH.mol2
14.	Tambahkan hidrogen dengan cara Build, add Hydrogen
15.	Lakukan optimasi geometri dengan avogadro dengan cara klik Extensions, Molecular Mechanics, Setup Force Field, pilih GAFF (General Amber Force Field)
16.	Selanjutnya klik Extensions, klik optimize geometry
17.	Klik file selanjutnya save as, pastikan format filenya adalah .mol2 dan timpa file sebelumnya atau bisa langsung klik save
18. Proses docking dilakukan sama pada step # Part 2: Preparasi Input File dan Simulasi Docking pada Bagian A poin 9 sampai dilakukan pembuatan Box, Grid, Minimisasi, Rigid docking dan Flexible docking

**J. Analisis Footprint hasil minimisasi**
1. Download file footprint.in https://github.com/purnawanpp/dock6_2nnq/blob/main/footprint.in dan plot.py https://github.com/purnawanpp/dock6_2nnq/blob/main/plot.py dan simpan di folder kerja
2. Jalankan perintah berikut: `dock6 -i footprint.in`
3. Selanjutnya jalankan perintah berikut: `python plot.py fps.min.output_footprint_scored.txt 50`
4. Akan diperoleh file berupa file: https://github.com/purnawanpp/dock6_2nnq/blob/main/fps.min.output_footprint_scored.txt.pdf 
5. Hasil akan berbeda jika menggunakan footprint hasil flexibledocking

## Catatan
Tutorial ini cocok untuk versi DOCK6.9, banyak update pada DOCK6.10 terutama perhitugan GBSA yang hanya bisa dalam keadaan rigid. Contoh script perhitungan GBSA pada DOCK6.10 dapat dibaca disini: https://github.com/purnawanpp/dock6_2nnq/blob/main/hawkins_gbsa1.dockin

# Daftar Pustaka
1. https://ringo.ams.stonybrook.edu/index.php/2018_DOCK_tutorial_1_with_PDBID_2NNQ#VI._Virtual_Screen
