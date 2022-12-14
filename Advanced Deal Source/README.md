# BBO Script

Pengaturan ini bisa digunakan untuk practice & bidding table
<br>
Practice Table : Bermain berempat (bisa dengan diri sendiri) atau robot jika punya
<br>
Bidding Table : Hanya Bidding tetapi harus berdua / sama partner
<br>
Setelah masuk table :
1. Klik garis 3 di kiri atas
2. Klik "Deal Source" atau ketiga dari bawah
3. Klik "Advanced" atau yang paling kanan
4. Wajib centang pilihan kedua
5. Pilih Dealer
6. Jika ingin gantian yang dapet kartu settingan, centang pilihan pertama
7. Copy salah satu script, misal "Open_1C.txt"
8. Paste di BBO-nya
9. Redeal

## Catatan
- Kartu yang diatur lewat script agar melakukan opening adalah South
- East & West diatur agar tidak bisa melakukan bidding
- Bisa saja partner mendapatkan kartu opening lain, sehingga jika ingin selalu opening,
	jangan centang pilihan pertama (rotate 180 degree) dan dealer selalu south
<img src="images/random_180_off_south.jpg">

- Kartu musuh diatur supaya tidak bisa overcall, jika ingin bisa overcall, ubah nilai variabel can_bid_EW ke 1:
```
can_bid_EW = 1
```
EW akan diberikan distribusi yang balance tanpa 5 lembar dan hcp yang rata, sehingga kemungkinan melakukan overcall sangat kecil

- Jika ingin mengganti rentang minimum maksimum HCP North + South ( default : [21,40] ), edit bagian ini :
```
//Minimum HCP North + South
min_hcp_NS = 21
max_hcp_NS = 40
```
- Jika ingin dibuatkan script, menemukan error, dll, dapat menghubungi saya lewat email : aminemc236@gmail.com

## Cara menulis Script sendiri
Berikut beberapa command / syntax agar bisa menulis sendiri

### Logic / Syntax
Logic pada BBO script ada beberapa yaitu :
```
+	: tambah
-	: kurang
*	: kali
/	: bagi
%	: modulo
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
?:	: (ekspresi) ? true : false
```

### Variable
Kita bisa menyimpan kondisi dalam variabel untuk mempermudah pembuatan script, semisal seperti dibawah ini
```
nama_variabel = 1
```
Pada contoh di atas, kita set nilai dari nama_variabel True
<br>
Nama variabel hanya boleh diawali dengan huruf, tidak mengandung spesial karakter selain "_"
<br>
Berikut contoh penulisan variabel yang benar
```
nama_variabel
south5332
northHCP
eastHCP_16_17
```

Berikut contoh penulisan variabel yang salah
```
123_adf
_nama
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
Kita juga bisa mengecek HCP pada salah satu suit
```
//Cek apakah south mempunyai 10 HCP pada suit Heart
hcp(south, heart) == 10
```

diakhir untuk membuat distribusi yang diberikan oleh BBO mengeluarkan logic yang kita telah simpan pada variabel jalankan perintah pada bagian berikutnya yaitu "condition"

### Condition
Command "condition" digunakan untuk mengecek kartu yang telah digenerate apakah sesuai dengan logic yang diberikan, jika tidak akan di-skip, jika memenuhi akan diberikan kepada pemain
```
condition HCPW12Plus && HCPN_15_17
```
taruh command tersebut di akhir script
condition hanya boleh sekali, jika lebih dari sekali, hanya menerapkan condition yang terakhir kali dipanggil

### Lembaran Trump
Kita bisa mengecek lembaran masing masing suit yaitu dengan command ["namaSuit"+"s({player})"] atau langsung saja lihat contohnya
```
spades(north) > 5
hearts(west) == 4
clubs(east) < 2
diamonds(south) > 6
```

### Shape
Shape merupakan salah satu tool terkuat yang dimiliki BBO untuk mengeluarkan distribusi, berikut beberapa beserta penjelasan
#### simple shape
shape di bawah akan mengecek tangan north, apakah distribusinya 5431 dengan 5 Spades, 4 Hearts, 3 Diamonds, 1 Clubs
```
shape(north, 5431)
```

#### any shape
shape di bawah akan mengecek tangan east apakah 5332, akan tetapi bisa teracak tidak harus sesuai urutan spade, heart, diamond, club
```
shape(east, any 5332)
```

#### x shape
shape di bawah akan mengecek apakah tangan south mempunyai lembaran sesuai shape tanpa mempedulikan distribusi di suit lainnya yang x
```
//55 minor
shape(south, xx55)
//55 Major
shape(south, 55xx)
//55 any two suiter
shape(south, any 55xx)
//Weak 8 dengan void
shape(south, any 80xx)
```

#### + shape
shape di bawah akan mengecek tangan west apakah 4333 atau 4432 atau 5332 tidak harus seusai urutan SHDC karena ada "any"
```
shape(west, any 4333 + any 4432 + any 5332)
```
shape di atas sering digunakan untuk mendapatkan distribusi balance

#### - shape
shape di bawah akan mengecek apakah tangan south balance, tetapi tidak mempunyai 5 lembar Major
```
shape(south, any 4333 + any 4432 + any 5332 - 5xxx - x5xx)
```
"- 5xxx" membuat distribusi tidak boleh mempunyai 5 lembar spade dengan tidak mempedulikan suit lainnya, jika digabungkan dengan "+ any 5332", maka kita tidak akan mendapatkan 5332 dengan spade 5 lembar
<br>
begitu juga dengan heart "- x5xx"

### Contoh Script
Berikut contoh script untuk opening 1C presisi yaitu "16-17 HCP Unbalance / 18+ any dist"
```
//----- Open 1C -----//
//1C = 16-17 Unbalance or 18+ any

//Rentang HCP South
southHCP = hcp(south) >= 16

southHCP_16_17 = 16 <= hcp(south) && hcp(south) <= 17
southHCP_18 = hcp(south) >= 18

//Distribusi South
southBal = shape(south, any 4432 + any 4333 + any 5332)
southDist = ( (southHCP_16_17 && !southBal) || southHCP_18 )

condition southDist

//----- ..... -----//
```

