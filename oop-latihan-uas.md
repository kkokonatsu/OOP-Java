# Latihan UAS PBO

by explorestat

## Materi Singkat

### Pemrograman Multithreading

_Multithreading_ adalah kemampuan program untuk menjalankan beberapa bagian dari kode **secara bersamaan** (konkuren).

Pendekatan _multithreading_ mampu meningkatkan efisiensi dan responsivitas aplikasi, terutama dalam tugas-tugas yang membutuhkan banyak perhitungan atau menunggu operasi I/O, karena program dapat menggunakan sumber daya CPU secara lebih optimal dan tidak perlu berhenti total saat menunggu operasi eksternal selesai.

#### Konsep Thread Utama dan Thread Tambahan

Di setiap awal titik masuk program Java, di mana method **main()** dieksekusi.

```java
public static void main(String[] args) {
        // Kode di bawah ini akan dieksekusi oleh main thread
}
```

Maka, setiap kali menjalankan sebuah program Java, baik itu program sederhana "Hello World" atau aplikasi kompleks, _Java Virtual Machine_ (JVM) akan secara otomatis membuat dan menjalankan **satu thread pertama yang disebut Main thread**.

![alt](https://www.scientecheasy.com/wp-content/uploads/2020/07/java-join-method.png)

Dari Main Thread inilah, thread tambahan (_child threads_) dapat dibuat dan diluncurkan untuk menjalankan tugas **secara paralel** atau bersamaan dengan Main Thread.

#### Cara Memulai Thread Baru

Ada beberapa cara utama untuk membuat dan memulai thread baru di Java:

- **Cara 1: Meng-extend Class Thread**: Anda membuat class baru yang mewarisi `java.lang.Thread` dan meng-_override_ method `run()`. Kode yang akan dieksekusi di thread baru ditempatkan di dalam method `run()`. Thread dimulai dengan memanggil method `start()` pada objek class Anda.

```java
class MyThread extends Thread {
    public void run() {
        // Kode yang akan dieksekusi oleh thread ini
    }
}
// Untuk memulai: new MyThread().start();
```

- **Cara 2: Mengimplementasikan Interface `Runnable`**: Anda membuat class baru yang mengimplementasikan interface `java.lang.Runnable` dan menyediakan implementasi untuk method `run()`. Objek dari class ini kemudian diteruskan ke constructor `Thread`, lalu `start()` dipanggil pada objek Thread tersebut.

```java
class MyRunnable implements Runnable {
    public void run() {
        // Kode yang akan dieksekusi oleh thread ini
    }
}
// Untuk memulai: new Thread(new MyRunnable()).start();
```

- **Cara 3: Implementasi `Runnable` dengan Lambda Expression**: Sejak Java 8, Anda bisa menggunakan lambda expression untuk implementasi `Runnable` secara ringkas, terutama untuk tugas-tugas singkat.

```java
// Untuk memulai: new Thread(() -> { /* Kode */ }).start();
```

<br>

**Perbandingan:** Mengimplementasikan `Runnable` seringkali lebih disarankan karena Java tidak mendukung _multiple inheritance_ dari class. Dengan `Runnable`, class Anda masih bisa mewarisi class lain sambil tetap menyediakan fungsionalitas untuk dijalankan sebagai thread.

#### Membaca dan Menuliskan Hasil Program Multithreading

Membaca program multithreading memerlukan pemahaman tentang bagaimana CPU menjadwalkan thread. Karena eksekusi thread bersifat non-deterministik (tidak selalu berurutan), output dari program multithreading dapat bervariasi setiap kali dijalankan, terutama jika ada akses ke sumber daya bersama tanpa sinkronisasi.

#### Menulis Program Multithreading Berdasarkan Deskripsi

Ketika menulis program multithreading berdasarkan deskripsi, fokuslah pada:

1. **Identifikasi Tugas Konkuren:** Bagian mana dari program yang dapat berjalan secara independen atau paralel.

2. **Pembagian Tugas ke Thread:** Tentukan class mana yang akan mengimplementasikan Runnable atau meng-extend Thread.

3. **Manajemen Sumber Daya Bersama:** Pikirkan tentang data yang akan diakses oleh beberapa thread dan bagaimana melindunginya dari race condition (lihat 1.6).

4. **Komunikasi Antar Thread:** Bagaimana thread akan bertukar informasi atau sinyal (lihat wait(), notify()).

**Contoh soal:**

Perhatikan potongan kode berikut:

```java
public class SimpleThread extends Thread {
    private String message;
    public SimpleThread(String msg) { this.message = msg; }
    public void run() {
        System.out.println(message + " is running.");
    }
    public static void main(String[] args) {
        SimpleThread t1 = new SimpleThread("Thread A");
        SimpleThread t2 = new SimpleThread("Thread B");
        t1.start();
        t2.start();
    }
}
```

Manakah pernyataan yang PALING TEPAT mengenai potensi output dari program di atas?
A. Pasti "Thread A is running." diikuti oleh "Thread B is running.".
B. Pasti "Thread B is running." diikuti oleh "Thread A is running.".
**C. Output mungkin bervariasi antara "Thread A is running." dan "Thread B is running." karena eksekusi thread tidak berurutan.**
D. Program akan mengalami error kompilasi karena dua thread dijalankan bersamaan.
E. Program akan mengalami deadlock.

**Jawaban:**
Saat `start()` dipanggil, thread baru akan dijadwalkan oleh JVM. Urutan eksekusi antar thread tidak dijamin, sehingga "Thread A is running." dan "Thread B is running." dapat muncul dalam **urutan acak** atau bahkan bercampur jika ada lebih banyak operasi.

<br>

#### Fungsi join()

Method `join()` pada sebuah objek `Thread` digunakan untuk memastikan bahwa sebuah thread telah menyelesaikan eksekusinya sebelum thread lain (yang memanggil `join()`) melanjutkan eksekusinya. Misalnya, jika Main `Thread` memanggil `threadA.join()`, Main Thread akan berhenti dan menunggu hingga `threadA` selesai, baru kemudian melanjutkan eksekusinya.

contoh singkat penggunaan `join()`

```java
public class JoinExample {

    public static void main(String[] args) {
        System.out.println("Main Thread: Memulai.");

        // Membuat sebuah child thread
        Thread childThread = new Thread(() -> {
            System.out.println("Child Thread: Memulai tugasnya. Saya akan tidur 2 detik...");
            try {
                Thread.sleep(2000); // Child thread simulasi tugas (tidur 2 detik)
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                System.out.println("Child Thread: Terinterupsi!");
            }
            System.out.println("Child Thread: Tugas selesai.");
        });

        // Memulai child thread
        childThread.start();

        // Tanpa join(), main thread akan langsung melanjutkan
        // System.out.println("Main Thread: Saya tidak menunggu child thread.");

        try {
            System.out.println("Main Thread: Saya akan menunggu Child Thread selesai...");
            childThread.join(); // Main Thread berhenti dan menunggu childThread selesai
            System.out.println("Main Thread: Child Thread telah selesai, saya bisa melanjutkan.");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.out.println("Main Thread: Menunggu child terinterupsi!");
        }

        System.out.println("Main Thread: Selesai.");
    }
}
```

**output:**

```bash
Main Thread: Memulai.
Main Thread: Saya akan menunggu Child Thread selesai...
Child Thread: Memulai tugasnya. Saya akan tidur 2 detik...
Child Thread: Tugas selesai.
Main Thread: Child Thread telah selesai, saya bisa melanjutkan.
Main Thread: Selesai.
```

#### Keadaan Deadlock

_Deadlock_ adalah situasi di mana dua atau lebih thread saling menunggu satu sama lain untuk melepaskan resource yang mereka kunci, sehingga tidak ada thread yang bisa melanjutkan eksekusinya. Ini seperti kemacetan lalu lintas di persimpangan di mana tidak ada mobil yang bisa bergerak.

_Kapan Deadlock Terjadi?_ _Deadlock_ dapat terjadi jika keempat kondisi Coffman terpenuhi:

- **Mutual Exclusion:**

  - Artinya: Hanya **satu thread** yang boleh memakai suatu sumber daya (resource) dalam satu waktu.
  - Contoh awam: Kamu dan temanmu ingin menggunakan **satu printer**. Hanya satu orang bisa mencetak pada satu waktu.

- **Hold and Wait** (Pegang dan Tunggu)

  - Artinya: Thread sudah memegang satu resource, tapi masih **menunggu resource lain**.
  - Contoh awam: Kamu sedang memakai printer, lalu kamu juga butuh scanner. Tapi scanner sedang dipakai temanmu — jadi kamu menunggu, sambil tetap memegang printer.

- **No Preemption:**

  - Artinya: Resource **tidak bisa direbut secara paksa** dari thread lain. Hanya yang punya yang bisa melepasnya sendiri.
  - Contoh awam: Kamu tidak bisa memaksa temanmu menyerahkan scanner. Dia harus selesai dulu dan menyerahkannya sendiri.

- **Circular Wait:**

  - Artinya: Ada **rantai saling tunggu** antar thread.
  - Contoh awam:
    - Kamu menunggu scanner dari temanmu,
    - Temanmu menunggu laptop dari orang lain,
    - Orang itu menunggu printer yang sedang kamu pegang.
    - Akhirnya **semua saling tunggu, tidak ada yang bisa lanjut**.

#### Class CompletableFuture

`CompletableFuture` adalah sebuah class di Java yang mirip seperti sebuah janji (_promise_) untuk sebuah hasil yang akan tersedia di masa depan. Ia tidak hanya memberi tahu apakah tugas sudah selesai, tetapi juga memungkinkan Anda untuk mendefinisikan apa yang harus terjadi selanjutnya setelah tugas itu selesai, atau bahkan jika tugas itu gagal. Ini mirip seperti "kontrak" atau "alur kerja" untuk tugas-tugas yang belum selesai.

Ini seperti meminta komputer untuk melakukan sesuatu di belakang layar, sementara program tetap berjalan seperti biasa. Misalnya, kita bisa memulai proses mengambil data dari internet, lalu melanjutkan pekerjaan lain tanpa harus menunggu data itu datang. Setelah datanya siap, `CompletableFuture` akan memberi tahu kita dan menjalankan kode selanjutnya.

Kita juga bisa menggabungkan beberapa tugas asinkron, membuat urutan kerja (_chaining_), atau mengatur hasilnya saat sudah selesai dan semuanya dilakukan secara otomatis di balik layar, bahkan bisa berjalan di thread yang berbeda. Singkatnya, `CompletableFuture` membuat program kita lebih cepat, responsif, dan tidak mudah macet karena menunggu.

Method utama:

- `supplyAsync()`: Digunakan untuk menjalankan tugas asinkron yang akan mengembalikan sebuah hasil, di mana tugas ini dijalankan di common `ForkJoinPool` atau `Executor` yang Anda tentukan.

- `thenApply()`: Memungkinkan Anda untuk mengubah atau memproses hasil dari `CompletableFuture` sebelumnya menjadi tipe data lain secara asinkron.

- `thenAccept()`: Digunakan untuk melakukan sebuah aksi dengan hasil dari `CompletableFuture` sebelumnya, tanpa mengembalikan hasil apa pun ke `CompletableFuture` berikutnya.

- `thenRun()`: Digunakan untuk menjalankan sebuah aksi setelah `CompletableFuture` sebelumnya selesai, tanpa peduli apa hasilnya dan tanpa mengembalikan hasil baru.

- `thenCombine()`: Memungkinkan Anda untuk menggabungkan hasil dari dua `CompletableFuture` independen menjadi satu hasil baru.

- `allOf()`: Mengembalikan `CompletableFuture` baru yang selesai ketika semua `CompletableFuture` yang diberikan telah selesai.

- `anyOf()`: Mengembalikan `CompletableFuture` baru yang selesai ketika salah satu dari `CompletableFuture` yang diberikan telah selesai, dengan hasil dari yang pertama selesai.

#### Class CountDownLatch

`java.util.concurrent.CountDownLatch` adalah alat bantu sinkronisasi di Java yang memungkinkan satu atau beberapa thread untuk menunggu sampai satu set operasi yang dilakukan oleh thread lain selesai.

Bayangkan Anda memiliki beberapa pekerja (thread) yang masing-masing harus menyelesaikan bagian tugasnya, dan ada seorang manajer (thread lain) yang perlu menunggu semua pekerja menyelesaikan tugas mereka sebelum manajer bisa melanjutkan. CountDownLatch adalah sistem hitungan mundur yang mengatur hal ini.

**Cara Kerjanya:**

1. `CountDownLatch` dimulai dengan sebuah hitungan (count). Angka ini mewakili jumlah "kejadian" atau "tugas" yang harus diselesaikan sebelum thread yang menunggu bisa melanjutkan.

2. Setiap kali sebuah thread (pekerja) menyelesaikan bagian tugasnya, ia memanggil metode `countDown()` pada objek `CountDownLatch`. Panggilan ini akan mengurangi hitungan sebanyak satu.

3. Thread yang perlu menunggu (manajer) akan memanggil metode `await()`. Thread ini akan diblokir (berhenti) dan tidak akan melanjutkan eksekusinya sampai hitungan di `CountDownLatch` mencapai nol.

4. Begitu hitungan mencapai nol (artinya semua tugas telah selesai atau semua "kejadian" telah terjadi), semua thread yang memanggil `await()` akan dibebaskan (unblocked) dan dapat melanjutkan eksekusinya.

**Kapan Menggunakan CountDownLatch?**

Ketika Anda perlu memastikan bahwa beberapa tugas independen telah selesai sebelum tugas utama dapat dimulai.

Untuk memulai beberapa thread secara bersamaan (misalnya, di awal pengujian kinerja). Anda bisa membuat `CountDownLatch` dengan hitungan 1. Semua thread pekerja memanggil `await()`. Thread utama mengurangi hitungan saat siap.

<br>

### Pemrograman Input/Output (I/O)

Pemrograman I/O di Java melibatkan aliran data (stream) antara program dan sumber/tujuan eksternal seperti file, jaringan, atau konsol.

#### Fungsi Stream

Stream dalam pemrograman I/O adalah urutan data. Bayangkan sebuah pipa air:

- **Input Stream:** Air mengalir dari sumber ke wadah (program Anda). Ini digunakan untuk membaca data.
- **Output Stream:** Air mengalir dari wadah (program Anda) ke tujuan. Ini digunakan untuk menulis data.

#### Buffer

**Buffer** adalah area memori sementara yang digunakan untuk menyimpan data saat sedang dipindahkan antara dua lokasi. Sebagai aturan umum, selalu anggap bahwa Anda perlu menggunakan buffer untuk operasi I/O yang melibatkan file, jaringan, atau perangkat eksternal lainnya. Java Virtual Machine (JVM) dan sistem operasi sangat dioptimalkan untuk bekerja dengan buffer.

#### Class-Class Utama dalam I/O

**Byte Streams** (untuk data biner)

Digunakan untuk membaca dan menulis data biner, seperti gambar, audio, video, atau file yang tidak berformat teks. Kelas-kelas utamanya adalah InputStream dan OutputStream.

- `InputStream` (abstract base class untuk membaca byte)
  - `FileInputStream`: Membaca byte dari file.
  - `BufferedInputStream`: Menambahkan buffering untuk efisiensi pembacaan byte.
- `OutputStream` (abstract base class untuk menulis byte)
  - `FileOutputStream`: Menulis byte ke file.
  - `BufferedOutputStream`: Menambahkan buffering untuk efisiensi penulisan byte.

**Character Streams** (untuk data teks):

Digunakan untuk membaca dan menulis data teks. Secara otomatis menangani encoding karakter (misalnya, UTF-8, ISO-8859-1). Kelas-kelas utamanya adalah `Reader` dan `Writer`. Ini adalah preferensi untuk menangani teks.

- `Reader` (abstract base class untuk membaca karakter)

  - `FileReader`: Membaca karakter dari file.
  - `BufferedReader`: Membaca karakter secara efisien dengan buffering, sering digunakan dengan readLine().
  - `InputStreamReader`: Menjembatani byte stream ke character stream, memungkinkan spesifikasi encoding.

- `Writer` (abstract base class untuk menulis karakter)
  - `FileWriter`: Menulis karakter ke file.
  - `PrintWriter`: Menulis teks yang diformat (seperti `println()`) ke stream.
  - `BufferedWriter`: Menulis karakter secara efisien dengan buffering.
  - `OutputStreamWriter`: Menjembatani byte stream ke character stream, memungkinkan spesifikasi encoding.

**Object Streams** (untuk objek Java):

- `ObjectInputStream`: Membaca objek Java yang telah diserialisasi.
- `ObjectOutputStream`: Menulis objek Java (serialisasi).

#### Menulis Program I/O Teks

Untuk menulis program I/O teks, langkah-langkah umumnya adalah:

1. **Membuka Stream:** Buat objek dari class stream yang sesuai (misalnya, FileReader, FileWriter, atau yang dibungkus dengan BufferedReader/PrintWriter).

2. **Membaca/Menulis Data:** Gunakan method read()/readLine() untuk input atau write()/print()/println() untuk output.

3. **Menutup Stream:** Selalu panggil close() untuk membebaskan sumber daya dan memastikan data tertulis sepenuhnya. Gunakan blok try-with-resources untuk penutupan otomatis dan penanganan pengecualian yang lebih baik.

**Contoh Soal:**

Perhatikan potongan kode berikut:

```java
import java.io.*;
public class FileChecker {
    public static void main(String[] args) {
        try {
            FileReader fr = new FileReader("nonexistent.txt");
            BufferedReader br = new BufferedReader(fr);
            String line = br.readLine();
            System.out.println(line);
            br.close();
            fr.close();
        } catch (FileNotFoundException e) {
            System.out.println("File tidak ditemukan.");
        } catch (IOException e) {
            System.out.println("Terjadi error I/O.");
        }
    }
}
```

Jika file "nonexistent.txt" tidak ada, apa yang akan menjadi output dari program di atas?
A. "Terjadi error I/O."
B. null
C. Program akan berhenti tanpa output karena error.
**D. "File tidak ditemukan."**
E. Pesan kesalahan dari stack trace.

**Jawaban:**
`FileReader` akan mencoba membuka file "nonexistent.txt". Karena file tidak ada, ini akan melempar `FileNotFoundException`, yang akan ditangkap oleh blok catch (`FileNotFoundException e`), sehingga mencetak "File tidak ditemukan.".

Dalam pemrograman I/O, apa fungsi utama dari InputStreamReader atau OutputStreamWriter?
A. Mengkonversi aliran byte menjadi aliran objek Java.
B. Mengkonversi aliran karakter menjadi aliran byte.
**C. Menjembatani aliran byte dengan aliran karakter, memungkinkan konversi encoding**
D. Menyediakan fungsionalitas buffering untuk meningkatkan performa.
E. Membaca dan menulis data primitif (int, double, dll).

**Jawaban:**
`InputStreamReader` dan `OutputStreamWriter` adalah "bridge" stream. Mereka mengkonversi aliran byte menjadi aliran karakter, dan sebaliknya, dengan mempertimbangkan encoding karakter.

#### Memilih Class yang Tepat dalam Pemrograman I/O

Pemilihan class I/O yang tepat tergantung pada jenis data dan tujuan:

- **Teks vs. Biner:** Gunakan character streams (`Reader`/`Writer`) untuk teks dan byte streams (`InputStream`/`OutputStream`) untuk data biner.

- **Buffering:** Untuk efisiensi, selalu gunakan class `BufferedXxx` (misalnya `BufferedReader`, `BufferedWriter`) yang membungkus stream dasar, terutama untuk operasi file besar.

- **Objek Java:** Gunakan `ObjectInputStream` dan `ObjectOutputStream` untuk membaca/menulis objek Java.

- **Input User/Konsol:** Scanner untuk input yang mudah dari konsol, `BufferedReader` untuk input baris yang lebih efisien.

**Contoh Soal:**

Sebuah tim pengembang sedang membangun aplikasi manajemen perpustakaan sederhana menggunakan Java. Mereka memiliki beberapa kebutuhan I/O untuk fitur-fitur berikut:

Setiap buku memiliki data: judul, penulis, dan tahun terbit. Mereka ingin bisa menyimpan daftar buku ke file dan membacanya kembali dalam bentuk objek Java agar mudah diproses.

Class I/O apa yang paling tepat digunakan untuk menyimpan dan membaca data buku dalam bentuk objek?

A. BufferedReader dan BufferedWriter
B. FileReader dan FileWriter
**C. ObjectOutputStream dan ObjectInputStream**
D. PrintWriter dan Scanner

<br>

### Pemrograman Jaringan

Pemrograman jaringan di Java memungkinkan aplikasi berkomunikasi melalui jaringan menggunakan protokol seperti TCP atau UDP.

#### Class-Class Utama dalam Pemrograman Jaringan

- `java.net.Socket`: Merepresentasikan satu sisi koneksi TCP. Digunakan oleh klien untuk terhubung ke server, dan oleh server untuk berkomunikasi dengan klien yang terhubung.

- `java.net.ServerSocket`: Digunakan oleh sisi server untuk mendengarkan koneksi TCP yang masuk dari klien pada port tertentu.

- `java.net.DatagramSocket`: Digunakan untuk mengirim dan menerima paket data connectionless (UDP).

- `java.net.DatagramPacket`: Merepresentasikan paket data yang akan dikirim atau diterima melalui `DatagramSocket`.

- `java.net.InetAddress`: Merepresentasikan alamat IP. Digunakan untuk bekerja dengan alamat host dan resolusi nama.

![alt](https://media.geeksforgeeks.org/wp-content/uploads/20230406112559/TCP-3.png)

#### Method-Method Penting dalam Class Pemrograman Jaringan

- `ServerSocket.accept()`: Method yang memblokir eksekusi server sampai koneksi klien masuk diterima, dan mengembalikan objek Socket baru untuk komunikasi dengan klien tersebut.

- `Socket.connect(SocketAddress):` Method yang digunakan klien untuk mencoba membangun koneksi TCP ke server.

- `Socket.getInputStream()`: Mengembalikan InputStream yang terhubung ke socket, digunakan untuk membaca data yang dikirim oleh sisi lain.

- `Socket.getOutputStream()`: Mengembalikan OutputStream yang terhubung ke socket, digunakan untuk menulis data yang akan dikirim ke sisi lain.

- `DatagramSocket.send(DatagramPacket):` Mengirim paket UDP.

- `DatagramSocket.receive(DatagramPacket)`: Menerima paket UDP.

<br>

### Analisis dan Perancangan Berorientasi Objek (OOA/D)

Analisis dan Perancangan Berorientasi Objek (OOA/D) adalah pendekatan rekayasa perangkat lunak untuk memodelkan sistem sebagai kumpulan objek yang berinteraksi.

#### Tujuan Analisis dan Perancangan

- **Tujuan Analisis:** Memahami apa yang harus dilakukan sistem. Ini melibatkan pengumpulan dan pemodelan persyaratan dari perspektif domain masalah, tanpa memikirkan detail implementasi.

- **Tujuan Perancangan:** Merencanakan bagaimana sistem akan dibangun. Ini melibatkan pembuatan arsitektur sistem, identifikasi komponen perangkat lunak, dan detail desain kelas yang akan diimplementasikan.

- **Manfaat Umum:** Mengurangi risiko, meningkatkan kualitas, memfasilitasi komunikasi antar tim, mempermudah pemeliharaan dan pengembangan di masa depan, serta memastikan solusi yang dibangun sesuai dengan kebutuhan bisnis.

#### Tahapan dalam Analisis dan Perancangan Berorientasi Objek

Proses OOA/D biasanya mengikuti tahapan iteratif:

1. **Model Kebutuhan (Requirements Model)**

- Mengidentifikasi dan mendokumentasikan persyaratan fungsional dan non-fungsional dari sistem. Diagram UML yang Tepat:
  - **Use Case Diagram**:
    - Memodelkan fungsionalitas sistem dari sudut pandang aktor (pengguna atau sistem eksternal).
    - Menjawab pertanyaan "apa" yang sistem lakukan untuk aktor.
    - Fokus pada interaksi antara aktor dan sistem, bukan bagaimana sistem bekerja di dalam.

![alt](https://www.researchgate.net/publication/361093005/figure/fig5/AS:11431281100350982@1669348210317/Use-case-diagram-of-F-R-10.png)

2. **Model Analisis (Analysis Model)**

- Memahami domain masalah secara lebih dalam dan membuat model konseptual dari entitas-entitas utama dan hubungannya, terlepas dari detail implementasi. Diagram UML yang Tepat:

  - **Class Diagram (versi analisis)**: - Memodelkan entitas-entitas kunci dalam domain masalah, atribut, dan hubungan statisnya. - Abstraksi dari konsep dunia nyata, tanpa mempertimbangkan detail teknis implementasi.
    ![alt](https://cdn-images.visual-paradigm.com/guide/uml/what-is-class-diagram/12-uml-class-diagram-example.png)

  - **Sequence Diagram**:

    - Menggambarkan interaksi antar objek dalam urutan waktu.
    - Menunjukkan bagaimana objek saling mengirim pesan (memanggil method) dalam skenario tertentu.

    ![alt](https://awsimages.detik.net.id/community/media/visual/2023/01/28/sequence-diagram-1_169.jpeg?w=1200)

  - **Activity Diagram**: - Memodelkan aliran kontrol dan proses bisnis. - Menunjukkan bagaimana aktivitas dimulai, berurutan, bercabang, atau berakhir.
    ![alt](https://dicoding-assets.sgp1.cdn.digitaloceanspaces.com/blog/wp-content/uploads/2020/04/intern-rendi-activity.png)

3. **Model Perancangan (Design Model)**

- Menerjemahkan model analisis ke dalam model yang lebih detail dan berorientasi implementasi, merencanakan arsitektur perangkat lunak. Diagram UML yang Tepat:

  - **Class Diagram (versi desain)**:
    - Memodelkan kelas-kelas perangkat lunak yang akan diimplementasikan.
    - Termasuk atribut, method, visibilitas (public, private, protected), dan hubungan seperti inheritance, association, aggregation, dan composition.
  - **Component Diagram**:

    - Menunjukkan komponen-komponen perangkat lunak dan hubungan ketergantungannya.
    - Cocok untuk melihat gambaran arsitektur modul secara menyeluruh.
      ![alt](https://agilemodeling.com/wp-content/uploads/2023/05/componentDiagram.gif)

  - **Package Diagram**:
    - Mengorganisir kelas atau komponen ke dalam grup logis (package).
    - Berguna untuk manajemen modularitas dan skala besar.
      ![alt](https://www.uml-diagrams.org/package-diagrams/package-diagram-elements.png)

4. **Model Implementasi (Implementation Model)**

- Mengkonversi model perancangan menjadi kode program yang sebenarnya. Diagram UML yang Tepat:

  - **Deployment Diagram**:

    - Menunjukkan bagaimana komponen perangkat lunak dan artefak lainnya ditempatkan pada node (perangkat keras) fisik.
    - Bermanfaat untuk perencanaan infrastruktur dan topologi sistem.
      ![alt](https://d2slcw3kip6qmk.cloudfront.net/marketing/pages/chart/uml/deployment-diagram/deployment-diagram-example-700x412@2x.jpeg)

  - **Component Diagram (versi fisik)**:
    - Menggambarkan komponen nyata (library, file, modul) dan bagaimana mereka dideploy.
  - **Package Diagram**:
    - Menunjukkan struktur direktori atau organisasi file dalam proyek kode.

5. **Model Tes (Test Model)**

- Merencanakan dan melaksanakan pengujian untuk memastikan sistem memenuhi persyaratan. Diagram UML yang Tepat:
  - **Activity Diagram**:
    - Menggambarkan aliran aktivitas dalam skenario pengujian.
    - Menunjukkan langkah-langkah pengujian dan percabangan alur jika terjadi error.
  - **State Machine Diagram**:
    - Menggambarkan siklus hidup objek dengan status dan transisinya.
    - Cocok untuk objek dengan banyak kondisi atau validasi berdasarkan status tertentu.
      [alt](https://www.softwareideas.net/i/DirectImage/3089/uml-state-machine-diagram-for-login-)

#### Responsibility-Driven Design (RDD)

_Responsibility-Driven Design (RDD)_ adalah sebuah pendekatan dalam perancangan berorientasi objek di mana fokus utama adalah mengidentifikasi dan mendistribusikan tanggung jawab (_responsibilities_) antar objek atau kelas.

Dalam pemrograman berorientasi objek, kita seringkali terpaku pada data dan struktur. RDD menggeser fokus kita ke pertanyaan yang lebih fundamental: "Apa yang harus dilakukan oleh objek ini?" atau "Tanggung jawab apa yang diemban objek ini?". Pendekatan ini memiliki beberapa keuntungan signifikan:

- **_Separation of Concerns_**: RDD mendorong pembagian tanggung jawab yang jelas antar objek, menghasilkan kelas-kelas yang lebih kohesif dan kopling yang lebih longgar. Ini selaras dengan prinsip Single Responsibility Principle (SRP) dari SOLID.

- **Desain yang Lebih Baik**: Dengan memikirkan tanggung jawab terlebih dahulu, kita cenderung mendesain kelas-kelas yang lebih bermakna dan fungsional, bukan hanya wadah data pasif.

- **Kemudahan Pengujian**: Objek dengan tanggung jawab yang jelas lebih mudah diuji secara independen.

- **Fleksibilitas dan Skalabilitas**: Sistem yang dirancang dengan RDD lebih mudah diadaptasi terhadap perubahan kebutuhan dan diperluas di masa depan.

- **Kolaborasi yang Jelas**: RDD membantu dalam mengidentifikasi bagaimana objek-objek berinteraksi satu sama lain untuk mencapai tujuan keseluruhan.

##### Konsep Utama RDD

Responsibility-Driven Design (RDD) adalah pendekatan perancangan perangkat lunak yang berfokus pada **tanggung jawab** dan **kolaborasi** antar objek.

**Tanggung Jawab (Responsibilities)**

Yaitu tugas atau tindakan yang dapat dilakukan oleh sebuah objek, atau informasi yang dapat diketahuinya. Jenis Tanggung Jawab:

a. Melakukan sesuatu (_Doing Responsibilities_)

- Menjalankan aksi, perhitungan, atau inisiasi proses terhadap objek lain.
- **Contoh**:
  - `hitungTotalHarga()`
  - `kirimEmail()`

b. Mengetahui sesuatu (_Knowing Responsibilities_)

- Menyimpan atau mengakses informasi, status, atau relasi dengan objek lain.
- **Contoh**:
  - `getNamaProduk()`
  - `isTersedia()`

**Kolaborasi (Collaborations)**

Kolaborasi adalah interaksi antar objek untuk menyelesaikan suatu tugas yang lebih besar. Objek seringkali perlu berkolaborasi untuk memenuhi tanggung jawabnya sendiri. Objek berkolaborasi dengan mengirimkan pesan (memanggil metode) kepada objek lain.

<br>

##### Studi kasus: Kisah Toko Buku

**Skenario 1: Tanpa RDD (Desain Tradisional)**

Di toko buku **"Buku Pintar"**, awalnya mereka mendesain sistem dengan pendekatan tradisional. Struktur kelasnya:

- **DatabaseManager**: Menyimpan semua data buku.
- **UIManipulator**: Mengatur tampilan antarmuka pengguna.
- **LogikaBisnis**: Menampung semua aturan bisnis penjualan.

Ketika seorang pelanggan membeli buku:

1. `UIManipulator` menerima input dari pengguna.
2. Ia memanggil `LogikaBisnis` untuk memproses pembelian.
3. `LogikaBisnis` langsung berinteraksi dengan `DatabaseManager` untuk mengurangi stok dan mencatat transaksi.

Masalah yang Muncul:

- Perubahan kecil pada pengelolaan stok (misal: menambahkan diskon otomatis) mengharuskan perubahan di `LogikaBisnis`.
- Ingin mengganti sistem penyimpanan dari SQL ke NoSQL? Lagi-lagi, `LogikaBisnis` perlu diubah.
- Sulit debugging karena **satu kelas terlalu banyak tanggung jawab**.
- `LogikaBisnis` menjadi **God Object** dan melanggar prinsip **Single Responsibility Principle (SRP)**.

<br>

**Skenario 2: Dengan RDD (Desain Berbasis Tanggung Jawab)**

Setelah berbagai masalah, mereka mendesain ulang sistem dengan pendekatan **Responsibility-Driven Design (RDD)**. Pertanyaannya kini menjadi:

> "Objek apa yang bertanggung jawab atas apa?"

Pembagian Tanggung Jawab:

- **Buku (kelas)**:

  - Mengetahui judul, penulis, harga, dan stok sendiri.
  - Bertanggung jawab mengurangi stok saat dibeli.

- **KeranjangBelanja (kelas)**:

  - Menambah/menghapus buku.
  - Menghitung total harga.
  - Berkolaborasi dengan `Buku` untuk mendapatkan harga.

- **Pesanan (kelas)**:

  - Memastikan pembelian terjadi.
  - Mengurangi stok buku terkait.
  - Mencatat detail pesanan.
  - Berkolaborasi dengan `Buku` dan `Pembayaran`.

- **ManajerStok (kelas baru)**:
  - Mengelola dan memperbarui stok secara keseluruhan.

Sehingga, alur Ketika Pelanggan Membeli Buku:

1. `UIManipulator` menerima input.
2. Memerintahkan `KeranjangBelanja` untuk menambahkan buku.
3. Setelah keranjang siap, `Pesanan` dibuat dari isinya.
4. `Pesanan` meminta setiap `Buku` mengurangi stoknya sendiri.
5. `Pesanan` berkolaborasi dengan `Pembayaran` untuk menyelesaikan transaksi.

**Manfaat yang Terlihat:**

- Perubahan pada cara `Buku` mengelola stok **hanya** memengaruhi kelas `Buku`.
- Sistem pembayaran bisa diganti tanpa mengubah `Pesanan`, selama antarmuka `Pembayaran` tetap sama.
- Setiap kelas punya **tanggung jawab tunggal (SRP)**.
- Kolaborasi antar objek menjadi **jelas dan terisolasi**.

**Kesimpulan**

Tanpa RDD:

- Sistem cenderung menjadi kumpulan kelas yang **saling bergantung erat**.
- Sulit diubah dan tidak fleksibel.

Dengan RDD:

- **Tanggung jawab tersebar merata** antar objek.
- Sistem lebih **modular, fleksibel, dan mudah dirawat**.
- Mematuhi prinsip desain seperti **SRP dan loose coupling**.

---

<br>

## Pilihan Ganda

#### Materi 1: Pemrosesan Input/Output & Pemrograman Jaringan

1. Mana di antara pernyataan berikut yang TIDAK TEPAT mengenai perbedaan dasar antara `InputStream` dan `OutputStream` dalam Java I/O?  
   A. `InputStream` digunakan untuk membaca data, sedangkan `OutputStream` digunakan untuk menulis data.  
   B. `InputStream` dan `OutputStream` keduanya merupakan class abstrak.  
   C. `InputStream` berfokus pada sumber data, sedangkan `OutputStream` berfokus pada tujuan data.  
   D. `InputStream` menangani aliran data masuk, sedangkan `OutputStream` menangani aliran data keluar.  
   E. `InputStream` dan `OutputStream` dapat digunakan secara bersamaan pada stream yang sama untuk operasi read dan write.

**Jawaban: E**. `InputStream` dan `OutputStream` secara inheren menangani aliran data satu arah (masuk atau keluar). Untuk operasi baca dan tulis pada stream yang sama, biasanya diperlukan kombinasi atau stream khusus seperti RandomAccessFile.

2. Untuk membaca data teks baris per baris dari sebuah file di Java, class I/O mana yang paling sering digunakan dan efisien?  
   A. `FileInputStream`  
   B. `DataInputStream`  
   C. `BufferedReader`  
   D. `FileReader`  
   E. `Scanner`

**Jawaban: C**. `BufferedReader` efisien untuk membaca teks baris per baris karena melakukan buffering, mengurangi jumlah akses fisik ke file. `FileReader` adalah dasar untuk membaca karakter, tetapi `BufferedReader` membungkusnya untuk efisiensi.

3. Anda ingin menulis objek Java secara langsung ke dalam sebuah file sehingga dapat dibaca kembali sebagai objek di lain waktu. Class I/O kunci mana yang PALING TEPAT untuk keperluan ini?  
   A. `FileWriter`  
   B. `PrintWriter`  
   C. `ObjectOutputStream`  
   D. `FileOutputStream`
   E. `BufferedWriter`

**Jawaban: C**. `ObjectOutputStream` digunakan untuk serialisasi objek, yaitu mengubah objek menjadi aliran byte yang dapat disimpan dan kemudian di-deserialisasi kembali menjadi objek.

4. Manakah dari pernyataan berikut yang TIDAK BENAR mengenai konsep dasar Pemrograman Jaringan (_Networking_) di Java?  
   A. `Socket` adalah salah satu mekanisme utama untuk komunikasi antar program melalui jaringan.  
   B. `ServerSocket` digunakan oleh aplikasi klien untuk membuat koneksi ke server.  
   C. Port adalah angka 16-bit yang digunakan untuk mengidentifikasi aplikasi atau layanan tertentu pada sebuah host.  
   D. Protokol seperti TCP dan UDP menentukan aturan untuk pertukaran data melalui jaringan.  
   E. `InetAddress` digunakan untuk merepresentasikan alamat IP.

**Jawaban: B**. `ServerSocket` digunakan oleh sisi server untuk mendengarkan dan menerima koneksi masuk dari klien, bukan oleh klien untuk membuat koneksi. Sisi klien menggunakan Socket.

5. Ketika membuat aplikasi client-server menggunakan TCP di Java, class mana yang digunakan oleh sisi klien untuk inisiasi koneksi ke server?  
   A. `ServerSocket`  
   B. `DatagramSocket`
   C. `Socket`  
   D. URL  
   E. URLConnection

**Jawaban: C**. `Socket` adalah class yang digunakan oleh sisi klien untuk membangun koneksi TCP ke server.

6. Apa tujuan utama dari method `flush()` pada `OutputStream` atau `Writer` dalam Java I/O?  
   A. Menutup stream dan membebaskan sumber daya.  
   B. Memastikan semua data yang di-buffer telah ditulis ke tujuan.  
   C. Membaca semua data yang tersisa dari sumber.  
   D. Mengatur ulang posisi pointer dalam stream.  
   E. Mengenkripsi data sebelum ditulis.

**Jawaban: B**. flush() memaksa data yang masih ada di buffer untuk segera ditulis ke tujuan, yang penting terutama untuk stream yang di-buffer atau untuk memastikan data terkirim di jaringan.

7. Mana di antara class berikut yang merupakan class kunci untuk mengirim dan menerima paket data connectionless (tanpa koneksi) dalam pemrograman jaringan Java?  
   A. `Socket`  
   B. `ServerSocket`  
   C. `DatagramSocket`  
   D. `MulticastSocket`  
   E. `HttpURLConnection`

**Jawaban: C**. `DatagramSocket` digunakan untuk komunikasi UDP (User Datagram Protocol), yang merupakan protokol _connectionless_.

---

#### Materi 2: Concept of _Multithreading_ & Advanced Concurrency

8. Apa perbedaan PALING TEPAT antara _Multiprocessing_ dan _Multithreading_?  
   A. _Multiprocessing_ beroperasi pada satu CPU, _Multithreading_ pada banyak CPU.
   B. _Multiprocessing_ melibatkan banyak program terpisah, _Multithreading_ melibatkan banyak bagian dalam satu program.
   C. _Multiprocessing_ berbagi memori antar proses, _Multithreading_ memiliki memori terpisah per thread.
   D. _Multiprocessing_ lebih ringan dan cepat, _Multithreading_ lebih berat dan lambat.
   E. _Multiprocessing_ hanya berlaku untuk sistem operasi, _Multithreading_ hanya untuk aplikasi

**Jawaban: B**. _Multiprocessing_ melibatkan eksekusi bersamaan dari beberapa program atau proses terpisah, masing-masing dengan ruang memori sendiri. _Multithreading_ melibatkan eksekusi bersamaan dari beberapa bagian (thread) dalam satu program yang sama, berbagi ruang memori yang sama.

---

9. Mengapa `Runnable` _interface_ seringkali lebih disarankan daripada Thread class saat membuat thread baru di Java?  
   A. Karena `Runnable` memiliki method `start()` yang lebih powerful.  
   B. Karena Java tidak mendukung _multiple inheritance_ dari class, sehingga `Runnable` memungkinkan class untuk mewarisi class lain.  
   C. Karena `Runnable` secara otomatis menyediakan sinkronisasi thread.  
   D. Karena `Runnable` lebih efisien dalam penggunaan memori.  
   E. Karena `Runnable` dapat dieksekusi oleh `ExecutorService` secara langsung.

**Jawaban: B**. Java tidak mendukung pewarisan ganda (_multiple inheritance_) dari class. Dengan mengimplementasikan `Runnable`, sebuah class masih bisa mewarisi class lain sambil tetap menyediakan fungsionalitas untuk dijalankan sebagai thread.

---

10. Perhatikan kode program berikut:

```java
public class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running.");
    }
    public static void main(String[] args) {
        MyThread t = new MyThread();
        // Baris mana yang harus ditambahkan agar thread berjalan?
    }
}
```

Baris kode mana yang PALING TEPAT untuk ditambahkan agar thread dapat berjalan?
A. `t.run()`;
B. `t.start()`;
C. `Thread.sleep(100)`;
D. `new Thread(t).start()`;
E. `t.execute()`;

**Jawaban: B**. Memanggil `start()` pada objek Thread akan membuat thread baru dan memanggil method `run()` di thread tersebut. Memanggil` t.run()` akan mengeksekusi method `run()` di thread utama (_main thread_) saja.

11. Apa yang dimaksud dengan "Race Condition Problem" dalam konteks _multithreading_?
    A. Situasi di mana dua atau lebih thread mencoba mengakses resource yang berbeda secara bersamaan.
    B. Situasi di mana hasil eksekusi program bergantung pada urutan non-deterministik di mana thread mengeksekusi kode.
    C. Kondisi di mana satu thread menunggu thread lain selamanya.
    D. Keadaan di mana thread berhenti merespons karena kekurangan sumber daya CPU.
    E. Masalah ketika thread tidak dapat mengakses memori yang dialokasikan.

**Jawaban: B**. Race condition terjadi ketika beberapa thread mengakses dan memanipulasi data bersamaan, dan hasil akhirnya bergantung pada urutan (non-deterministik) eksekusi thread.

12. Kata kunci Java mana yang digunakan untuk memastikan bahwa hanya satu thread yang dapat mengeksekusi bagian kode tertentu (critical section) pada satu waktu, sehingga mencegah race condition?
    A. volatile
    B. final
    C. static
    D. synchronized
    E. transient

**Jawaban: D**. synchronized adalah kata kunci yang digunakan untuk mengontrol akses ke blok kode atau method, memastikan bahwa hanya satu thread yang dapat mengeksekusinya pada satu waktu, sehingga mencegah race condition.

13. Mana di antara pernyataan berikut yang TIDAK BENAR mengenai status-status thread di Java?
    A. `NEW`: Sebuah thread yang baru dibuat dan belum dimulai.
    B. `RUNNABLE`: Sebuah thread yang sedang dieksekusi atau siap dieksekusi oleh JVM.
    C. `BLOCKED`: Sebuah thread yang menunggu untuk mendapatkan monitor lock untuk memasuki blok synchronized.
    D. `WAITING`: Sebuah thread yang sedang menunggu thread lain untuk melakukan aksi tertentu tanpa batas waktu.
    E. `TERMINATED`: Sebuah thread yang sedang dalam proses shutdown dan tidak dapat dimulai kembali.

**Jawaban: E**. TERMINATED berarti thread telah menyelesaikan eksekusinya atau telah dihentikan, dan tidak dapat dimulai kembali.

14. Dalam konteks komunikasi antar thread, method `wait()` dan `notify()` harus dipanggil dari blok atau method yang mana?
    A. Hanya dari method static.
    B. Hanya dari method non-static.
    C. Dari blok atau method yang disinkronisasi pada objek yang sama.
    D. Dari blok try-catch manapun.
    E. Dari class Thread itu sendiri.

**Jawaban: C**. Method `wait()`, `notify()`, dan `notifyAll()` harus dipanggil dari dalam blok atau method yang telah disinkronisasi pada objek yang sedang di-wait atau di-notify. Ini untuk memastikan bahwa thread memiliki monitor lock pada objek tersebut.

15. Apa tujuan utama dari _interface_ java.util.concurrent.Executor dan `ExecutorService`?
    A. Untuk membuat thread secara manual.
    B. Untuk menyediakan mekanisme bagi thread untuk berkomunikasi.
    C. Untuk mengelola dan menjalankan tugas (tasks) secara asinkron dengan thread pool.
    D. Untuk mencegah deadlock dalam aplikasi multithread.
    E. Untuk mengukur performa eksekusi thread.

**Jawaban: C**. Executor dan `ExecutorService` menyediakan kerangka kerja untuk menjalankan tugas secara asinkron, mengelola siklus hidup thread, dan seringkali menggunakan thread pool untuk efisiensi.

---

#### Materi 3: Analisis & Perancangan Berorientasi Objek

16. Dalam model kebutuhan (requirement model) pada analisis berorientasi objek, diagram UML mana yang PALING TEPAT digunakan untuk menggambarkan fungsionalitas sistem dari sudut pandang pengguna?
    A. Class Diagram
    B. Sequence Diagram
    C. Use Case Diagram
    D. Component Diagram
    E. Activity Diagram

**Jawaban: C**. Use Case Diagram adalah alat utama untuk memodelkan persyaratan fungsional sistem dari perspektif pengguna (aktor).

17. Apa tujuan utama dari Class Diagram dalam fase analisis berorientasi objek?
    A. Menggambarkan interaksi objek dalam urutan waktu.
    B. Memvisualisasikan struktur statis sistem, termasuk kelas, atribut, metode, dan hubungan antar kelas.
    C. Menampilkan aliran kontrol dan keputusan dalam suatu proses.
    D. Menggambarkan deployment komponen perangkat lunak.
    E. Mendefinisikan status dan transisi objek.

**Jawaban: B**. Class Diagram dalam fase analisis fokus pada identifikasi entitas-entitas utama dan hubungan statisnya dalam domain masalah.

18. Mana di antara pernyataan berikut yang TIDAK TEPAT mengenai perbedaan antara Class Diagram pada fase analisis dan Class Diagram pada fase perancangan?
    A. Class Diagram analisis berfokus pada "what" (apa yang harus dilakukan sistem), sedangkan class diagram perancangan berfokus pada "how" (bagaimana sistem melakukannya).
    B. Class Diagram analisis lebih abstrak, sedangkan class diagram perancangan lebih detail dengan informasi implementasi.
    C. Class Diagram analisis mencakup entitas dunia nyata, sedangkan class diagram perancangan mencakup kelas-kelas perangkat lunak.
    D. Class Diagram analisis biasanya tidak mencantumkan tipe data atau visibility, sedangkan class diagram perancangan mencantumkannya.
    E. Class Diagram analisis dan perancangan memiliki tujuan yang sama persis dan tidak ada perbedaan signifikan.

**Jawaban: E**. Class Diagram analisis dan perancangan memiliki tujuan dan tingkat detail yang berbeda signifikan. Analisis fokus pada domain masalah ("what"), sedangkan perancangan fokus pada solusi teknis ("how").

19. Untuk menggambarkan interaksi objek dalam urutan kronologis atau waktu tertentu, diagram UML mana yang PALING TEPAT digunakan?
    A. Activity Diagram
    B. State Machine Diagram
    C. Component Diagram
    D. Sequence Diagram
    E. Package Diagram

**Jawaban: D**. Sequence Diagram adalah diagram interaksi yang menunjukkan bagaimana objek berinteraksi satu sama lain dalam urutan waktu.

20. Apa peran Component Diagram dalam fase perancangan berorientasi objek (versi desain) dibandingkan dengan Component Diagram (versi fisik) pada fase implementasi?
    A. Versi desain menampilkan struktur logis komponen, sedangkan versi fisik menampilkan bagaimana komponen-komponen tersebut didistribusikan pada perangkat keras.
    B. Versi desain fokus pada detail implementasi, sedangkan versi fisik fokus pada abstraksi.
    C. Versi desain menggambarkan interaksi pengguna, sedangkan versi fisik menggambarkan interaksi antar database.
    D. Keduanya memiliki fungsi yang sama persis tanpa perbedaan.
    E. Component Diagram versi desain hanya digunakan untuk sistem kecil, sedangkan versi fisik untuk sistem besar.

**Jawaban: A**. Component Diagram versi desain menunjukkan bagaimana komponen-komponen logis saling terhubung untuk membentuk sistem, sedangkan versi fisik (deployment diagram, yang seringkali digabungkan atau merupakan evolusi dari component diagram dalam konteks implementasi) menunjukkan bagaimana komponen-komponen tersebut didistribusikan dan dijalankan pada node perangkat keras.

---

## Esai

#### Soal 21: Analisis Kode Multithreading dan Sinkronisasi

**Materi 1 & 2: Pemrosesan Input/Output, Pemrograman Jaringan, dan Multithreading**

Perhatikan potongan kode Java berikut yang mencoba mengelola saldo bank secara bersamaan oleh beberapa thread.

```java
class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    public void deposit(double amount) {
        // Line A
        double temp = balance;
        try {
            Thread.sleep(10); // Simulasi penundaan
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        temp += amount;
        balance = temp;
        // Line B
    }

    public void withdraw(double amount) {
        // Line C
        double temp = balance;
        try {
            Thread.sleep(10); // Simulasi penundaan
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        if (temp >= amount) {
            temp -= amount;
            balance = temp;
        } else {
            System.out.println("Insufficient funds for withdrawal.");
        }
        // Line D
    }

    public double getBalance() {
        return balance;
    }
}

public class TransactionSimulator {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);

        Runnable depositTask = () -> {
            for (int i = 0; i < 50; i++) {
                account.deposit(10);
            }
        };

        Runnable withdrawTask = () -> {
            for (int i = 0; i < 50; i++) {
                account.withdraw(5);
            }
        };

        Thread t1 = new Thread(depositTask, "DepositThread");
        Thread t2 = new Thread(withdrawTask, "WithdrawThread");

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println("Final Balance: " + account.getBalance());
    }
}
```

a. Jelaskan mengapa kode di atas (tanpa modifikasi) berpotensi mengalami Race Condition! Identifikasi bagian kode (Line A hingga Line D) yang merupakan critical section dan berikan alasannya.
b. Sebutkan dan jelaskan salah satu teknik sinkronisasi yang paling sesuai untuk mengatasi race condition pada `BankAccount` class dalam kasus ini.
c. Berikan contoh modifikasi kode pada `BankAccount` class (cukup bagian deposit dan withdraw method) menggunakan teknik sinkronisasi yang Anda sebutkan di poin (b) agar program berjalan dengan benar dan aman dari race condition.

**Jawaban:**

**a. Penjelasan Race Condition dan Critical Section:**
Kode di atas berpotensi mengalami Race Condition karena multiple thread (`DepositThread` dan `WithdrawThread`) mengakses dan memodifikasi sumber daya bersama (`balance` pada objek `BankAccount`) secara bersamaan tanpa mekanisme sinkronisasi yang memadai.

**Critical Section** adalah bagian kode di mana sumber daya bersama diakses atau dimodifikasi, dan harus dieksekusi oleh hanya satu thread pada satu waktu untuk menghindari inkonsistensi data. Dalam kode ini, **critical section** terjadi pada:

- `deposit` method (antara Line A dan Line B):

```java
// Line A
double temp = balance; // Baca nilai balance
try { Thread.sleep(10); } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
temp += amount; // Modifikasi nilai lokal
balance = temp; // Tulis kembali nilai ke balance
// Line B
```

- `withdraw` method (antara Line C dan Line D):

```java
// Line C
double temp = balance; // Baca nilai balance
try { Thread.sleep(10); } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
if (temp >= amount) {
    temp -= amount; // Modifikasi nilai lokal
    balance = temp; // Tulis kembali nilai ke balance
}
// Line D
```

**Alasan:** Potongan kode ini melibatkan urutan operasi baca-modifikasi-tulis (_read-modify-write_) pada variabel `balance`. Jika dua thread (misalnya, satu `deposit` dan satu `withdraw`) membaca nilai balance yang sama, lalu masing-masing memodifikasinya secara lokal, dan kemudian menuliskan kembali hasilnya, maka salah satu modifikasi bisa "menimpa" modifikasi yang lain, menyebabkan hasil akhir balance menjadi tidak akurat atau tidak sesuai harapan. Penambahan `Thread.sleep(10)` secara eksplisit mensimulasikan penundaan dan meningkatkan kemungkinan terjadinya race condition ini.

**b. Teknik Sinkronisasi yang Sesuai:**
Teknik sinkronisasi yang paling sesuai untuk mengatasi race condition ini adalah menggunakan Keyword `synchronized`. Karena Keyword `synchronized` di Java digunakan untuk mengamankan akses ke sumber daya bersama (_shared resources_) dari multiple thread. Ketika sebuah method atau blok kode ditandai sebagai `synchronized`, maka hanya satu thread yang dapat masuk dan mengeksekusi method/blok tersebut pada objek yang sama pada satu waktu. Thread lain yang mencoba masuk akan diblokir (status `BLOCKED`) hingga thread yang sedang berjalan selesai dan melepaskan monitor lock. Ini memastikan atomisitas operasi pada critical section, artinya operasi tersebut dianggap sebagai satu unit yang tidak dapat diganggu oleh thread lain.

**c. Modifikasi Kode pada BankAccount class:**

```java
class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    // Menggunakan synchronized method
    public synchronized void deposit(double amount) {
        // Line A
        double temp = balance;
        try {
            Thread.sleep(10); // Simulasi penundaan
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        temp += amount;
        balance = temp;
        // Line B
    }

    // Menggunakan synchronized method
    public synchronized void withdraw(double amount) {
        // Line C
        double temp = balance;
        try {
            Thread.sleep(10); // Simulasi penundaan
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        if (temp >= amount) {
            temp -= amount;
            balance = temp;
        } else {
            System.out.println("Insufficient funds for withdrawal. (Attempt by " + Thread.currentThread().getName() + ")");
        }
        // Line D
    }

    public double getBalance() {
        return balance;
    }
}
```

**Penjelasan Modifikasi:**
Dengan menambahkan keyword `synchronized` pada method `deposit` dan `withdraw`, kita memastikan bahwa hanya satu thread yang dapat mengeksekusi salah satu dari method ini pada objek `account` yang sama pada satu waktu. Jika `DepositThread` sedang mengeksekusi `deposit()`, maka `WithdrawThread` yang mencoba memanggil `withdraw()` pada objek `account` yang sama akan menunggu hingga `deposit()` selesai dan melepaskan lock-nya. Ini mencegah terjadinya race condition dan menjamin konsistensi data `balance`.

<br>

#### Soal 22: Perancangan Sistem Manajemen Perpustakaan dengan UML

**Materi 3: Analisis & Perancangan Berorientasi Objek**

Anda diminta untuk merancang sebagian kecil dari sistem manajemen perpustakaan menggunakan UML. Sistem ini akan memungkinkan pengguna (pustakawan dan anggota) untuk mengelola buku dan peminjaman. Dengan skenario Sistem:

- Pustakawan dapat menambah buku baru, menghapus buku, dan mencatat peminjaman/pengembalian buku.
- Anggota Perpustakaan dapat mencari buku dan meminjam buku (jika tersedia).
- Setiap Buku memiliki judul, pengarang, ISBN, dan status ketersediaan.
- Setiap Anggota memiliki ID anggota dan nama.
- Peminjaman mencatat tanggal pinjam, tanggal jatuh tempo, dan buku serta anggota yang terlibat.

Tugas Anda:

a. Buatlah Use Case Diagram untuk menggambarkan fungsionalitas utama sistem ini dari sudut pandang Pustakawan dan Anggota Perpustakaan.
b. Buatlah Class Diagram (versi analisis) yang merepresentasikan entitas-entitas utama dalam skenario ini (Buku, Anggota, Peminjaman, Pustakawan). Pastikan untuk menyertakan atribut dan hubungan antar kelas (asosiasi, multiplicity).
c. Jelaskan secara singkat peran dari masing-masing diagram (Use Case Diagram dan Class Diagram) yang Anda buat dalam proses analisis berorientasi objek.

**Jawaban:**

a. Use Case Diagram:

Aktor:

- Pustakawan: Pengguna yang bertanggung jawab mengelola koleksi buku dan transaksi peminjaman.
- Anggota Perpustakaan: Pengguna yang ingin mencari dan meminjam buku.

Use Cases:

- Kelola Buku: Merupakan use case umum yang dapat di-generalize menjadi Tambah Buku dan Hapus Buku.
- Tambah Buku: Pustakawan menambahkan informasi buku baru ke sistem.
- Hapus Buku: Pustakawan menghapus data buku dari sistem.
- Catat Peminjaman: Pustakawan merekam detail peminjaman buku oleh anggota.
- Catat Pengembalian: Pustakawan merekam detail pengembalian buku oleh anggota.
- Cari Buku: Anggota perpustakaan mencari buku berdasarkan kriteria tertentu (misalnya judul, pengarang).
- Pinjam Buku: Anggota perpustakaan meminjam buku yang tersedia.

b. Class Diagram:

Kelas:

- `Buku`: Merepresentasikan entitas buku dalam perpustakaan.

  - judul: String
  - pengarang: String
  - ISBN: String
  - statusKetersediaan: Boolean

- `Anggota`: Merepresentasikan anggota perpustakaan.

  - idAnggota: String
  - nama: String

- `Pustakawan`: Merepresentasikan staf perpustakaan.

  - idPustakawan: String
  - nama: String

- `Peminjaman`: Merepresentasikan transaksi peminjaman buku.
  - tanggalPinjam: Date
  - tanggalJatuhTempo: Date

Hubungan (Asosiasi):

- Pustakawan - Buku: Pustakawan dapat mengelola banyak buku. (Asosiasi: 1.._ Pustakawan - 0.._ Buku)
- Anggota - Peminjaman: Satu Anggota dapat melakukan banyak Peminjaman. (Asosiasi: 1 Anggota - 0..\* Peminjaman)
- Buku - Peminjaman: Satu Buku dapat terlibat dalam banyak Peminjaman. (Asosiasi: 1 Buku - 0..\* Peminjaman)

c. Peran Diagram dalam Analisis Berorientasi Objek:

- Use Case Diagram: Diagram ini berperan dalam model kebutuhan. Fungsinya adalah untuk mengidentifikasi dan menggambarkan fungsionalitas sistem dari sudut pandang pengguna (aktor). Ini membantu dalam memahami "apa" yang harus dilakukan sistem dan siapa yang berinteraksi dengannya, tanpa membahas detail implementasi internal. Diagram ini penting untuk komunikasi antara pengembang dan pemangku kepentingan.
- Class Diagram: Diagram ini berperan dalam model analisis. Fungsinya adalah untuk memvisualisasikan struktur statis sistem dalam domain masalah. Ini mengidentifikasi entitas-entitas utama (konsep atau objek dunia nyata) yang relevan, atribut-atributnya, dan hubungan-hubungan (asosiasi, multiplicity) antar entitas tersebut. Pada fase analisis, class diagram masih bersifat konseptual dan tidak terlalu detail dengan aspek implementasi (seperti tipe data spesifik atau visibility method). Tujuannya adalah untuk memahami "informasi apa" yang perlu dikelola oleh sistem dan bagaimana entitas-entitas tersebut saling terkait.
