# Tutorial Simulasi Molecular Docking Menggunakan DOCK6
Video Tutorial Dapat dilihat disini: https://youtu.be/3y_uxZAhbqo
# Part 1 Preparasi Receptor dan Ligand Menggunakan Chimera
**A.	Preparasi Receptor**
1.	Semua file hasil simulasi dan script dapat didownload pada link berikut: https://github.com/purnawanpp/dock6_2nnq
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

**B.	Preparasi Ligand**
1.	Buka lagi file 2nnq.pdb
2.	Klik file > open 2nnq
3.	Klik Select> Residue pilih residu T4B
4.	Klik Select> Klik Invert (Selected models)
5.	Klik Actions>atom/Bonds>delete
6.	Klik Tools> Structure Editing >Klik Dock Prep
7.	Klik Tools -> Structure Editing> Klik Dock Prep -> Secara otomatis akan ditambahkan Tambahkan H (Untuk menambahkan atom Hidrogen) dan muatan (Untuk menambahkan muatan, gunakan medan gaya AMBER terbaru untuk residu standar. Di tutorial kali ini menggunakan menggunakan AMBER ff14SB)
8.	Pada Dock Prep Langsung klik “Ok” Selanjutnya 
9.	Pada Add Hydrogen langsung klik “Ok”
10.	Pada bagian label jangan dicentang
11.	Pada bagian specipy net charge pilih -1 klik ok, pastikan charge method nya adalah Gastaiger
12.	Simpan dengan format .mol2 dengan nama 2nnq_lig_withH.mol2 
13.	Klik file close session
 
# Part 2: Preparasi Input File dan Simulasi Docking
**A.	Pembuatan receptor surface and spheres (Bola Receptor)**
1.	Buka file 2nnq_rec_noH.mol2 menggunakan chimera
2.	Klik Action -> Surface -> Show
3.	Klik Tools -> Structure Editing -> Write DMS
4.	Simpan dengan nama file: 2nnq_rec_noH.dms
5.	Pastikan file 2nnq_rec_noH.dms ada dalam folder kerja
6.	Buat File dengan Nama *INSPH* https://github.com/purnawanpp/dock6_2nnq/blob/main/INSPH
7.	Buka terminal Ubuntu pastikan terminal ubuntu telah membaca directory anda. Ketik Perintah Berikut pada Terminal: 
**sphgen -i INSPH -o OUTSPH**
9.	Jika terjadi eror baca pesan eror tersebut dan hapus file yang exist
10.	Di sini selanjutnya akan dipilih spheres yang merupakan kantong pengikat ligan, kita akan mencoba mengarahkan ligan ke tempat pengikatan ke reseptor. Untuk memilih mengatur sphres tersebut ketik perintah berikut pada terminal:
**sphere_selector 2nnq_rec.sph 2nnq_lig_withH.mol2 10.0**

**B.	Pembuatan Box**
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *showbox.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/showbox.in
2.	Arti script diatas adalah Kita akan membuat kotak dengan panjang persegi 8 Angstrom, Gunakan file selected_spheres di lokasi folder, nama file output pdb yang berisi kotak yang dihasilkan adalah 2nnq.box.pdb
3.	Pada terminal ketikan perintah berikut: **showbox < showbox.in**
4.	Jika langkah ini berhasil, Anda akan melihat file baru dengan nama 2nnq.box.pdb

**C.	Pembuatan Grid**
1.	Pastikan file flex.defn, flex_drive.tbl, dan vdw_AMBER_parm99.defn ada dalam folder kerja, file tersebut dapat didownload pada link berikut: https://github.com/purnawanpp/dock6_2nnq
2.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *grid.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/grid.in
3.	Pada terminal ketikan perintah berikut: **grid -i grid.in -o gridinfo.out**
4.	Jika perintah berhasil, tiga file baru akan dihasilkan seperti (gridinfo.out, grid.nrg, grid.bmp). Buka file gridinfo.out untuk memastikan semua informasi tentang reseptor dalam file sesuai dengan informasi asli dari reseptor. (Misalnya: Total muatan, residu dan muatannya) Jika informasi tidak cocok, itu berarti Anda telah melakukan kesalahan pada salah satu langkah yang Anda ikuti sejauh ini.

**D.	Minimisasi Energi**
1. Sebelum melakukan docking, ligan akan mengalami minimisasi energi untuk menghilangkan benturan atom yang tidak menguntungkan. Benturan ini akan mempengaruhi rigid docking karena pada rigid docking ligan akan ditambatkan sebagai ligan yang lengkap, sedangkan pada metode flexible docking dan fixed anchor docking ligan akan dipecah menjadi fragmen dan ligan akan dibangun perbagian dengan mempertimbangkan orientasi dan sudut putar.
2. Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *min.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/min.in
3.	Pada terminal ketikan perintah berikut: **dock6 -i min.in**
4.	Jika prosesnya berhasil, file baru dengan nama 2nnq.lig.min_scored.mol2.

**E. Simulasi Rigid Docking**
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *rigid.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/rigid.in
2.	Pada terminal ketikan perintah berikut: **dock6 -i rigid.in**
3.	Setelah Simulasi Rigid Docking berhasil, file output baru yaitu rigid.out_scored.mol2 Visualisasikan file output ini menggunakan Chimera dengan mengikuti langkah-langkah untuk memeriksa keberhasilan proses docking
4.	Klik File> open -> 2nnq_rec_withH.mol2 File 
5.	Klik open -> 2nnq_lig_withH.mol2
6.	Klik Tools -> Surface/binding Analysis -> ViewDock -> Pilih file  rigid.out_scored.mol2
7.	Pilih “Dock4,5 or 6” klik OK
8.	Untuk mengatur tampilan dapat mengklik column>show>Pilih yang ingin ditampilkan

**F.	Fixed Anchor Docking**
1.	Buat file baru dengan cara klik kanan lalu pilih Nex Text Document.txt ubah file tersebut menjadi *fixed.in* https://github.com/purnawanpp/dock6_2nnq/blob/main/fixed.in
2.	Pada terminal ketikan perintah berikut: **dock6 -i fixed.in**
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
3.	Pada terminal ketikan perintah berikut: **dock6 -i flex.in**
4.	Ketikan perintah masing-masing sesuai dalam kurung secara manual [    ] pada terminal lalu enter. 
5.	Hasil simulasi berikut dibawah ini, nilai Docking/Grid score yaitu -57.144726: 
6.	Nilai keseluruhan GBSA yaitu GBSA Score -38.356567, nilai GBSA Energy Van der walls -53.421894, Energi elektrostatik -96.454689, Energy Generalized Born yaitu 118.059814 dan energy surface area yaitu -6.539795

