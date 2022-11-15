# BBO Script

Pengaturan ini bisa digunakan untuk practice & bidding table
setelah masuk table :
1. Klik garis 3 di kiri atas
2. Klik "Deal Source" atau ketiga dari bawah
3. Klik "Advanced" atau yang paling kanan
4. Wajib centang pilihan kedua
5. Pilih Dealer
6. Jika ingin gantian yang dapet kartu settingan, centang pilihan pertama
7. Copy salah satu codingan / settingan dari file lain, misal "Open_1C.txt"
8. Paste di BBO-nya
9. Redeal

## Catatan
1. Kartu yang diatur lewat codingan adalah South
2. Bisa saja partner mendapatkan kartu opening lain, sehingga jika ingin selalu opening,
	jangan centang pilihan pertama (rotate 180 degree) dan dealer selalu south
<img src="images/random_180_off_south.jpg">
3. Kartu musuh diatur supaya tidak bisa overcall, jika ingin bisa overcall, hapus bagian ini :
```
//Supaya kemungkinan musuh melakukan bid sangat kecil
balancedOpps = shape(east, any 4432 + any 4333) && shape(west, any 4432 + 4333)
balancedHCP = -1 <= (hcp(east) - hcp(west)) && (hcp(east) - hcp(west)) <= 1
conditionEW = balancedOpps && balancedHCP
```
4. Jika ingin selalu Game / ++, edit bagian ini :
```
//Jika ingin kartunya mengarah game terus, 
//atur ini ke ">= 24 atau 25"
totalHCP = (hcp(south) + hcp(north)) >= 21
```




## Cara menulis Script sendiri
Berikut beberapa command / syntax agar bisa menulis sendiri

### Logic / Syntax
Logic pada BBO script ada beberapa yaitu :
```
1   : True
0   : False
&&  : dan
||  : atau
!   : negasi
>   : lebih dari
>=  : lebih dari sama dengan
<   : kurang dari
<=  : kurang dari sama dengan
==  : sama dengan
!=  : tidak sama dengan
()  : tanda kurung
//  : Comment
```

### Variable
Kita bisa menyimpan kondisi dalam variabel untuk mempermudah pembuatan script, semisal seperti dibawah ini
```
nama_variabel = 1
```
Pada contoh di atas, kita set nilai dari nama_variabel True
<br>
Nama variabel yang tidak diperbolehkan adalah diawali dengan angka, mengandung spesial karakter selain "_"
```
123_adf
nama variabel
hcpw_12++
```

### Player
Player ditulis dengan "north", "west", "south", "east"
<br>
Contoh pemakaian akan ditunjukkan pada command command selanjutnya

### HCP
Kita bisa mengecek HCP salah satu player, dengan cara "hcp(player)", berikut contohnya
```
//Cek apakah west hcp nya > 12
HCPW12Plus = hcp(west) >= 12
//Cek apakah hcp north terdapat pada rentang [15,17]
HCPN_15_17 = 15 <= hcp(north) && hcp(north) <= 17
```
diakhir untuk membuat distribusi yang diberikan oleh BBO mengeluarkan logic yang kita telah simpan pada variabel jalankan perintah pada bagian berikutnya

### Condition
Command "condition" digunakan untuk mengecek kartu yang telah digenerate apakah sesuai dengan logic yang diberikan, jika tidak akan di-skip, jika memenuhi akan diberikan kepada pemain
```
condition HCPW12Plus && HCPN_15_17
```
taruh command tersebut di akhir script
condition hanya boleh sekali, jika lebih dari sekali, hanya menerapkan condition yang terakhir kali dipanggil



