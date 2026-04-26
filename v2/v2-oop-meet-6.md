# OOP: Chapter 6

composed by [_Bimo Ade Budiman Fikri_](https://www.linkedin.com/in/bimoadee/)

![alt text](../assets/5.png)

## **Table of Contents**

- [Pemrosesan Input/Output](#pemrosesan-io)
  - [Pendahuluan](#pendahuluan)
  - [Konsep Dasar I/O](#konsep-dasar-io-dalam-java)
  - [Class-Class Kunci pada I/O](#class-class-kunci-pada-io)
  - [Contoh Kode](#contoh-kode)
- [Pemrograman Jaringan](#pemrograman-jaringan)
  - [Pendahuluan](#pendahuluan-1)
  - [Konsep Dasar Networking](#konsep-dasar-pemrograman-jaringan)
  - [Class-Class Kunci pada Networking](#class-class-kunci-pada-networking)
  - [Contoh Kode](#contoh-kode-1)

---

# Pemrosesan I/O

## Pendahuluan

Dalam praktik pengembangan aplikasi nyata, program tidak selalu berkutat dengan _resource_-nya sendiri. Di pertemuan sebelumnya, program telah berinteraksi dengan penyimpanan fisik di luar sistem (_database_).

Di level selanjutnya, program akan sering juga berinteraksi pada pemrosesan **_input_** (membaca data dari sumber eksternal) dan pemrosesan **_output_** (menulis data ke tujuan eksternal).

Java telah menyediakan kerangka kerja I/O yang kaya dan kuat yang sebagian besar dibangun di atas prinsip _Object-Oriented Programming_ (OOP). Materi ini akan membahas konsep dasar I/O dalam Java, penjelasan rinci mengenai kelas-kelas kunci, analogi atau studi kasus nyata untuk memperjelas pemahaman, dan contoh kode lengkap untuk setiap skenario.

---

## Konsep Dasar I/O dalam Java

Secara umum, I/O dalam Java melibatkan transfer data antara program Kita dan perangkat eksternal. Perangkat eksternal ini bisa berupa:

- **Keyboard**: Sumber input paling umum dari pengguna.
- **Layar Konsol**: Tujuan output paling umum untuk menampilkan informasi.
- **File**: Penyimpanan data persisten di disk.
- **Jaringan**: Komunikasi dengan aplikasi atau server lain melalui internet/intranet.
- **Memory**: Transfer data antar bagian dalam program.

Java mengimplementasikan I/O mengikuti konsep _[stream](#stream)_ atau aliran.

![alt](https://media.geeksforgeeks.org/wp-content/uploads/20210913171831/TypesofJavaIOStreams.png)

### Stream

_Stream_ adalah abstraksi aliran data. Bayangkan memindahkan data itu seperti memindahkan air menggunakan pipa:

- **Input Stream:** Pipa yang menyedot air (data) dari luar ke dalam program kita (Membaca/_Read_).
- **Output Stream:** Pipa yang menyemburkan air (data) dari program kita ke luar (Menulis/_Write_).

Dengan begitu, _Stream_ menjadi cara dalam menangani transaksi data secara berurutan, satu per satu. Aliran ini bisa berupa _[byte stream](#byte-stream)_ (untuk data biner) atau _[character stream](#character-stream)_ (untuk data teks).

<br>

### Exception Handling untuk I/O

Operasi I/O bersifat "berisiko" karena bergantung pada faktor di luar kendali kode. Misalnya:

- Mencoba membaca file yang tidak ada (`FileNotFoundException`).
- Koneksi jaringan terputus saat transfer data.
- Media penyimpanan penuh atau rusak.

Karena risiko ini, sebagian besar kelas dalam _package_ `java.io` menggunakan _Checked Exceptions_, yang artinya _compiler_ akan memaksa kita untuk menangani _error_ tersebut secara eksplisit.

Di Java, `IOException` adalah kelas induk untuk hampir semua kesalahan terkait I/O. Beberapa subclass terkait `IOException`, yaitu:

- `FileNotFoundException`: File tidak ditemukan.
- `EOFException`: Mencapai akhir file secara tidak terduga.
- `SocketException`: Kesalahan pada koneksi jaringan.

Sejak Java 7, cara stKitar untuk menangani _exception_ adalah menggunakan blok **_Try-with-Resources_**. Fitur ini secara otomatis menutup file meskipun terjadi _error_.

```java
try (FileReader fr = new FileReader("test.txt")) {
    // Lakukan sesuatu dengan file
} catch (IOException e) {
    e.printStackTrace();
}
// File otomatis tertutup di sini, tidak perlu blok finally!
```

<br>

### Menutup Stream

Sangat penting untuk selalu menutup stream setelah selesai menggunakannya. Kegagalan menutup stream dapat menyebabkan:

- **Kebocoran Sumber Daya:** Sumber daya sistem (seperti file handle) tetap terbuka, menghabiskan memori dan membatasi aplikasi lain.
- **Data _Corrupt_/Hilang:** Data yang ditulis mungkin tidak sepenuhnya disimpan ke tujuan hingga stream ditutup.

<br>

### Buffer

Dalam dunia komputer, memindahkan data (Input/Output) itu mirip seperti memindahkan air. Data perlu dibaca dari suatu sumber (misalnya, hard drive, jaringan) atau ditulis ke suatu tujuan (hard drive, jaringan, layar). Operasi I/O, terutama yang melibatkan perangkat fisik, adalah operasi yang sangat lambat dibandingkan dengan kecepatan CPU dan memori.

Di sinilah konsep Buffer menjadi sangat penting.

#### Apa itu Buffer?

**Buffer** adalah area memori sementara yang digunakan untuk menyimpan data saat sedang dipindahkan antara dua lokasi. Sebagai aturan umum, selalu anggap bahwa kita perlu menggunakan buffer untuk operasi I/O yang melibatkan file, jaringan, atau perangkat eksternal lainnya.

**_Kenapa butuh Buffer?_** Mengirim air tetes demi tetes ke dalam pipa itu sangat lambat dan membuang waktu. Oleh karena itu, kita butuh _Buffer_ (Ember) untuk menampung air (data) terlebih dahulu. Jika ember sudah penuh, barulah kita tuangkan semuanya ke dalam pipa sekaligus. Pendekatan ini memberikan beberapa keuntungan:

1. Meningkatkan performa aplikasi
2. Mengurangi Frekuensi Akses ke Disk/Jaringan secara langsung
3. Menyediakan Fleksibilitas dalam Ukuran Data
4. Memungkinkan Operasi `flush()` untuk memastikan data tidak hilang jika terjadi _crash_ atau aplikasi ditutup

<br>

## Class-class Kunci Pemrosesan I/O

Java memiliki hierarki kelas I/O yang luas di dalam paket `java.io`. Kelas-kelas ini dibagi menjadi dua kategori utama berikut ini.

### Dasar Arsitektur I/O: Byte Stream

_Byte Stream_ adalah pondasi utama I/O di Java. Ia digunakan untuk membaca dan menulis data biner mentah (_raw binary data_) berukuran _8-bit_.

Karakteristik utamanya adalah ia tidak memedulikan isi data—baik itu gambar, audio, video, atau file teks, _Byte Stream_ memperlakukannya sebagai deretan angka biner.

#### `InputStream` (Abstrak Class)

`InputStream` adalah superclass abstrak yang merepresentasikan aliran input _byte_. Karena bersifat abstrak, Kita tidak bisa membuat objek langsung darinya, melainkan melalui _subclass_-nya (seperti `FileInputStream`).

- `int read()`: Membaca satu _byte_ data. Mengembalikan nilai int (0-255). Mengembalikan -1 jika mencapai akhir stream (EOF/_End of File_).
- `int read(byte[] b)`: Membaca sejumlah _byte_ ke dalam array b. Mengembalikan jumlah _byte_ yang dibaca, atau -1 jika akhir stream tercapai.
- `void close()`: Menutup input stream.

_Mengapa `read()` mengembalikan `int`_?

Meskipun yang dibaca adalah _byte_, Java menggunakan `int` agar bisa memberikan nilai khusus -1 sebagai penKita bahwa data sudah habis. Jika menggunakan tipe data _byte_, nilai -1 bisa disalahartikan sebagai data biner biasa.

#### `OutputStream` (Abstrak Class)

`OutputStream` adalah _superclass_ abstrak untuk semua kelas yang menulis aliran _byte_ ke tujuan tertentu (seperti file atau jaringan).

- `void write(int b)`: Menulis satu _byte_ data ke tujuan.
- `void write(byte[] b)`: Menulis seluruh array _byte_ ke tujuan.
- `void wrie(byte[] b, int off, int len)`: Menulis sebagian _array byte_, dimulai dari posisi `off` sebanyak `len` _byte_.
- `void flush()`: Memaksa data yang masih tertahan di _buffer_ internal untuk segera ditulis ke tujuan. Ini memastikan data tidak "tersangkut" di memori.
- `void close()`: Menutup aliran output. Secara otomatis memanggil `flush()` sebelum menutup.

#### Implementasi Penting Byte Stream

Untuk mempraktikkan konsep abstrak di atas, Java menyediakan beberapa _subclass_ implementatif yang sering digunakan dalam pengembangan aplikasi:

1. `FileInputStream` / `FileOutputStream`: Digunakan untuk membaca atau menulis data langsung ke file di media penyimpanan (_harddisk_).

2. `BufferedInputStream` / `BufferedOutputStream`: Seperti yang telah dibahas sebelumnya, kelas ini membungkus _stream_ lain untuk menambahkan lapisan _buffer_. Ini sangat meningkatkan efisiensi karena mengurangi jumlah interaksi langsung dengan hardware.

3. `DataInputStream` / `DataOutputStream`: Memungkinkan kita membaca/menulis tipe data primitif Java (`int`, `double`, `boolean`) secara langsung ke stream tanpa harus mengonversinya secara manual menjadi _array byte_.

4. `ObjectInputStream` / `ObjectOutputStream`: Digunakan untuk proses **Serialisasi**, yaitu menyimpan seluruh objek Java ke dalam bentuk biner dan membacanya kembali.

<br>

### Pengolahan Teks: Character Stream

Jika _Byte Stream_ beroperasi pada level biner (_8-bit_), _Character Stream_ dirancang khusus untuk menangani data teks secara efisien. Di balik layar, _Character Stream_ tetap menggunakan _Byte Stream_, namun ia secara otomatis melakukan konversi dari byte ke karakter berdasarkan stKitar _encoding_ seperti UTF-8 atau UTF-16.

Dengan menggunakan _Character Stream_, kita tidak perlu khawatir karakter non-ASCII (seperti simbol matematika atau huruf Arab/Jepang) akan rusak saat dibaca atau ditulis. Unit data terkecil yang diproses di sini adalah karakter _Unicode 16-bit_.

#### `Reader` (Abstrak Class)

Superclass untuk semua aliran input karakter. Metode Kunci:

- `int read()`: Membaca satu karakter. Mengembalikan nilai `int` dalam rentang 0 hingga 65535. Mengembalikan -1 jika sudah mencapai akhir file.
- `int read(char[] cbuf)`: Membaca sejumlah karakter dan menyimpannya ke dalam _array_ `cbuf`.
- `void close()`: Menutup aliran input dan melepaskan sumber daya.

#### `Writer` (Abstrak Class)

Superclass untuk semua aliran output karakter. Metode Kunci:

- `void write(int c)`: Menulis satu karakter tunggal.
- `void write(char[] cbuf)`: Menulis array satu karakter.
- `void write(String str)`: Menulis sebuah string secara langsung (sangat praktis dibanding _Byte Stream_).
- `void flush()`: Mengosongkan _buffer_ dan memaksa karakter keluar ke tujuan.
- `void close()`: Menutup aliran output.

#### Kelas Jembatan

Inilah salah satu bagian terpenting dalam OOP Java I/O. Kelas ini bertindak sebagai _Adapter_ yang mengubah aliran byte menjadi aliran karakter.

- `InputStreamReader`: Mengubah `InputStream` (_byte_) menjadi `Reader` (karakter)
  **Contoh:** `new InputStreamReader(System.in)` — Mengubah input keyboard yang berupa _byte_ menjadi karakter yang bisa dibaca teksnya.

- `OutputStreamWriter`: Mengubah `OutputStream` (_byte_) menjadi `Writer` (karakter).

#### Subclass Implementasi Penting dari Character Stream

Selain kelas jembatan, berikut adalah kelas-kelas yang akan paling sering digunakan dalam pengembangan:

- `FileReader` / `FileWriter`: Cara termudah untuk membaca dan menulis file teks secara langsung di _disk_.
- `BufferedReader` / `BufferedWriter`:
  - Sangat disarankan untuk performa.
  - Memiliki metode sakti `readLine()` pada `BufferedReader` yang memungkinkan kita membaca teks baris demi baris, bukan karakter demi karakter.

- `PrintWriter`:
  - Kelas "serba bisa" untuk output teks.
  - Memiliki metode `print()` dan `println()` seperti yang biasa Kita gunakan pada `System.out`.
  - Sangat fleksibel karena bisa mencetak berbagai tipe data (`int`, `double`, `object`) langsung sebagai teks.

**Mengapa memilih Character Stream untuk Teks?**

| Aspek     | Byte Stream               | Character Stream                   |
| --------- | ------------------------- | ---------------------------------- |
| Unit Data | 8-bit (Byte)              | 16-bit (Unicode Character)         |
| Fokus     | Data Biner (Gambar/Video) | Data Teks (String/Dokumen)         |
| Encoding  | Manual (Raw)              | Otomatis (UTF-8, dll)              |
| Kemudahan | Sulit mengolah baris teks | Sangat mudah (tersedia readLine()) |

### `Scanner` untuk Input User

Meskipun bukan bagian dari hierarki `java.io` (ia berada di paket `java.util`), `Scanner` adalah kelas yang paling sering digunakan untuk berinteraksi dengan pengguna. Berbeda dengan _Stream_ yang hanya "memindahkan" data, `Scanner` dirancang untuk mengurai (_parsing_) data mentah menjadi tipe data primitif (`int`, `double`, `boolean`) atau `String`.

`Scanner` bekerja dengan memecah input menjadi potongan-potongan kecil yang disebut Token berdasarkan pemisah (_delimiter_), yang secara _default_ adalah spasi atau baris baru.

#### A. Fleksibilitas Sumber Data (OOP Context)

`Scanner` mendemonstrasikan kekuatan _Polymorphism_. Ia tidak hanya bisa membaca dari _keyboard_, tapi dari berbagai sumber data yang telah kita pelajari sebelumnya:

```java
Scanner s1 = new Scanner(System.in);          // Mengolah Byte Stream (Keyboard)
Scanner s2 = new Scanner(new File("data.txt")); // Mengolah File
Scanner s3 = new Scanner("1 2 3");            // Mengolah String langsung
```

#### B. Kategori _Method_ Utama

Untuk menggunakan `Scanner` dengan benar, kita harus memahami bagaimana "pointer" internalnya bergerak di dalam _buffer_ input.

**1. Kelompok `next()` dan `next<Type>()`**

Contoh: `next()`, `nextInt()`, `nextDouble()`.

Cara Kerja: Membaca Token berikutnya. Ia akan melompati spasi/_delimiter_ di depan, mengambil data, dan berhenti tepat setelah data tersebut, namun sebelum _delimiter_ berikutnya.

Karakteristik: Tidak pernah mengonsumsi karakter _newline_ (`\n`).

**Contoh:**

```java
int angka = scanner.nextInt(); // angka = 123. Pointer setelah '3', sebelum spasi.
String teks = scanner.next();  // teks = "ABC". Pointer setelah 'C', sebelum '\n'.
// Karakter '\n' (newline) masih ada di buffer!
```

Gunakan `next()` (atau `nextInt()`, `nextDouble()`, dll.) ketika Kita ingin membaca satu "kata" atau satu nilai berformat (misalnya, nama depan, angka, satu token). Ingat untuk hati-hati dengan karakter _newline_ yang tersisa jika Kita mencampur `next()` dengan `nextLine()`.

**2. Kelompok `nextLine()`**

Cara Kerja: Membaca seluruh baris dari posisi pointer saat ini hingga ditemukan karakter _newline_ (`\n`).

Karakteristik: Berbeda dengan `next()`, metode ini mengonsumsi (membuang) karakter _newline_ tersebut dan memindahkan _pointer_ ke baris baru.

**Contoh:**

```java
String baris1 = scanner.nextLine(); // baris1 = "Halo Dunia". Pointer setelah '\n' pertama.
String baris2 = scanner.nextLine(); // baris2 = "Saya Java". Pointer setelah '\n' kedua.
```

**Gunakan `nextLine()` ketika Kita ingin membaca seluruh baris input**, termasuk spasi di dalamnya (misalnya, nama lengkap, alamat, kalimat panjang). Ini adalah pilihan terbaik untuk input teks yang memungkinkan spasi.

<br>

#### C. Masalah Umum "Jebakan Newline"

Ini adalah kesalahan paling umum saat mencampur penggunaan `nextInt()` (atau sejenisnya) dengan `nextLine()`.

Karena `nextInt()` tidak mengonsumsi karakter _newline_ (`\n`) setelah membaca token angka/kata, `nextLine()` yang dipanggil setelahnya akan segera membaca `newline` yang tersisa di _buffer_ dan menganggapnya sebagai baris kosong.

**Contoh:**

```java
Scanner scanner = new Scanner(System.in);

System.out.print("Masukkan usia: ");
int usia = scanner.nextInt();
// Input: 25[ENTER] -> Mengambil 25, '\n' tertinggal di buffer

System.out.print("Masukkan kota: ");
String kota = scanner.nextLine();
// Langsung mengambil '\n' yang tertinggal. Hasil: String kosong!

System.out.println("Usia: " + usia);
System.out.println("Kota: " + kota); // Akan menampilkan "Kota: " (kosong)
```

<br>

**Solusinya adalah _Dummy Read_**

Tambahkan `scanner.nextLine()` sebagai _dummy read_ setelah `nextInt()` untuk membersihkan karakter _newline_ yang tersisa di buffer.

```java
Scanner scanner = new Scanner(System.in);

System.out.print("Masukkan usia: ");
int usia = scanner.nextInt();
scanner.nextLine(); // MEMBUANG KARAKTER NEWLINE YANG TERSISA

System.out.print("Masukkan kota: ");
String kota = scanner.nextLine(); // Sekarang akan membaca "Jakarta" dengan benar

System.out.println("Usia: " + usia);
System.out.println("Kota: " + kota);
```

<br>

#### D. Metode Validasi (Safety First)

Sebelum membaca data, ada baiknya memeriksa apakah data yang tersedia sesuai dengan tipe yang kita inginkan untuk menghindari `InputMismatchException`.

- `hasNextInt()`: Menghasilkan _true_ jika token berikutnya adalah _integer_.
- `hasNextLine()`: Menghasilkan _true_ jika masih ada baris data yang bisa dibaca.

Selalu ingat untuk memanggil `scanner.close()` setelah selesai digunakan untuk mencegah kebocoran sumber daya (_resource leak_), terutama jika `Scanner` tersebut membaca dari sebuah File.

<br>

### Analogi atau Studi Kasus Nyata

#### Studi Kasus 1: Memasak dan Buku Resep

Bayangkan program Java Kita adalah seorang koki di dapur.

- **Input Stream:** Anggap ini seperti Kita membaca resep dari sebuah buku resep (file input) atau mendengarkan instruksi dari seseorang (_keyboard_ input). Kita mendapatkan bahan-bahan atau langkah-langkah satu per satu.
- **Output Stream:** Ini seperti Kita menulis resep baru ke dalam buku resep Kita (file output) atau mengatakan instruksi kepada asisten Kita (konsol output). Kita memberikan informasi.
- **Byte Stream vs. Character Stream:**
  - Jika Kita membaca resep dalam bentuk gambar (misalnya, dari buku resep kuno yang difoto), itu seperti _byte stream_ karena Kita membaca piksel-piksel biner.
  - Jika Kita membaca resep yang ditulis dalam teks biasa, itu seperti _character stream_ karena Kita membaca huruf demi huruf atau kata demi kata.

- **Buffering:** Jika Kita harus mengambil satu butir beras setiap kali Kita memasak, itu akan sangat lambat. Lebih baik mengambil segenggam beras (_buffer_) sekaligus. `BufferedReader`dan `BufferedWrite` bekerja seperti ini, mereka mengumpulkan sejumlah data sebelum benar-benar memprosesnya, mempercepat operasi.
- **try-with-resources:** Jika Kita menggunakan buku resep dan selesai memasak, Kita harus menutup buku dan meletakkannya kembali agar tidak rusak atau hilang. **try-with-resources** secara otomatis melakukan "menutup buku" ini untuk Kita, bahkan jika ada masalah saat memasak.

#### Studi Kasus 2: Aplikasi Catatan Sederhana

Bayangkan Kita sedang membangun aplikasi catatan sederhana.

- **Menulis Catatan ke File (_Output_):**
  - Pengguna mengetik catatannya di antarmuka pengguna.
  - Kita akan menggunakan `FileWriter` atau `BufferedWriter` (dibungkus `FileWriter`) untuk menulis teks catatan tersebut ke sebuah file `.txt`.
  - Jika Kita ingin menyimpan juga tanggal dan waktu catatan dibuat dalam format biner, Kita bisa menggunakan `DataOutputStream`.

- **Membaca Catatan dari File (_Input_):**
  - Ketika aplikasi dibuka, Kita perlu membaca catatan-catatan yang sudah ada.
  - Kita akan menggunakan `FileReader` atau `BufferedReader` (dibungkus `FileReader`) untuk membaca teks dari file `.txt`.
  - Jika ada data biner seperti tanggal dan waktu, Kita akan menggunakan `DataInputStream`.
- **Input dari Pengguna (_Konsol_):**
  - Jika Kita membuat aplikasi konsol, Kita akan menggunakan `Scanner` untuk membaca input dari keyboard (misalnya, judul catatan, isi catatan).
- **Output ke Konsol:**
  - Kita akan menggunakan `System.out.println()` (yang pada dasarnya adalah `PrintWriter`) untuk menampilkan pesan kepada pengguna, seperti `"Catatan berhasil disimpan!"` atau menampilkan daftar catatan.

---

## Contoh Kode

Berikut adalah contoh kode untuk berbagai skenario I/O.

### Scanner

#### Membaca Input dari Konsol menggunakan `Scanner`

Setelah memahami teori di balik Scanner, mari kita bedah bagaimana kode tersebut bekerja secara teknis di dalam memori.

```java
import java.util.Scanner;

public class KonsolInput {
    public static void main(String[] args) {
        // Membuat objek Scanner untuk membaca input dari System.in (keyboard)
        Scanner scanner = new Scanner(System.in);

        System.out.print("Masukkan nama Kita: ");
        String nama = scanner.nextLine(); // Membaca satu baris string

        System.out.print("Masukkan usia Kita: ");
        int usia = scanner.nextInt(); // Membaca integer

        // Membersihkan buffer setelah nextInt() jika ada nextLine() setelahnya
        // karena nextInt() hanya membaca angka, tidak membaca karakter newline
        scanner.nextLine();

        System.out.print("Masukkan kota asal Kita: ");
        String kota = scanner.nextLine(); // Membaca string lagi

        System.out.println("\nHalo, " + nama + "!");
        System.out.println("Kita berusia " + usia + " tahun dan berasal dari " + kota + ".");

        // Penting: Selalu tutup Scanner setelah selesai digunakan
        scanner.close();
    }
}
```

<br>

#### Penjelasan

**1. Inisialisasi dan Sumber Data**

```java
Scanner scanner = new Scanner(System.in);
```

- Di sini kita menerapkan prinsip _Composition_. Objek `Scanner` membungkus `System.in`. Perlu diingat bahwa `System.in` adalah sebuah _Byte Stream_ (`InputStream`).
- `Scanner` bertindak sebagai asisten canggih yang mengubah aliran byte mentah dari keyboard menjadi data yang bisa kita pahami (`String` dan `int`).

**2. Alur Pembacaan Token**

```java
System.out.print("Masukkan nama Kita: ");
String nama = scanner.nextLine(); // Membaca satu baris string
```

- `nextLine()` pertama: Program akan menunggu hingga pengguna menekan Enter. Seluruh teks beserta karakter _newline_ (`\n`) akan diambil, namun `\n` tersebut langsung dibuang oleh `Scanner`, sehingga variabel nama hanya berisi teks murni.

```
System.out.print("Masukkan usia Kita: ");
int usia = scanner.nextInt(); // Membaca integer
```

- `nextInt()`: Metode ini hanya mencari angka. Jika Kita mengetik 25 lalu menekan Enter, `nextInt()` hanya mengambil 25. Karakter Enter (`\n`) tetap tertinggal di dalam _buffer_.

**3. Penanganan "The Newline Trap"**

```java
scanner.nextLine(); // Dummy read
```

Inilah bagian krusial yang sering dilupakan. Baris ini tidak menyimpan data ke variabel mana pun. Tujuannya murni untuk membersihkan _buffer_. Tanpa baris ini, `nextLine()` berikutnya akan melihat karakter `\n` yang tersisa dari input usia, menganggapnya sebagai baris kosong, dan program akan langsung "melompat" ke perintah berikutnya tanpa membiarkan pengguna mengetik nama kota.

<br>

### Character Stream

#### Menulis dan Membaca Data Teks ke/dari File menggunakan `FileWriter` dan `FileReader` (tanpa buffering)

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileTeksDasar {
    private static final String NAMA_FILE = "catatan_dasar.txt";

    public static void main(String[] args) {
        // Menulis ke file
        try (FileWriter writer = new FileWriter(NAMA_FILE)) {
            writer.write("Ini adalah baris pertama.\n");
            writer.write("Baris kedua berisi angka: " + 123 + "\n");
            writer.write("Dan ini adalah baris terakhir.\n");
            System.out.println("Data berhasil ditulis ke " + NAMA_FILE);
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat menulis ke file: " + e.getMessage());
        }

        // Membaca dari file
        try (FileReader reader = new FileReader(NAMA_FILE)) {
            int karakter;
            System.out.println("\nMembaca data dari " + NAMA_FILE + ":");
            while ((karakter = reader.read()) != -1) {
                System.out.print((char) karakter); // Mengkonversi integer ke karakter
            }
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat membaca dari file: " + e.getMessage());
        }
    }
}
```

**Penjelasan:**

- **Try-with-Resources**: Perhatikan penulisan `try (Resource res = ...)`. Ini adalah fitur Java modern yang otomatis menutup _stream_ (`close()`) meskipun terjadi _error_. Ini mencegah kebocoran memori tanpa perlu blok `finally`.
- **Proses Karakter-per-Karakter**: Metode `reader.read()` mengambil satu karakter tunggal dalam bentuk integer. Kita harus melakukan _casting_ (`char`) untuk menampilkannya sebagai huruf.
- **Logika Berhenti**: _Loop_ `while` akan terus berjalan sampai `read()` mengembalikan -1, yang menKitakan "_End of File_" (EOF).
- **Kelemahan**: Menulis/membaca langsung ke _disk_ untuk setiap karakter tunggal sangat lambat jika file berukuran besar karena setiap operasi memicu akses fisik ke perangkat penyimpanan.

<br>

#### Menulis dan Membaca Data Teks ke/dari File menggunakan `BufferedWriter` dan `BufferedReader` (dengan buffering)

Ini adalah cara yang lebih efisien dan direkomendasikan untuk menangani file teks.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileTeksBuffer {
    private static final String NAMA_FILE = "catatan_buffer.txt";

    public static void main(String[] args) {
        // Menulis ke file dengan buffering
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(NAMA_FILE))) {
            writer.write("Java OOP adalah fundamental.");
            writer.newLine(); // Menulis karakter baris baru
            writer.write("Input/Output adalah bagian penting dari interaksi aplikasi.");
            writer.newLine();
            writer.write("Gunakan buffering untuk performa yang lebih baik.");
            System.out.println("Data berhasil ditulis ke " + NAMA_FILE + " dengan buffering.");
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat menulis ke file: " + e.getMessage());
        }

        // Membaca dari file dengan buffering
        try (BufferedReader reader = new BufferedReader(new FileReader(NAMA_FILE))) {
            String baris;
            System.out.println("\nMembaca data dari " + NAMA_FILE + " dengan buffering:");
            while ((baris = reader.readLine()) != null) { // Membaca per baris
                System.out.println(baris);
            }
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat membaca dari file: " + e.getMessage());
        }
    }
}
```

**Penjelasan:**

- **Abstraksi Baris Baru**: Daripada mengetik manual `\n`, kita menggunakan `writer.newLine()`. Metode ini lebih aman karena secara otomatis menyesuaikan dengan sistem operasi.
- **`readLine()` yang Praktis**: Pada sisi pembacaan, `BufferedReader` menyediakan metode `readLine()`. Kita tidak lagi berurusan dengan integer/karakter tunggal, melainkan langsung mendapatkan satu baris utuh sebagai `String`.
- **Logika Berhenti**: Karena `readLine()` mengembalikan objek `String`, indikator akhir filenya bukan lagi -1, melainkan `null`.

<br>

### Byte Stream

Jika `Character Stream` fokus pada teks, `Byte Stream` adalah tentang efisiensi data biner. Di sini kita melihat dua level abstraksi: menangani _byte_ mentah dan menangani tipe data Java yang terstruktur.

#### Menulis dan Membaca Data Biner ke/dari File menggunakan `FileOutputStream` dan `FileInputStream`

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileBinerDasar {
    private static final String NAMA_FILE = "data_biner.bin";

    public static void main(String[] args) {
        // Data biner contoh (array byte)
        byte[] dataUntukDitulis = {72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100}; // "Hello World" dalam ASCII

        // Menulis data biner ke file
        try (FileOutputStream fos = new FileOutputStream(NAMA_FILE)) {
            fos.write(dataUntukDitulis);
            System.out.println("Data biner berhasil ditulis ke " + NAMA_FILE);
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat menulis data biner: " + e.getMessage());
        }

        // Membaca data biner dari file
        try (FileInputStream fis = new FileInputStream(NAMA_FILE)) {
            int byteDibaca;
            System.out.println("\nMembaca data biner dari " + NAMA_FILE + ":");
            while ((byteDibaca = fis.read()) != -1) {
                System.out.print((char) byteDibaca); // Menampilkan sebagai karakter (jika bisa dibaca)
            }
            System.out.println(); // Baris baru setelah selesai membaca
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat membaca data biner: " + e.getMessage());
        }
    }
}
```

Kode ini menunjukkan cara memindahkan data biner dalam bentuk aslinya tanpa modifikasi.

- **_Raw Data Handling_**: Kita menulis sebuah `byte[]`. Java tidak peduli apakah array ini berisi teks ASCII, potongan piksel gambar, atau suara. Ia hanya memindahkan angka-angka biner tersebut ke disk.

- **Proses _Read/Write_**: Sama seperti pada _Character Stream_, kita menggunakan _loop_ `while` dengan kondisi -1 sebagai tKita akhir file.

- **Casting Manual**: Saat membaca, kita melakukan `(char) byteDibaca`. Ini hanya berfungsi karena angka yang kita simpan kebetulan adalah kode ASCII. Jika file tersebut adalah file gambar, melakukan casting ke char akan menghasilkan karakter acak yang tidak terbaca.

<br>

#### Menulis dan Membaca Tipe Data Primitif dengan `DataOutputStream` dan `DataInputStream`

```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class DataStreamContoh {
    private static final String NAMA_FILE = "data_primitif.dat";

    public static void main(String[] args) {
        // Menulis tipe data primitif
        try (FileOutputStream fos = new FileOutputStream(NAMA_FILE);
             DataOutputStream dos = new DataOutputStream(fos)) {

            dos.writeInt(12345);
            dos.writeDouble(3.14159);
            dos.writeBoolean(true);
            dos.writeUTF("Ini adalah sebuah string UTF-8."); // Untuk string

            System.out.println("Tipe data primitif berhasil ditulis ke " + NAMA_FILE);
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat menulis data primitif: " + e.getMessage());
        }

        // Membaca tipe data primitif
        try (FileInputStream fis = new FileInputStream(NAMA_FILE);
             DataInputStream dis = new DataInputStream(fis)) {

            int nilaiInt = dis.readInt();
            double nilaiDouble = dis.readDouble();
            boolean nilaiBoolean = dis.readBoolean();
            String nilaiString = dis.readUTF();

            System.out.println("\nMembaca data primitif dari " + NAMA_FILE + ":");
            System.out.println("Integer: " + nilaiInt);
            System.out.println("Double: " + nilaiDouble);
            System.out.println("Boolean: " + nilaiBoolean);
            System.out.println("String: " + nilaiString);

        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat membaca data primitif: " + e.getMessage());
        }
    }
}
```

- **Abstraksi Tipe Data**: Bayangkan betapa sulitnya menyimpan angka double (64-bit) ke dalam file hanya menggunakan byte stream dasar. Kita harus memecah angka tersebut menjadi 8 byte secara manual. `DataOutputStream` melakukan "keajaiban" itu untuk Kita melalui metode seperti `writeDouble()` dan `writeUTF()`.

- **Urutan Itu Penting**: Dalam `DataStream`, data disimpan secara sekuensial tanpa label. Artinya, urutan saat membaca harus sama persis dengan urutan saat menulis. Jika Kita menulis Int lalu `Double`, namun mencoba membaca `Double` terlebih dahulu, hasilnya akan berupa sampah data atau error.

<br>

#### Serialisasi dan Deserialisasi Objek dengan `ObjectOutputStream` dan `ObjectInputStream`

_Serialisasi_ adalah proses mengubah status objek menjadi aliran byte agar dapat disimpan ke dalam file, database, atau dikirim melalui jaringan. _Deserialisasi_ adalah kebalikannya: membangun kembali objek hidup dari aliran byte tersebut.

**1. Kontrak Serializable**

- **Marker Interface**: _class_ `Mahasiswa` harus mengimplementasikan _interface_ `Serializable`. Ini adalah instruksi kepada JVM bahwa objek ini "diizinkan" untuk diubah menjadi biner. Tanpa ini, program akan melempar `NotSerializableException`.

- `serialVersionUID`: Angka ini berfungsi sebagai "ID Versi". Jika Anda mengubah struktur _class_ (misal menambah _field_ baru), ID ini memastikan bahwa file lama yang disimpan masih kompatibel dengan _class_ yang baru.

**2. Kata Kunci `transient`**

- **Keamanan & Privasi**: Field yang ditandai sebagai `transient` (seperti `kataSandi`) akan diabaikan selama proses serialisasi.
- **Hasil**: Saat objek dibaca kembali (_deserialisasi_), field `transient` akan bernilai bawaan (_default_), yaitu null untuk objek atau 0 untuk angka. Ini sangat berguna untuk data sensitif atau data yang tidak perlu disimpan (seperti hasil perhitungan sementara).

**3. Proses Serialisasi (`ObjectOutputStream`)**

- Objek `mahasiswa1` dikonversi menjadi biner dan ditulis ke file menggunakan metode `writeObject()`.
- Perhatikan hirarki pembungkusnya:
  `ObjectOutputStream` (Logika Objek) $\rightarrow$ `FileOutputStream` (Koneksi File).

**4. Proses Deserialisasi (`ObjectInputStream`)**

- `readObject()`: Metode ini mengembalikan tipe data `Object`. Oleh karena itu, kita wajib melakukan _Explicit Casting_ ke tipe aslinya: `(Mahasiswa) ois.readObject()`.
- `ClassNotFoundException`: Saat membaca objek, Java perlu memastikan bahwa class `Mahasiswa` tersedia di dalam program. Jika tidak ada, _exception_ ini akan muncul.

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

// Kelas yang akan diserialisasi
class Mahasiswa implements Serializable {
    private static final long serialVersionUID = 1L; // Direkomendasikan untuk serializable

    private String nama;
    private int nim;
    private transient String kataSandi; // 'transient' berarti tidak akan diserialisasi

    public Mahasiswa(String nama, int nim, String kataSandi) {
        this.nama = nama;
        this.nim = nim;
        this.kataSandi = kataSandi;
    }

    public String getNama() {
        return nama;
    }

    public int getNim() {
        return nim;
    }

    public String getKataSandi() {
        return kataSandi; // Akan null setelah deserialisasi karena transient
    }

    @Override
    public String toString() {
        return "Mahasiswa{" +
               "nama='" + nama + '\'' +
               ", nim=" + nim +
               ", kataSandi='" + kataSandi + '\'' + // Akan null setelah deserialisasi
               '}';
    }
}

public class ObjekSerialisasi {
    private static final String NAMA_FILE = "objek_mahasiswa.ser";

    public static void main(String[] args) {
        Mahasiswa mahasiswa1 = new Mahasiswa("Budi Santoso", 10123, "rahasia123");

        // Serialisasi objek
        try (FileOutputStream fos = new FileOutputStream(NAMA_FILE);
             ObjectOutputStream oos = new ObjectOutputStream(fos)) {

            oos.writeObject(mahasiswa1);
            System.out.println("Objek Mahasiswa berhasil diserialisasi ke " + NAMA_FILE);
            System.out.println("Objek asli: " + mahasiswa1);
        } catch (IOException e) {
            System.err.println("Terjadi kesalahan saat serialisasi objek: " + e.getMessage());
        }

        // Deserialisasi objek
        Mahasiswa mahasiswaBaca = null;
        try (FileInputStream fis = new FileInputStream(NAMA_FILE);
             ObjectInputStream ois = new ObjectInputStream(fis)) {

            mahasiswaBaca = (Mahasiswa) ois.readObject();
            System.out.println("\nObjek Mahasiswa berhasil dideserialisasi dari " + NAMA_FILE);
            System.out.println("Objek hasil deserialisasi: " + mahasiswaBaca);
            System.out.println("Kata sandi (setelah deserialisasi): " + mahasiswaBaca.getKataSandi()); // Akan null
        } catch (IOException | ClassNotFoundException e) {
            System.err.println("Terjadi kesalahan saat deserialisasi objek: " + e.getMessage());
        }
    }
}
```

**Catatan:**

- **Objek di dalam Objek**: Jika _class_ `Mahasiswa` memiliki atribut berupa objek lain (misal: `Alamat alamat`), maka _class_ `Alamat` tersebut juga wajib mengimplementasikan `Serializable`. Jika tidak, proses serialisasi akan gagal.

- **`Static Field`**: Variabel `static` tidak akan ikut diserialisasi karena variabel tersebut milik _class_, bukan milik objek (_instance_).

<br>

---

# Pemrograman Jaringan

## Pendahuluan

**Jaringan** itu adalah sistem yang memungkinkan beberapa perangkat untuk **terhubung satu sama lain** menggunakan jalur unik tertentu, agar mereka bisa saling **bertukar informasi**.

Dalam era digital modern, kemampuan aplikasi untuk berkomunikasi melalui jaringan adalah hal yang sangat penting. Dari aplikasi chat sederhana hingga sistem terdistribusi yang kompleks, pemrograman jaringan memungkinkan aplikasi untuk saling berinteraksi, berbagi data, dan menyediakan layanan. Java, dengan pustaka `java.net` yang kaya, menawarkan kerangka kerja Object-Oriented Programming (OOP) yang kuat untuk membangun aplikasi jaringan yang Kital dan skalabel.

---

## Konsep Dasar Pemrograman Jaringan

Pemrograman jaringan pada dasarnya melibatkan dua atau lebih program yang berkomunikasi satu sama lain melalui jaringan. Komunikasi ini mengikuti model client-server.

### Model Client-Server

![alt](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/Client-server-model.svg/1200px-Client-server-model.svg.png)

- **Server:** Sebuah program yang menyediakan layanan, menunggu koneksi dari klien, dan merespons permintaan klien. Server biasanya berjalan terus-menerus.
- **Client:** Sebuah program yang menginisiasi koneksi ke server, membuat permintaan, dan menerima respons dari server. Klien biasanya diaktifkan oleh pengguna.

**Analogi sederhana:**

Bayangkan ketika Kita berada di restoran atau toko. Ketika toko/restoran tersebut dibuka, maka pelayan akan selalu siap untuk melayani pelanggan yang datang.

- Pelanggan akan menjadi **client**, dan pelayan akan menjadi **server**
- Ketika toko dibuka artinya **koneksi** ke jaringan dibuat.
- Saat pelanggan memesan artinya **client** mengirim **request**.
- Setelah itu, pelayan akan memproses pesanan dan mengirimkannya ketika pesanan telah rampung, artinya **pelayan** akan mengirimkan **response**.

<br>

### Protokol Jaringan

Agar klien dan server dapat berkomunikasi, mereka harus berbicara dalam "bahasa" yang sama, yang dikenal sebagai protokol jaringan. Beberapa protokol yang umum digunakan dalam Java meliputi:

![alt](https://media.geeksforgeeks.org/wp-content/uploads/20230406112559/TCP-3.png)

- **TCP** (_Transmission Control Protocol_): Protokol berorientasi koneksi yang **menjamin pengiriman data yang Kital, berurutan, dan bebas kesalahan**.Seperti menelepon teman. Sebelum bicara, Kita harus men-_dial_ nomor dan memastikan teman Kita mengangkat telepon (proses _handshake_ atau membangun koneksi). Setelah tersambung, percakapan Kita terjamin sampai, berurutan, dan jika ada yang terlewat, Kita bisa meminta diulang. Jika telepon mati, Kita harus mendial lagi. Cocok untuk aplikasi yang memerlukan keKitalan tinggi seperti transfer file, web Browse, atau email.
  - **Kapan dipakai?** Untuk komunikasi yang sangat penting dan harus sampai dengan utuh, seperti:
    - Mengirim email
    - Membuka halaman web
    - Transfer file (Kita tidak mau file rusak kan?)
  - **Keunggulan:** Sangat Kital, data pasti sampai dan utuh.
  - **Kekurangan:** Sedikit lebih lambat karena ada proses "_dial_" dan "memastikan".

<br>

- **UDP** (_User Datagram Protocol_): Protokol tanpa koneksi yang lebih **cepat tetapi tidak menjamin pengiriman data atau urutan data**. Seperti mengirim surat kilat. Kita menulis surat, masukkan ke amplop, dan langsung kirim. Kita tidak perlu memastikan apakah surat itu sampai, atau apakah sampai dalam urutan yang benar jika Kita kirim banyak surat. Mungkin saja suratnya hilang di jalan atau datang duluan yang terakhir Kita kirim. Cocok untuk aplikasi yang sensitif terhadap waktu dan dapat mentolerir kehilangan data, seperti streaming video/audio atau game online real-time.
  - **Kapan dipakai?** Untuk komunikasi yang butuh kecepatan tinggi dan sedikit kehilangan data tidak masalah, seperti:
    - Streaming video/musik (lag sedikit tidak terlalu fatal)
    - Game online real-time (Kita lebih peduli kecepatan daripada setiap paket data sampai)
  - **Keunggulan:** Sangat cepat karena tidak ada proses "mendial" atau "memastikan".
  - **Kekurangan:** Tidak ada jaminan data sampai atau urutan data benar.

### Port dan IP Address

Untuk mengidentifikasi sebuah aplikasi di suatu mesin dalam jaringan, kita membutuhkan dua hal:

![alt](https://miro.medium.com/v2/resize:fit:713/1*inXJyc6F9icN_6nCgrqBwQ.png)

- Alamat IP (_Internet Protocol Address_): Mengidentifikasi perangkat (komputer, server, smartphone) dalam jaringan. Contoh: 192.168.1.1 atau 203.0.113.45.
- Nomor _Port_: Mengidentidentifikasi aplikasi atau layanan spesifik yang berjalan pada perangkat dengan alamat IP tertentu. Contoh: Port 80 untuk HTTP, Port 22 untuk SSH, Port 8080 untuk aplikasi web kustom.

Kombinasi alamat IP dan nomor port (IP Address:Port) dikenal sebagai _socket address_ atau _network address_.

**Analogi Sederhana:**

Setiap rumah punya alamat jalan yang unik (misalnya, Jl. Merdeka No. 10). Ini seperti IP Address di dunia jaringan. IP Address adalah identitas unik setiap perangkat di jaringan.

Tapi di dalam satu rumah, bisa ada banyak orang atau "layanan". Misalnya, di Jl. Merdeka No. 10, ada ruang tamu untuk menerima tamu (web server), ada dapur untuk masak (aplikasi database), dan ada kamar tidur (layanan lain). Nah, nomor pintu untuk setiap ruangan ini disebut Port.

Jadi, ketika Kita ingin menghubungi suatu layanan, Kita perlu tahu Alamat IP (rumah) dan Nomor Port (pintu) yang tepat. Contoh: 192.168.1.1:80 berarti Kita ingin terhubung ke rumah dengan IP 192.168.1.1 melalui pintu nomor 80 (yang biasanya untuk layanan web).

### Socket

_Socket_ adalah titik akhir (_endpoint_) komunikasi. Dalam Java, sebuah _socket_ adalah objek yang mewakili koneksi antara dua program di jaringan. Jika perangkat adalah rumah, dan alamat IP serta port adalah pintu, maka Socket adalah jendela komunikasi yang kita buka untuk mengirim dan menerima data.

- **Server _Socket_:** Digunakan oleh server untuk mendengarkan koneksi masuk dari klien. Ini adalah **jendela** yang dibuka oleh "pelayan" (server) untuk mendengarkan apakah ada "pelanggan" (klien) yang ingin masuk dan terhubung. Ketika ada pelanggan yang datang, server akan membuat jendela baru khusus untuk pelanggan itu.
- **Client _Socket_:** Digunakan oleh klien untuk membuat koneksi ke server. Ini adalah **jendela** yang dibuka oleh "pelanggan" (klien) untuk menghubungi jendela pelayan (server) dan memulai percakapan.

---

## Class-class Kunci pada Networking

Java menyediakan kelas-kelas inti untuk pemrograman jaringan di paket `java.net`.

#### Kelas untuk Alamat IP

`InetAddress`merupakan kelas representasi alamat IP. Metode kunci:

- `static InetAddress getByName(String host)`: Mengembalikan objek InetAddress untuk nama host atau alamat IP tertentu.
- `static InetAddress getLocalHost()`: Mengembalikan objek InetAddress untuk alamat IP mesin lokal.
- `String getHostName()`: Mengembalikan nama host.
- `String getHostAddress()`: Mengembalikan representasi string dari alamat IP.

### Kelas untuk TCP (Socket Berorientasi Koneksi)

#### `ServerSocket` (untuk Server)

Kelas ini digunakan oleh server untuk mendengarkan permintaan koneksi klien pada port tertentu.

- **Konstruktor Kunci:**
  - `ServerSocket(int port)`: Membuat server socket yang mendengarkan pada port yang ditentukan.
- **Metode Kunci:**
  - `Socket accept()`: Menunggu (memblokir) hingga koneksi klien diterima. Ketika klien terhubung, metode ini mengembalikan objek Socket baru yang mewakili koneksi dengan klien tersebut.
  - `void close()`: Menutup server socket.

#### `Socket` (untuk Klien dan Server)

Kelas ini merepresentasikan salah satu ujung koneksi TCP. Klien menggunakannya untuk membuat koneksi, dan server menggunakannya (objek yang dikembalikan oleh `accept()`) untuk berkomunikasi dengan klien yang terhubung.

- **Konstruktor Kunci (untuk Klien):**
  - `Socket(String host, int port)`: Membuat socket dan mencoba terhubung ke host dan port yang ditentukan.
- **Metode Kunci (untuk Input/Output Data):**
  - `InputStream getInputStream()`: Mengembalikan InputStream untuk membaca data yang dikirim oleh ujung koneksi lainnya.
  - `OutputStream getOutputStream()`: Mengembalikan OutputStream untuk menulis data ke ujung koneksi lainnya.
  - `void close()`: Menutup socket.

**PENTING!!:** Setelah mendapatkan `InputStream` dan `OutputStream` dari `Socket`, Kita akan menggunakan kelas-kelas I/O Java yang telah kita bahas sebelumnya (`BufferedReader`, `PrintWriter`, `DataInputStream`, dll.) untuk membaca dan menulis data yang sebenarnya.

### Kelas untuk UDP (Socket Tanpa Koneksi)

#### `DatagramSocket`

Kelas ini digunakan untuk mengirim dan menerima paket data UDP (`DatagramPacket`).

- **Konstruktor Kunci:**
  - `DatagramSocket()`: Membuat datagram socket yang mengikat ke port yang tersedia di mesin lokal.
  - `DatagramSocket(int port)`: Membuat datagram socket yang mengikat ke port yang ditentukan.
- **Metode Kunci:**
  - `void send(DatagramPacket p)`: Mengirim datagram packet.
  - `void receive(DatagramPacket p)`: Menerima datagram packet. Metode ini memblokir hingga paket diterima.
  - `void close()`: Menutup datagram socket.

#### `DatagramPacket`

Kelas ini merepresentasikan paket data yang akan dikirim atau diterima melalui `DatagramSocket`.

- **Konstruktor Kunci:**
  - `DatagramPacket(byte[] buf, int length, InetAddress address, int port)`: Untuk mengirim paket. buf adalah data, address adalah alamat IP tujuan, port adalah port tujuan.
  - `DatagramPacket(byte[] buf, int length)`: Untuk menerima paket. buf adalah buffer tempat data yang diterima akan disimpan.
- **Metode Kunci:**
  - `InetAddress getAddress()`: Mengembalikan alamat IP pengirim/penerima.
  - `int getPort()`: Mengembalikan nomor port pengirim/penerima.
  - `byte[] getDat`a(): Mengembalikan array byte yang berisi data.
  - `int getLength()`: Mengembalikan panjang data yang valid dalam array.

---

## Analogi atau Studi Kasus Nyata

### TCP (Transmission Control Protocol) - Sistem Telepon

**Bayangkan Kita ingin menelepon teman**. Pertama, Kita harus membangun koneksi (mendial nomor). Setelah terhubung, Kita berdua dapat berbicara (mengirim data) dan yakin bahwa apa yang Kita katakan akan didengar dalam urutan yang benar. Jika koneksi terputus, Kita harus menelepon lagi. Ini adalah komunikasi yang Kital, berurutan, dan terkonfirmasi.

- **Server Socket** adalah telepon teman Kita yang menunggu panggilan masuk (mendengarkan).
- **Socket** adalah koneksi telepon yang sudah terjalin antara Kita dan teman Kita.

### UDP (User Datagram Protocol) - Kantor Pos

**Bayangkan Kita mengirim surat melalui kantor pos**. Kita menulis surat, menaruhnya di amplop, menulis alamat dan melemparkannya ke kotak surat. Kita tidak tahu pasti kapan atau bahkan apakah surat itu akan sampai, dan Kita tidak akan tahu urutan pengirimannya jika Kita mengirim beberapa surat sekaligus. Ini lebih cepat karena tidak ada proses _handshake_ awal, tetapi kurang Kital.

- **DatagramSocket** adalah kantor pos Kita yang menerima dan mengirimkan surat.
- **DatagramPacket** adalah surat itu sendiri, berisi pesan dan alamat tujuan.

### Studi Kasus Nyata: Aplikasi Chat Sederhana

Mari kita bayangkan membangun aplikasi chat sederhana.

#### Menggunakan TCP (Untuk KeKitalan Pesan):

- **Server Chat:**
  - Membuat `ServerSocket` pada port tertentu (misal: 12345).
  - Dalam _loop_ tak terbatas, server memanggil `accept()` untuk menunggu klien terhubung.
  - Ketika klien terhubung, `accept()` mengembalikan `Socket` baru. Server kemudian membuat _thread_ baru untuk menangani komunikasi dengan klien ini. Ini penting agar server dapat menangani banyak klien secara bersamaan.
  - Dalam _thread_ klien, server mendapatkan `InputStream` dan `OutputStream` dari `Socket` untuk membaca pesan dari klien dan mengirim pesan kembali.
  - Server dapat menyiarkan pesan dari satu klien ke semua klien lain yang terhubung.
- **Client Chat:**
  - Membuat `Socket` dan mencoba terhubung ke alamat IP server dan port 12345.
  - Setelah terhubung, klien mendapatkan `InputStream` dan `OutputStream` dari `Socket` untuk mengirim pesan ke server dan menerima pesan dari server.
  - Klien biasanya akan memiliki dua thread internal: satu untuk mengirim pesan (membaca dari input pengguna dan menulis ke `socket`) dan satu lagi untuk menerima pesan (membaca dari `socket` dan menampilkannya).

#### Menggunakan UDP (Untuk Kecepatan atau Siaran):

Jika kita ingin membuat aplikasi chat yang sangat ringan atau untuk mengirim pesan siaran ke semua perangkat di jaringan lokal tanpa perlu koneksi berkelanjutan, UDP bisa menjadi pilihan.

- **Klien/Server (P2P):** Setiap partisipan bisa memiliki `DatagramSocket`.
  - Untuk mengirim pesan: Buat DatagramPacket dengan pesan, alamat IP tujuan, dan port, lalu kirim menggunakan DatagramSocket.send().
  - Untuk menerima pesan: Buat DatagramPacket kosong dengan buffer, lalu panggil DatagramSocket.receive() untuk menunggu paket.

**Kekurangan:** Pesan bisa hilang atau datang tidak berurutan. Kita perlu mekanisme sendiri untuk menangani keKitalan jika diperlukan.

---

## Contoh Kode

Berikut adalah contoh kode untuk TCP dan UDP.

### Contoh TCP: Server dan Klien Sederhana

Buat `EchoServer.java` sebagai Server TCP.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class EchoServer {
    private static final int PORT = 12345; // Port server akan mendengarkan

    public static void main(String[] args) {
        System.out.println("Echo Server dimulai pada port " + PORT + "...");

        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            // Server terus-menerus mendengarkan koneksi klien
            while (true) {
                System.out.println("Menunggu koneksi klien...");
                // accept() memblokir hingga klien terhubung
                Socket clientSocket = serverSocket.accept();
                System.out.println("Klien terhubung: " + clientSocket.getInetAddress().getHostAddress());

                // Membuat thread baru untuk setiap klien yang terhubung
                // Ini memungkinkan server menangani banyak klien secara bersamaan
                new Thread(() -> handleClient(clientSocket)).start();
            }
        } catch (IOException e) {
            System.err.println("Kesalahan server: " + e.getMessage());
            e.printStackTrace();
        }
    }

    private static void handleClient(Socket clientSocket) {
        try (
            // Mendapatkan input stream dari klien
            BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
            // Mendapatkan output stream ke klien
            PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true); // 'true' untuk auto-flush
        ) {
            String inputLine;
            // Membaca input dari klien baris demi baris
            while ((inputLine = in.readLine()) != null) {
                System.out.println("Diterima dari klien " + clientSocket.getInetAddress().getHostAddress() + ": " + inputLine);
                // Mengirim kembali (echo) input ke klien
                out.println("Echo: " + inputLine);
                if (inputLine.equalsIgnoreCase("exit")) {
                    break; // Keluar dari loop jika klien mengetik 'exit'
                }
            }
            System.out.println("Klien " + clientSocket.getInetAddress().getHostAddress() + " terputus.");
        } catch (IOException e) {
            System.err.println("Kesalahan penanganan klien " + clientSocket.getInetAddress().getHostAddress() + ": " + e.getMessage());
            e.printStackTrace();
        } finally {
            try {
                clientSocket.close(); // Pastikan socket klien ditutup
            } catch (IOException e) {
                System.err.println("Gagal menutup socket klien: " + e.getMessage());
            }
        }
    }
}
```

<br>

Buat `EchoClient.java` sebagai Klien TCP.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
import java.net.UnknownHostException;
import java.util.Scanner;

public class EchoClient {
private static final String SERVER_ADDRESS = "localhost"; // Ganti dengan IP server jika berbeda
private static final int SERVER_PORT = 12345;

    public static void main(String[] args) {
        System.out.println("Mencoba terhubung ke server " + SERVER_ADDRESS + " pada port " + SERVER_PORT + "...");

        try (
            // Membuat socket klien dan terhubung ke server
            Socket socket = new Socket(SERVER_ADDRESS, SERVER_PORT);
            // Mendapatkan output stream untuk mengirim data ke server
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true); // 'true' untuk auto-flush
            // Mendapatkan input stream untuk membaca data dari server
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            // Scanner untuk membaca input dari keyboard pengguna
            Scanner scanner = new Scanner(System.in);
        ) {
            System.out.println("Berhasil terhubung ke server. Ketik 'exit' untuk keluar.");
            String userInput;
            String serverResponse;

            while (true) {
                System.out.print("Kita: ");
                userInput = scanner.nextLine(); // Membaca input dari keyboard

                out.println(userInput); // Mengirim input ke server

                if (userInput.equalsIgnoreCase("exit")) {
                    break; // Keluar dari loop jika pengguna mengetik 'exit'
                }

                // Membaca respons dari server
                serverResponse = in.readLine();
                System.out.println("Server: " + serverResponse);
            }
            System.out.println("Koneksi ditutup.");
        } catch (UnknownHostException e) {
            System.err.println("Host tidak ditemukan: " + SERVER_ADDRESS);
            e.printStackTrace();
        } catch (IOException e) {
            System.err.println("Kesalahan I/O saat terhubung ke server: " + e.getMessage());
            e.printStackTrace();
        }
    }

}

```

<br>

**Cara Menjalankan (1): Manual**

1. Compile kedua file: `javac EchoServer.java EchoClient.java`
2. Jalankan server terlebih dahulu: `java EchoServer`
3. Di terminal/CMD terpisah, jalankan klien: `java EchoClient`
4. Ketik pesan di klien, dan server akan "menggema" kembali pesan tersebut.

**Cara Menjalankan (2): Dengan Intellij IDE**

1. Buka file server, pilih ikon run di sebelah kiri baris `public static void main(String[] args)`.
2. Buka file client, pilih ikon run di sebelah kiri baris `public static void main(String[] args)`.

<br>

### Contoh UDP: Pengirim dan Penerima Pesan Sederhana

Buat file `UDPSender.java` sebagai Pengirim UDP.

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.util.Scanner;

public class UDPSender {
    private static final String RECEIVER_ADDRESS = "localhost"; // Ganti dengan IP penerima
    private static final int RECEIVER_PORT = 9876; // Port penerima mendengarkan

    public static void main(String[] args) {
        System.out.println("UDP Sender siap mengirim pesan.");

        try (DatagramSocket socket = new DatagramSocket(); // Socket pengirim
             Scanner scanner = new Scanner(System.in)) {

            InetAddress address = InetAddress.getByName(RECEIVER_ADDRESS); // Alamat tujuan

            String message;
            while (true) {
                System.out.print("Ketik pesan untuk dikirim (atau 'exit' untuk keluar): ");
                message = scanner.nextLine();

                if (message.equalsIgnoreCase("exit")) {
                    System.out.println("Keluar dari UDP Sender.");
                    break;
                }

                byte[] buffer = message.getBytes(); // Konversi pesan ke byte
                // Buat DatagramPacket dengan data, panjang, alamat, dan port tujuan
                DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, RECEIVER_PORT);

                socket.send(packet); // Kirim paket
                System.out.println("Pesan terkirim: '" + message + "'");
            }
        } catch (IOException e) {
            System.err.println("Kesalahan I/O UDP Sender: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

Buat file `UDPReceiver.java` sebagai Penerima UDP.

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPReceiver {
    private static final int PORT = 9876; // Port penerima akan mendengarkan
    private static final int BUFFER_SIZE = 1024; // Ukuran buffer untuk menerima data

    public static void main(String[] args) {
        System.out.println("UDP Receiver dimulai pada port " + PORT + "...");

        try (DatagramSocket socket = new DatagramSocket(PORT)) {
            byte[] buffer = new byte[BUFFER_SIZE]; // Buffer untuk menyimpan data yang diterima

            while (true) {
                // Buat DatagramPacket untuk menerima data
                DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
                System.out.println("Menunggu paket UDP...");

                socket.receive(packet); // Memblokir hingga paket diterima

                // Ekstrak data dari paket
                String receivedMessage = new String(packet.getData(), 0, packet.getLength());
                InetAddress senderAddress = packet.getAddress();
                int senderPort = packet.getPort();

                System.out.println("Diterima dari " + senderAddress.getHostAddress() + ":" + senderPort + " -> " + receivedMessage);

                // Opsional: Kirim respons (uncomment jika perlu)
                // String response = "ACK: " + receivedMessage;
                // byte[] responseBuffer = response.getBytes();
                // DatagramPacket responsePacket = new DatagramPacket(responseBuffer, responseBuffer.length, senderAddress, senderPort);
                // socket.send(responsePacket);
            }
        } catch (IOException e) {
            System.err.println("Kesalahan I/O UDP Receiver: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

<br>

**Cara Menjalankan (1): Manual**

1. Compile kedua file: `javac UDPSender.java UDPReceiver.java`
2. Jalankan penerima terlebih dahulu: `java UDPReceiver`
3. Di terminal/CMD terpisah, jalankan pengirim: `java UDPSender`
4. Ketik pesan di pengirim, dan pesan akan muncul di penerima.

**Cara Menjalankan (2): Dengan Intellij IDE**

1. Buka file sender, pilih ikon run di sebelah kiri baris `public static void main(String[] args)`.
2. Buka file receiver, pilih ikon run di sebelah kiri baris `public static void main(String[] args)`.

---

# The End

```

Have a nice day 👋

```
