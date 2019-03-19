# Partisi
Partisi adalah memecah tabel ke dalam beberapa segment (partisi atau subpartisi), di mana tabel konvensional hanya mempunyai satu segment.
# Tipe Partisi
1. Range
Data dikelompokkan berdasarkan range(rentang) nilai yang kita tentukan. Range partition ini cocok digunakan pada kolom yang nilainya terdistribusi secara merata. Contoh yang paling sering adalah kolom tanggal.
2. List
Pada list partition, data dikelompokkan berdasarkan nilainya. Cocok untuk kolom yang variasi nilainya tidak banyak.
3. Hash
Jika kita ingin melakukan partisisi namun tidak cocok dengan RANGE ataupun LIST, maka kita bisa menggunakan HASH partition. Penentuan “nilai mana di taruh di partisi mana” itu diatur secara internal oleh Oracle (berdasarkan hash value).
4. Key
Partisi ini mirip dengan partisi HASH, perbedaannya hash menggunakan algoritma fungsi mysql standar yaitu MOD. sedangkan partisi key menggunakan algoritma yang memang didesain untuk data yang memiliki key jadi partisi ini digunakan untuk kolom yang memiliki key.

# Implementasi Partisi pada database Vertabelo
pada kasus ini kami menggunakan database vertabelo 
## 1. Range Partition
```
CREATE TABLE lc (
    a INT NULL,
    b INT NULL
)
PARTITION BY LIST COLUMNS(a,b) (
    PARTITION p0 VALUES IN( (0,0), (NULL,NULL) ),
    PARTITION p1 VALUES IN( (0,1), (0,2), (0,3), (1,1), (1,2) ),
    PARTITION p2 VALUES IN( (1,0), (2,0), (2,1), (3,0), (3,1) ),
    PARTITION p3 VALUES IN( (1,3), (2,2), (2,3), (3,2), (3,3) )
);
```
## 2. Lish Partition
```
CREATE TABLE serverlogs (
    serverid INT NOT NULL, 
    logdata BLOB NOT NULL,
    created DATETIME NOT NULL
)
PARTITION BY LIST (serverid)(
    PARTITION server_east VALUES IN(1,43,65,12,56,73),
    PARTITION server_west VALUES IN(534,6422,196,956,22)
);
```
## 3. Hash Partision
```CREATE TABLE serverlogs2 (
    serverid INT NOT NULL, 
    logdata BLOB NOT NULL,
    created DATETIME NOT NULL
)
PARTITION BY HASH (serverid)
PARTITIONS 10;
```
## 4. Key Partition
```
CREATE TABLE serverlogs4 (
    serverid INT NOT NULL, 
    logdata BLOB NOT NULL,
    created DATETIME NOT NULL,
    UNIQUE KEY (serverid)
)
PARTITION BY KEY()
PARTITIONS 10;
```

## Mengecek apakah plugin partition telah aktif (command dan screenshot hasil)
```
SHOW PLUGINS 
atau
INFORMATION_SCHEMA.PLUGINS
```
[ss]

## Create partitiuat tabel dan contoh insert data dan hasil query-nya

# Referensi
https://www.vertabelo.com/blog/technical-articles/everything-you-need-to-know-about-mysql-partitions