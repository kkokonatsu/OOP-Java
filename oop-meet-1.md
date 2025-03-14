# OOP: Chapter 1

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQmSmJgXu6kW8ONXMTLq0LD6BJGFV3Hoc0DRg&s)

**Table of Contents**

<!-- [TOC] -->

<br>

## Java Architecture

![](https://miro.medium.com/v2/resize:fit:1400/1*lLBwoFv6RAZ15vRuK6yFQg.png)

**JDK (Java Development Kit)** – Paket yang berisi peralatan (_tools_) untuk mengembangkan program Java seperti _compiler_ (javac).

**JRE (Java Runtime Environment)** – Lingkungan yang dibutuhkan untuk menjalankan aplikasi Java, termasuk JVM.

**JVM (Java Virtual Machine)** – Mesin virtual yang menjalankan _bytecode_ Java di perangkat keras.

## How _Java_ Works?

Gambar berikut menjelaskan bagaimana **_Java_** bekerja, dari proses menulis kode hingga menjalankan program di sistem operasi. Berikut adalah tahapan-tahapan utama dalam proses tersebut:

![](https://media.licdn.com/dms/image/v2/C4E12AQFHYeU8N2ZDCw/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1651746917137?e=2147483647&v=beta&t=6AzWmgQVEkGpvTalSPWYX0BVagXGGbT83FOM9Xj75ds)

### Tahap 1-3: Menulis Kode Sumber Java

Seorang **developer (programmer)** menulis program menggunakan bahasa Java. Program ini dibuat dalam bentuk file teks dengan ekstensi `.java`. Misalnya, dalam gambar ditunjukkan file dengan nama `HelloWorld.java`. Kode sumber ini berisi instruksi-instruksi dalam bahasa pemrograman Java yang masih bisa dibaca manusia, namun belum dapat langsung dijalankan oleh komputer.

### Tahap 4-6: Kompilasi Kode Sumber ke _Bytecode_

Setelah kode sumber selesai ditulis, langkah berikutnya adalah **kompilasi** menggunakan Java Compiler yang disebut **`javac`**. Kompilasi ini dilakukan untuk mengubah kode sumber Java menjadi **bytecode**, yang merupakan bentuk instruksi yang lebih efisien tetapi tidak dapat dibaca langsung oleh manusia. Hasil dari proses ini adalah file `.class`, misalnya `HelloWorld.class`. File ini berisi bytecode, yaitu serangkaian instruksi yang tidak bergantung pada sistem operasi atau perangkat keras tertentu.

### Tahap 7-10: Eksekusi _Bytecode_ dengan JVM

Setelah kode dikompilasi menjadi _bytecode_, langkah berikutnya adalah menjalankannya dengan menggunakan **Java Virtual Machine (JVM)**. JVM berfungsi sebagai interpreter yang menerjemahkan _bytecode_ ke dalam instruksi yang dapat dimengerti oleh sistem operasi. Karena Java menggunakan JVM, program yang telah dikompilasi dapat dijalankan di berbagai sistem operasi tanpa perlu dikompilasi ulang. Inilah yang membuat Java dikenal dengan konsep **"Write Once, Run Anywhere (WORA)"**.

### Tahap 11: Menampilkan Output Program

Setelah JVM mengeksekusi _bytecode_, hasil dari eksekusi ini akan dikirim ke **sistem operasi**, yang kemudian menampilkan outputnya di layar. Misalnya, jika program ditulis untuk mencetak teks **"Hello World"**, maka sistem operasi akan menampilkan teks tersebut kepada pengguna.

<br>

## OOP Basic

Pemrograman Berorientasi Objek (_Object-Oriented Programming_ atau OOP) adalah **paradigma pemrograman** yang mengacu pada konsep **object**. Dalam OOP, program dibangun menggunakan **class** yang merupakan cetakan untuk membuat **object**. Berikut adalah konsep dasar dalam OOP menggunakan analogi sederhana agar lebih mudah dipahami.

Sebagai contoh, kita akan menggunakan **mobil** sebagai analogi untuk memahami konsep OOP dalam Java. Mobil memiliki **cetakannya** (_class_), sedangkan **mobil nyata** yang dijual adalah **instance** (_object_) dari cetakan tersebut.

### Class vs Object

![](https://nonlineardata.com/wp-content/uploads/2020/11/Car_Class.png)

Bayangkan Anda ingin membeli sebuah mobil. Lalu, apa hal pertama yang akan Anda lakukan? Pasti Anda akan memikirkan spesfikasi apa yang Anda inginkan bukan? mulai dari merk, harga, warna, dst. Misalkan, Anda ingin membeli mobil Toyota Avanza berwarna putih seharga Rp250 juta.

Setelah Anda memutuskan spesifikasi tersebut, Anda kemudian pergi ke salah satu showroom di kota Anda. Ketika Anda sampai di tempat, apa yang akan Anda minta ke sales di sana?

Apakah Anda akan meminta **blueprint** dari mobil tersebut dan merakitnya sendiri? **TENTU TIDAK!** Anda pasti membeli mobil yang sudah jadi bukan? 😏.

_Nahh_, dari analogi di atas kita bisa mengetahui konsep OOP yaitu **_Class vs Object._**

- **Class (Cetakan/Blueprint Mobil)**: Cetakan (_blueprint_) yang dimiliki pabrik mobil yang akan menentukan spesifikasi umum, seperti merek, warna, tahun produksi, dan harga. Spesifikasi tersebut disebut dengan **atribut**.
- **Object (Mobil Nyata)**: Mobil nyata yang sudah diproduksi berdasarkan cetakan tersebut, misalnya **Toyota Avanza** dengan warna putih dengan harga Rp250 juta.

<br>

**Contoh dalam Java**

```java
// Class Mobil sebagai blueprint (cetakan mobil)
class Mobil {
    String merk;
    String warna;
    double harga;

    // Constructor untuk menentukan spesifikasi mobil
    Mobil(String merk, String warna, double harga) {
        this.merk = merk;
        this.warna = warna;
        this.harga = harga;
    }

    // Method untuk menampilkan informasi mobil
    void tampilkanInfo() {
        System.out.println("Mobil " + merk + " berwarna " + warna + ", dengan harga Rp " + harga);
    }
}
```

<br>

### Constructor & Methods

Ketika sampai di showroom dan bertanya ke _dealer_, Anda sudah sangat bahagia karena ingin membawa pulang mobil baru untuk ditunjukkan ke teman-teman. Namun, hal tersebut urung karena ternyata **Toyota Avanza berwarna putih** yang Anda inginkan sedang kosong, sehingga Anda perlu menunggu sekitar beberapa hari untuk _indent_. Pada akhirnya, Anda pun tetap setuju untuk menunggu waktu _indent_ dan melakukan pembayaran.

Setelah pembayaran dilakukan, apa yang kemudian akan _dealer_ lakukan?

Tentu mereka akan meminta ke pabrik untuk membuatkan satu unit **Toyota Avanza berwarna putih**.

_Nahh_ dalam kasus di atas, _dealer_ membuat request ke pabrik untuk membuat mobil sesuai dengan spesifikasi yang Anda inginkan disebut sebagai **Constructor** dalam OOP.

Constructor digunakan untuk **menginisialisasi nilai awal** dari sebuah objek ketika dibuat. Constructor akan otomatis dipanggil ketika objek dibuat, tanpa perlu dipanggil secara eksplisit.

<br>

**Contoh penggunaan Constructor**

```java
// Class Mobil dengan Constructor
class Mobil {
    String merk;
    String warna;
    double harga;

    // Constructor: Digunakan untuk menginisialisasi objek dengan nilai awal
    Mobil(String merk, String warna, double harga) {
        this.merk = merk;
        this.warna = warna;
        this.harga = harga;
    }

    void tampilkanInfo() {
        System.out.println("Mobil " + merk + " berwarna " + warna + ", dengan harga Rp " + harga);
    }
}

// Membuat object dengan constructor
public class Main {
    public static void main(String[] args) {
        // Menggunakan constructior Mobil() untuk membuat Toyota Avanza Hitam
        Mobil avanzaPutih = new Mobil("Toyota Avanza", "Putih", 250000000);
        avanzaPutih.tampilkanInfo();

        // Menggunakan constructior Mobil() untuk membuat Honda Civic Putih
        Mobil civicMerah = new Mobil("Honda Civic", "Merah", 450000000);
        civicMerah.tampilkanInfo();
    }
}
```

Setelah Anda lelah menunggu waktu _indent_ membeli mobil, akhirnya waktu yang ditunggu-tunggu telah tiba. Anda menerima mobil Toyota Avanza Putih dengan sangat kegirangan. Kemudian, Anda mulai menghidupkan mesin, mengemudi, menyalakan lampu, dan membunyikan klakson untuk memastikan bahwa mobil tersebut bisa digunakan.

_Nahh_, tindakan-tindakan tersebut mirip dengan **Methods** dalam OOP.

Methods adalah tindakan-tindakan yang bisa dilakukan oleh sebuah objek setelah dibuat. Methods bisa dipanggil berkali-kali dan memiliki logika tertentu untuk menjalankan suatu aksi.

```java
class Mobil {
    String merk;
    String warna;
    double harga;

    // Constructor
    Mobil(String merk, String warna, double harga) {
        this.merk = merk;
        this.warna = warna;
        this.harga = harga;
    }

    // Method untuk menampilkan informasi mobil
    void tampilkanInfo() {
        System.out.println("Mobil " + merk + " berwarna " + warna + ", dengan harga Rp " + harga);
    }

    // Method untuk menyalakan mesin
    void nyalakanMesin() {
        System.out.println("Mobil " + merk + " menyalakan mesin...");
    }

    // Method untuk membunyikan klakson
    void bunyikanKlakson() {
        System.out.println("Mobil " + merk + " membunyikan klakson: Beep! Beep!");
    }
}

// Menggunakan methods dalam program
public class Main {
    public static void main(String[] args) {
        Mobil avanzaPutih = new Mobil("Toyota Avanza", "Putih", 250000000);

        // Memanggil methods untuk melakukan aksi
        avanzaPutih.tampilkanInfo();
        avanzaPutih.nyalakanMesin();
        avanzaPutih.bunyikanKlakson();
    }
}
```

<br>

**Perbandingan Constructor vs Methods dalam Java**

| **Fitur**                         | **Constructor**                              | **Methods**                                                   |
| --------------------------------- | -------------------------------------------- | ------------------------------------------------------------- |
| **Kapan dipanggil?**              | Saat objek dibuat                            | Bisa dipanggil kapan saja setelah objek dibuat                |
| **Tujuan**                        | Menginisialisasi nilai awal suatu objek      | Melakukan aksi pada objek (misalnya, menjalankan fitur mobil) |
| **Bisa dipanggil ulang?**         | ❌ Tidak                                     | ✅ Bisa                                                       |
| **Nama harus sama dengan class?** | ✅ Ya                                        | ❌ Tidak harus sama dengan class                              |
| **Digunakan untuk apa?**          | Menentukan bagaimana objek dibuat            | Menentukan apa yang bisa dilakukan oleh objek                 |
| **Contoh di dunia nyata**         | Menentukan spesifikasi mobil sebelum membeli | Menghidupkan mesin, membunyikan klakson                       |

<br>

### Access Modifier

_Access Modifier_ adalah **pengaturan hak akses** terhadap atribut (variabel) dan methods (fungsi) dalam suatu kelas. _Access Modifier_ menentukan apakah bagian dari kelas tersebut bisa diakses dari luar kelas atau hanya dari dalam kelas itu sendiri.

![](https://net-informations.com/java/basics/img/access-modifier.png)

Dalam Java, terdapat empat jenis **Access Modifier**. Kita bisa membayangkan Access Modifier seperti bagian-bagian dalam sebuah mobil sebagai berikut.

- **`public` (Setir & Jok Mobil)** → Semua orang bisa melihat dan menyentuhnya. Bisa diakses dari oleh seluruh class di manapun file berada.

```java
class Mobil {
    public String merk;

    public void tampilkanMerk() {
        System.out.println("Merk mobil: " + merk);
    }
}

public class Main {
    public static void main(String[] args) {
        Mobil avanzaPutih = new Mobil("Toyota Avanza", "Putih", 250000000);
        avanzaPutih.merk = "Toyota"; // Bisa diakses karena atribut bersifat public
        avanzaPutih.tampilkanMerk(); // Bisa dipanggil karena method bersifat public
    }
}
```

<br>

- **`private` (Mesin Mobil)** → Terbatas hanya mekanik atau sistem internal yang boleh mengaksesnya. _Modifier_ ini biasa digunakan untuk melindungi data penting agar tidak bisa diubah sembarangan.

```java
class Mobil {
    private int kecepatanMaks = 200;

    private void tampilkanKecepatan() {
        System.out.println("Kecepatan Maksimum: " + kecepatanMaks + " km/jam");
    }
}

public class Main {
    public static void main(String[] args) {
        Mobil avanzaPutih = new Mobil("Toyota Avanza", "Putih", 250000000);
        // avanzaPutih.kecepatanMaks = 250; // Error! Tidak bisa diakses karena private
        // avanzaPutih.tampilkanKecepatan(); // Error! Tidak bisa diakses karena private
    }
}
```

Solusi dari kendala di atas adalah menggunakan `getter` dan `setter` untuk mengakses atribut private yang akan dibahas lebih lanjut pada materi **_encapsulation_**.

<br>

- **`protected` (Sistem Rem ABS)** → Hanya dapat digunakan oleh model mobil yang sejenis atau memiliki atribut yang sama.

```java
class Mobil {
    protected String jenisRem = "ABS";

    protected void tampilkanJenisRem() {
        System.out.println("Jenis Rem: " + jenisRem);
    }
}

// Subclass (Kelas Turunan)
class MobilBalap extends Mobil {
    void tampilkanDetail() {
        System.out.println("Mobil balap ini rem jenis " + jenisRem);
    }
}

public class Main {
    public static void main(String[] args) {
        MobilBalap ferrariMerah = new MobilBalap();
        ferrariMerah.tampilkanJenisRem(); // Bisa diakses karena protected
        ferrariMerah.tampilkanDetail();
    }
}
```

<br>

- **`default` (Interior Mobil)** → Hanya bisa diakses dalam satu pabrik yang sama.

```java
class Mobil {
    String model = "SUV"; // Tidak ada access modifier (default)

    void tampilkanModel() {
        System.out.println("Model Mobil: " + model);
    }
}

public class Main {
    public static void main(String[] args) {
        Mobil mobil1 = new Mobil();
        mobil1.tampilkanModel(); // Bisa diakses karena dalam paket yang sama
    }
}
```

### Class Diagram

**Class Diagram** adalah visualisasi yang digunakan dalam **UML (Unified Modeling Language)** untuk menggambarkan struktur suatu kelas dalam pemrograman berorientasi objek (OOP). Komponen dari visualisasi ini akan terdiri dari:

- **Nama _Class_**
- **_Attribute_ (Variabel)**
- **_Methods_ (Fungsi)**
- **_Relationship_ (Association, Inheritance, Aggregation, Composition)**

![](https://khalilstemmler.com/img/blog/object-oriented/uml/uml-class-diagram-cheat-sheet.png)

## Four Pillars of OOP

### Encapsulation

### Inheritance

### Polymorphism

### Abstraction

# End

```

```

```

```
