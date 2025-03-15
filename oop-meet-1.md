# OOP: Chapter 1

<!-- [TOC] -->

## **Table of Contents**

- [Hi, I'm Java!](#hi-im-java)
  - [Java Architecture](#java-architecture)
  - [How _Java_ Works?](#how-java-works)
- [Fundamental OOP](#fundamental-oop)
  - [Class vs Object](#class-vs-object)
  - [Constructor & Methods](#constructor--methods)
  - [Access Modifier](#access-modifier)
  - [Class Diagram](#class-diagram)
- [Four Pillars of OOP](#four-pillars-of-oop)
  - [Encapsulation](#encapsulation)
  - [Inheritance](#inheritance)
  - [Polymorphism](#polymorphism)
  - [Abstraction](#abstraction)

---

## Hi, I'm Java!👋

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQmSmJgXu6kW8ONXMTLq0LD6BJGFV3Hoc0DRg&s)

Halo! Aku adalah **Java**, salah satu bahasa pemrograman yang paling populer di dunia. Aku lahir pada tahun 1995 di bawah naungan **Sun Microsystems** dan sejak itu telah digunakan untuk membangun berbagai macam aplikasi, mulai dari **aplikasi desktop, sistem backend, hingga aplikasi mobile seperti Android**. Aku dikenal dengan prinsip **"Write Once, Run Anywhere (WORA)"**, yang berarti kode yang ditulis denganku dapat berjalan di berbagai sistem operasi tanpa perlu diubah. Itu semua berkat **Java Virtual Machine (JVM)** yang menerjemahkan instruksiku agar bisa dijalankan di berbagai perangkat. Dengan sintaks yang mudah dipahami, keamanan yang kuat, dan komunitas yang besar, aku siap membantumu menjadi seorang developer yang handal! 🚀

### Java Architecture

![](https://miro.medium.com/v2/resize:fit:1400/1*lLBwoFv6RAZ15vRuK6yFQg.png)

**JDK (Java Development Kit)** – Paket yang berisi peralatan (_tools_) untuk mengembangkan program Java seperti _compiler_ (javac).

**JRE (Java Runtime Environment)** – Lingkungan yang dibutuhkan untuk menjalankan aplikasi Java, termasuk JVM.

**JVM (Java Virtual Machine)** – Mesin virtual yang menjalankan _bytecode_ Java di perangkat keras.

<br>

#### Sebuah Analogi Arsitektur Java 🎭

> Bayangkan kamu adalah seorang **sutradara film** yang ingin membuat dan menayangkan film di berbagai bioskop. Dalam dunia Java, **JDK (Java Development Kit)** adalah seperti **studio film** yang menyediakan semua alat produksi, seperti kamera, editor, dan efek khusus, agar kamu bisa membuat film dengan sempurna.
>
> Setelah film selesai, kamu memerlukan **JRE (Java Runtime Environment)**, yang bisa diibaratkan sebagai **bioskop** tempat film bisa diputar—bioskop ini sudah memiliki layar, proyektor, dan kursi penonton agar pengalaman menonton menjadi nyaman.
>
> _Tapi_, agar film bisa benar-benar diproyeksikan ke layar, bioskop membutuhkan seorang **operator proyektor** yang mengerti cara menampilkan film dalam format yang sesuai. Di sinilah **JVM (Java Virtual Machine)** berperan—sebagai **operator proyektor** yang menerjemahkan format film (_bytecode Java_) menjadi tampilan yang bisa dinikmati di berbagai layar (_sistem operasi_).
>
> Dengan kombinasi ini, film yang dibuat di satu studio bisa diputar di banyak bioskop tanpa perlu diubah formatnya, seperti halnya kode Java yang bisa berjalan di berbagai perangkat berkat konsep **Write Once, Run Anywhere (WORA).** <br> <br>

### How _Java_ Works?

Gambar berikut menjelaskan bagaimana **_Java_** bekerja, dari proses menulis kode hingga menjalankan program di sistem operasi. Berikut adalah tahapan-tahapan utama dalam proses tersebut:

![](https://media.licdn.com/dms/image/v2/C4E12AQFHYeU8N2ZDCw/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1651746917137?e=2147483647&v=beta&t=6AzWmgQVEkGpvTalSPWYX0BVagXGGbT83FOM9Xj75ds)

#### Tahap 1-3: Menulis Kode Sumber Java

Seorang **developer (programmer)** menulis program menggunakan bahasa Java. Program ini dibuat dalam bentuk file teks dengan ekstensi `.java`. Misalnya, dalam gambar ditunjukkan file dengan nama `HelloWorld.java`. Kode sumber ini berisi instruksi-instruksi dalam bahasa pemrograman Java yang masih bisa dibaca manusia, namun belum dapat langsung dijalankan oleh komputer.

#### Tahap 4-6: Kompilasi Kode Sumber ke _Bytecode_

Setelah kode sumber selesai ditulis, langkah berikutnya adalah **kompilasi** menggunakan Java Compiler yang disebut **`javac`**. Kompilasi ini dilakukan untuk mengubah kode sumber Java menjadi **bytecode**, yang merupakan bentuk instruksi yang lebih efisien tetapi tidak dapat dibaca langsung oleh manusia. Hasil dari proses ini adalah file `.class`, misalnya `HelloWorld.class`. File ini berisi bytecode, yaitu serangkaian instruksi yang tidak bergantung pada sistem operasi atau perangkat keras tertentu.

#### Tahap 7-10: Eksekusi _Bytecode_ dengan JVM

Setelah kode dikompilasi menjadi _bytecode_, langkah berikutnya adalah menjalankannya dengan menggunakan **Java Virtual Machine (JVM)**. JVM berfungsi sebagai interpreter yang menerjemahkan _bytecode_ ke dalam instruksi yang dapat dimengerti oleh sistem operasi. Karena Java menggunakan JVM, program yang telah dikompilasi dapat dijalankan di berbagai sistem operasi tanpa perlu dikompilasi ulang. Inilah yang membuat Java dikenal dengan konsep **"Write Once, Run Anywhere (WORA)"**.

#### Tahap 11: Menampilkan Output Program

Setelah JVM mengeksekusi _bytecode_, hasil dari eksekusi ini akan dikirim ke **sistem operasi**, yang kemudian menampilkan outputnya di layar. Misalnya, jika program ditulis untuk mencetak teks **"Hello World"**, maka sistem operasi akan menampilkan teks tersebut kepada pengguna.

---

## Fundamental OOP

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

    // Constructor tanpa parameter
    Mobil(){}

    // Constructor dengan parameter
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

        // Membuat instance dengan cara 1: memanggil constructor tanpa parameter
        Mobil avanzaPutih = new Mobil()
        avanzaPutih.merk = "Toyota Avanza";
        avanzaPutih.warna = "Putih";
        avanzaPutih.harga = 250000000;

        // Membuat instance dengan cara 2: memanggil constructor dengan parameter
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

**Tambahan:**

keyword `static` :

- `static` dalam Java digunakan untuk membuat atribut atau metode milik kelas, bukan milik objek tertentu. Keyword `static` bisa digunakan pada atribut maupun method.
- **Atribut `static`** → Nilai atribut dibagikan untuk semua objek dari kelas tersebut.
- **Method `static`** → Method dapat dipanggil tanpa membuat objek.

keyword `final` :

- `final` digunakan untuk membuat atribut atau metode yang **tidak bisa diubah atau diwarisi**.
- **Atribut `final`** → Atribut tidak bisa diubah setelah diinisialisasi.
- **Method `final`** → Method tidak bisa di-_override_ oleh subclass. (akan dibahas lebih lanjut di [Inheritance](#inheritance))

Penggunaan kedua keyword tersebut (`static final`) pada atribut bisa membuat suatu atribut menjadi sebuah KONSTANTA yang bisa diakses di mana pun. Namun, pemakaiannya di method jarang digunakan.

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

---

### Class Diagram

**Class Diagram** adalah visualisasi yang digunakan dalam **UML (Unified Modeling Language)** untuk menggambarkan struktur suatu kelas dalam pemrograman berorientasi objek (OOP). Class diagram dibuat ketika kita ingin merancang aplikasi yang akan kita buat, artinya sebelum kita _ngoding_ kita harus buat class diagram-nya terlebih dahulu. _Nahh_, class diagram tersebut nantinya bakal jadi acuan waktu _ngoding_ nantinya. Class diagram akan terdiri dari komponen-komponen berikut:

- **Nama _Class_**
- **Attribute (Variabel)**
- **Methods (Fungsi)**
- **Relationship (Association, Aggregation, Composition)**

#### Common Components

_Eitss_, tapi kita tidak boleh sembarangan _lho_ dalam membuat class diagram. Karena sudah terstandar UML, class diagram harus dibuat berdasarkan aturan berikut.

![](https://khalilstemmler.com/img/blog/object-oriented/uml/uml-class-diagram-cheat-sheet.png)

#### Class Diagram Format

| **ClassName**                                                                                                     |
| ----------------------------------------------------------------------------------------------------------------- |
| `visibility` Attribute A: datatype <br> `visibility` Attribute B: datatype <br> ...                               |
| `visibility` Operation A(`arg list`): return type <br> `visibility` Operation B(`arg list`): return type <br> ... |

**Keterangan:**

- `visibility` = access modifier symbols
  ![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTePsEaJpEwcPu1KZKTpDoqaw08LD1N_ropFg&s)
- `arg list` = parameter
- `attribute` = variabel
- `operation` = method/constructor

<br>

##### **Catatan Penulisan**

Setelah kita paham tentang bagaimana komponen-komponen dalam class diagram, sekarang kita perlu paham _nih_ bagaimana cara kita membuat class diagram. Dalam pembuatan class diagram Java, ada beberapa aturan standar yang perlu diperhatikan sebagai berikut:

**1. Nama Class**

📌 **Aturan:**

- Nama class harus menggunakan huruf kapital di awal (**PascalCase**).
- Jika terdiri dari lebih dari satu kata, gunakan huruf kapital untuk setiap kata.

**Contoh:**  
✅ `Mobil`, `BankAccount`, `PelangganVIP`  
❌ `mobil`, `bankaccount`, `pelangganvip`

<br>

**2. Nama Method**

📌 **Aturan:**

- Nama method harus menggunakan huruf kecil di awal, dan kata berikutnya menggunakan huruf kapital (**camelCase**).
- Nama method **harus menggunakan kata kerja** karena menunjukkan aksi.
- Boolean method biasanya diawali dengan `is`, `has`, atau `can`.

🔹 **Contoh umum:**  
✅ `getNama()`, `hitungTotalHarga()`, `tampilkanInfo()`  
❌ `GetNama()`, `Hitung_total_harga()`, `tampilkan_info()`

🔹 **Contoh Boolean Method:**  
✅ `isAktif()`, `hasDiskon()`, `canWithdraw()`  
❌ `cekAktif()`, `diskonAda()`, `bolehTarik()`

<br>

**3. `arg list` dalam UML**

📌 **Aturan:**

- Dalam UML, daftar parameter method **hanya mencantumkan tipe data** (tanpa nama variabel).
- Dalam Java, parameter menggunakan **camelCase**.

<br>

**4. Method yang Tidak Mengembalikan Nilai**

📌 **Aturan:**

- Jika sebuah method tidak mengembalikan nilai, gunakan `void`.
- Method `void` biasanya digunakan untuk **menjalankan aksi** tanpa menghasilkan output yang dapat dikembalikan.

<br>

**5. Static Method dan Static Field**

📌 **Aturan:**

- Dalam **UML**, method atau field `static` **digarisbawahi (underlining)** namun di beberapa kasus dibuat _italic_.

<br>

**6. Final Field (Konstanta)**

📌 **Aturan:**

- Field `final` (konstanta) ditulis dalam **HURUF_KAPITAL** dan menggunakan **underscore** untuk pemisah kata.
- Jika `final` digunakan tanpa `static`, maka nilai atribut masih bisa berbeda untuk setiap objek.
- Jika `final` digunakan bersama `static`, maka nilainya **bersifat konstan untuk seluruh objek** dari class tersebut.

#### **Contoh Class Diagram**

Supaya lebih mudah memahami class diagram, kita akan mengambil contoh untuk membuat suatu class `Employee` dengan beberapa atribut seperti nama, gaji, id, dst.

| Employee                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| - name: String <br> - payRate: double <br> - ID: int <br> - _nextID_: int <br> +_STARTING_PAY_RATE_: double                                                                                                              |
| + Employee(String) <br> + Employee(String, double) <br> + getName(): String <br> + getID(): int <br> + getPayRate(): double <br> + changeName(String): void <br> + changePayRate(double): void <br> + _getNextID()_: int |

Berdasarkan class diagram di atas, beberapa informasi yang bakal jadi acuan kita yaitu:

- class `Employee` memiliki 5 atribut.
- atribut `ID` dan `STARTING_PAY_RATE` dibuat kapital karena atribut **final**.
- atribut `nextID` dan `STARTING_PAY_RATE` dibuat miring (boleh digaris bawah) karena atribut **static**.

**Contoh Java Class berdasarkan Class Diagram**

```java
public class Employee {
    private String name;
    private double payRate;
    private final int ID;
    private static int nextID = 1000;
    public static final double STARTING_PAY_RATE = 7.75;

    public Employee(String name) {
        this.name = name;
        ID = getNextID();
        payRate = STARTING_PAY_RATE;
    }

    public Employee(String name, double startingPay) {
        this.name = name;
        ID = getNextID();
        payRate = startingPay;
    }

    public String getName() {
        return name;
    }

    public int getID() {
        return ID;
    }

    public double getPayRate() {
        return payRate;
    }

    public void changeName(String newName) {
        name = newName;
    }

    public void changePayRate(double newRate) {
        payRate = newRate;
    }

    public static int getNextID() {
        int id = nextID;
        nextID++;
        return id;
    }
}
```

#### Relationship in Class Diagram

##### Association

**Asosiasi** adalah hubungan antara dua kelas yang **saling terhubung tetapi independen**. Tidak ada kepemilikan di antara mereka.

**Analogi: Mobil dan Pengemudi**

- **Mobil** dapat digunakan oleh **pengemudi** yang berbeda.
- Pengemudi bisa mengendarai mobil yang berbeda.
- Jika mobil rusak, pengemudi masih ada.
- Jika pengemudi pindah kerja, mobil tetap ada.

> <br>
> Association bersifat "menggunakan" bukan "memiliki" <br>
> <br>

<br>

##### Aggregation

**Agregasi** adalah hubungan di mana satu objek **memiliki objek lain**, tetapi objek tersebut masih bisa ada tanpa objek pemiliknya.

**Analogi: Mobil dan Mesin**

- **Mobil** memiliki **Mesin**.
- Jika mobil dihancurkan, mesin masih bisa dilepas dan dipakai di mobil lain.
- Mesin bisa dipindahkan dari satu mobil ke mobil lain.

> <br>
> Aggregation menunjukkan "kepemilikan yang lemah", tetapi tetap ada keterikatan <br>
> <br>

<br>

##### Composition

**Komposisi** adalah hubungan di mana satu objek **memiliki objek lain secara eksklusif** dan jika objek pemilik dihapus, maka objek yang dimiliki juga ikut hilang.

**Analogi: Mobil dan Rangka**

- **Mobil** memiliki **rangka**.
- Jika mobil dihancurkan, rangka mobil tentu tidak bisa digunakan lagi.
- Rangka adalah bagian penting dari mobil, sehingga mobil tidak bisa ada tanpa rangka.

> <br>
> Hubungan ini menunjukkan "kepemilikan yang kuat" dan tidak bisa dipisahkan <br>
> <br>

<br>

##### **Perbandingan Association, Aggregation, dan Composition**

| **Jenis Hubungan** | **Definisi**                                                                                                    | **Tingkat Keterikatan** | **Simbol**                                        |
| :----------------: | --------------------------------------------------------------------------------------------------------------- | :---------------------: | ------------------------------------------------- |
|   _Association_    | Hubungan antara dua kelas tanpa kepemilikan                                                                     |          Lepas          | Garis lurus tanpa panah (`----`)                  |
|   _Aggregation_    | Satu kelas memiliki kelas lain, tetapi yang dimiliki masih bisa berdiri sendiri                                 |         Longgar         | Garis dengan ujung belah ketupat kosong (`◇----`) |
|   _Composition_    | Satu kelas memiliki kelas lain secara eksklusif, jika pemilik dihapus maka objek yang dimiliki juga ikut hilang |          Erat           | Garis dengan ujung belah ketupat penuh (`◆----`)  |

---

## Four Pillars of OOP

Setelah kita paham tentang fundamental OOP, maka kita akan berkenalan lebih dalam dengan OOP. Ide dari OOP sendiri ternyata belum cukup hanya dijelaskan dengan konsep **class & object**. Konsep tersebut melahirkan beberapa ide lain yang akan menjadi submateri ke tiga kali ini yang disebut dengan **Four Pillars of OOP.**

### 1<sup>st</sup> Pillar: Encapsulation

**Encapsulation** adalah konsep dalam OOP dengan **menyembunyikan atribut dari akses langsung** dan hanya dapat diakses melalui **method khusus (`getter` dan `setter`)**. Dengan konsep ini, kita bisa mengontrol bagaimana data suatu objek bisa diakses dan diubah.

> <br> Bayangkan sebuah **mobil** yang memiliki berbagai komponen, seperti mesin, rem, dan roda. Sebagai pengemudi, kita tidak bisa langsung mengakses mesin secara langsung saat berkendara. Kita hanya bisa mengontrol mobil melalui **pedal gas, rem, dan setir** tanpa perlu mengatur bagaimana mesin bekerja secara internal. Hal tersebut menunjukkan terdapat fitur-fitur dari disembunyikan dari pengemudi. Apabila kita ingin mengubah pengaturan mesin maka kita perlu montir (`getter` dan `setter`). Ide tersebut memastikan bahwa **mobil tetap aman** dan **tidak rusak karena salah penggunaan** <br> <br>

Sebelumnya kita sudah membahas tentang **access modifier**. _Nah_, pada pilar ini kita akan memanfaatkan _access modifier_ `private` atau `protected` untuk mengamankan atribut-atribut pada suatu class. Perhatikan contoh berikut.

<br>

#### ❌ **Contoh Tanpa Encapsulation (Data Tidak Aman)**

```java
class Mobil {
    public String merk;
    public int kecepatan;
}

public class Main {
    public static void main(String[] args) {
        Mobil mobilSaya = new Mobil();
        mobilSaya.merk = "Toyota";
        mobilSaya.kecepatan = -50; // ❌ ERROR LOGIKA: Kecepatan tidak boleh negatif!

        System.out.println("Mobil: " + mobilSaya.merk);
        System.out.println("Kecepatan: " + mobilSaya.kecepatan + " km/jam");
    }
}
```

📌 **Masalah:**

- Atribut bisa diakses dan diubah secara langsung, yang bisa menyebabkan kesalahan logika (contoh: kecepatan negatif).
- Tidak ada perlindungan terhadap nilai yang tidak valid.

<br>

#### ✅ Dengan Encapsulation (Data Aman)

Sekarang, jika ingin menerapkan encapsulation maka kita perlu mengatur hak akses untuk setiap atribut menjadi `private` sehingga tidak bisa diubah secara langsung.

```java
class Mobil {
    private String merk;
    private int kecepatan;

    // Constructor untuk menginisialisasi mobil
    public Mobil(String merk) {
        this.merk = merk;
        this.kecepatan = 0; // Mobil diam saat dibuat
    }
}
```

***Lalu, bagaimana *dong* cara kita mengubah nilai kecepatan?***

Di sinilah, kita perlu membuat `getter` dan `setter`

- `getter` adalah method yang digunakan untuk **menampilkan nilai nilai** atribut private tanpa mengubahnya.
- `setter` adalah method yang digunakan untuk **mengubah ulang nilai** dari atribut private.

Berikut kita lengkapi class `Mobil` dengan menambahkan `getter`, `setter`, dan method lain (`tambahKecepatan` dan `kurangiKecepatan`) untuk mengelola perubahan nilai.

```java
class Mobil {
    private String merk;
    private int kecepatan;

    // Constructor untuk menginisialisasi mobil
    public Mobil(String merk) {
        this.merk = merk;
        this.kecepatan = 0; // Mobil diam saat dibuat
    }

    // Getter untuk membaca merk mobil
    public String getMerk() {
        return merk;
    }

    // Getter untuk membaca kecepatan
    public int getKecepatan() {
        return kecepatan;
    }

    // Method untuk menambah kecepatan dengan batasan
    public void tambahKecepatan(int jumlah) {
        if (jumlah > 0) {
            kecepatan += jumlah;
            System.out.println("Mobil " + merk + " bertambah kecepatan menjadi " + kecepatan + " km/jam.");
        } else {
            System.out.println("Kecepatan harus bertambah positif!");
        }
    }

    // Method untuk mengurangi kecepatan dengan batasan
    public void kurangiKecepatan(int jumlah) {
        if (jumlah > 0 && kecepatan - jumlah >= 0) {
            kecepatan -= jumlah;
            System.out.println("Mobil " + merk + " berkurang kecepatan menjadi " + kecepatan + " km/jam.");
        } else {
            System.out.println("Kecepatan tidak boleh negatif!");
        }
    }
}
```

<br>

Apabila di coba akses di `main.java` akan seperti berikut.

```java
public class Main {
    public static void main(String[] args) {
        Mobil mobilSaya = new Mobil("Toyota");

        // Mengakses data dengan getter
        System.out.println("Mobil: " + mobilSaya.getMerk());
        System.out.println("Kecepatan Awal: " + mobilSaya.getKecepatan() + " km/jam");

        // Menambah dan mengurangi kecepatan dengan kontrol yang aman
        mobilSaya.tambahKecepatan(50);
        mobilSaya.kurangiKecepatan(20);
        mobilSaya.kurangiKecepatan(100); // ❌ Tidak bisa, kecepatan tidak boleh negatif!
    }
}
```

Dengan menerapkan encapsulation, maka:

- Kecepatan mobil tidak bisa diubah secara langsung dari luar kelas.
- Mobil tidak bisa memiliki kecepatan negatif.
- Data hanya bisa diakses melalui method yang aman.
- Jika ada perubahan aturan kecepatan, hanya method terkait yang perlu diperbarui.

> <br> Jadi, mulai sekarang ketika Anda membuat class maka kalian harus mengatur **atribut menjadi `private`** serta **membuat `getter` dan `setter`** untuk setiap atribut tersebut. <br> <br>

<br>

### 2<sup>nd</sup> Pillar: Inheritance

**Inheritance** adalah konsep dalam OOP yang memungkinkan sebuah class **mewarisi atribut dan metode dari kelas lain (_parent_)** serupa dengan konsep pewarisan karakteristik (atribut/method) dari induk ke anak.

- **Superclass (Kelas Induk)** → Kelas utama yang mendefinisikan atribut dan metode dasar.
- **Subclass (Kelas Anak)** → Kelas yang mewarisi dari superclass dan bisa menambahkan atau mengubah perilaku.

Inheritance memungkinkan **kode menjadi lebih efisien dan terstruktur**, karena subclass tidak perlu mendefinisikan ulang fitur yang sudah ada di superclass.

> <br>Bayangkan ada **mobil generik** yang memiliki karakteristik dasar seperti jumlah roda, warna, dan mesin. Lalu, ada **mobil listrik** dan **mobil sport** yang merupakan jenis khusus dari mobil generik. Mobil listrik memiliki baterai, sedangkan mobil sport memiliki turbo. Semua jenis mobil ini tetap memiliki karakteristik dasar yang sama seperti jumlah roda dan mesin, tetapi mereka juga memiliki fitur tambahan masing-masing. <br><br>

Lalu, bagaimana contohnya? Saat ini sudah marak produksi mobil listrik seperti Hyundai, Tesla, BYD, dsb. Secara umum tidak ada perbedaan antar mobil biasa dan mobil listri, hanya saja mobil listrik menggunakan sumber tenaga dari baterai.

Maka, kita dapat menjadikan class `Mobil` sebagai superclass.

```java
// Superclass: Mobil
class Mobil {
    private String merk;
    private String warna;
    private double harga;

    // Constructor untuk menentukan spesifikasi mobil
    Mobil(String merk, String warna, double harga) {
        this.merk = merk;
        this.warna = warna;
        this.harga = harga;
    }

    // Method untuk menampilkan informasi mobil
    protected void tampilkanInfo() {
        System.out.println("Mobil " + merk + " berwarna " + warna + ", dengan harga Rp " + harga);
    }

    // getter dan setter
    ...
}
```

Karena kesamaan karakteristik dari mobil listrik dengan mobil biasa, maka kita akan membuat class `MobilListrik` sebagai subclass yang meng-_extend_ class `Mobil` tanpa perlu menuliskan ulang atribut atau method yang berkaitan dengan mobil secara umum. Kita hanya perlu menambahkan atribut dan method yang menjadi ciri khas dari mobil listrik yaitu `kapasitasBaterai` dan `infoBaterai()`.

```java
// Subclass: MobilListrik (Mewarisi dari Mobil)
class MobilListrik extends Mobil {
    private int kapasitasBaterai;

    // constructor 1
    MobilListrik(){}

    // constructor 2
    MobilListrik(String merk, String warna, double harga, double kapasitasBaterai) {
        super(merk, warna, harga); // untuk memanggil constructor di superclass
        this.kapasitasBaterai = kapasitasBaterai;
    }

    void infoBaterai() {
        System.out.println("Baterai saat ini " + kapasitasBaterai + " persen");
    }

    // getter dan setter
    ...
}
```

_Wah_ ada hal baru lagi _nih_, pada constructor subclass di atas terdapat keyword `super`. Jadi, dengan keyword tersebut kita dapat memanggil atribut ataupun method dari superclass. Pada konteks di atas, setiap kali objek `MobilListrik` dibuat maka untuk inisialisasi nilai pada atribut superclass cukup memanggil constructor dari superclass dengan `super()` lalu menambahkan inisialiasi untuk atribut khas dari `MobilListrik` yaitu `kapasitasBaterai`.

Sehingga, pada `Main.java` dapat dilakukan percobaan sebagai berikut.

```java
public class Main {
    public static void main(String[] args) {
        MobilListrik hyundaiIoniq5 = new MobilListrik();
        // Mewarisi atribut dari Mobil
        hyundaiIoniq5.merk = "Hyundai Ioniq 5";
        hyundaiIoniq5.warna = "Putih";
        hyundaiIoniq5.harga = 831100000;

        // Atribut tambahan di subclass
        hyundaiIoniq5.kapasitasBaterai = 100;

        hyundaiIoniq5.tampilkanInfo();  // Memanggil method dari superclass
        hyundaiIoniq5.infoBaterai(); // Memanggil method dari subclass
    }
}
```

**_Mudah sekali bukan?_**

<br>

### Polymorphism

Dalam OOP, polymorphism merupakan konsep di mana suatu class melakukan aksi yang sama dengan **implementasi berbeda** tergantung pada objek yang menggunakannya.

> <br> Bayangkan Anda memiliki **mobil otomatis** dan **mobil manual**. Kedua mobil ini memiliki tujuan yang sama yaitu **dapat dikendarai**. Namun, cara mengoperasikannya berbeda. **Mobil otomatis** hanya perlu menekan tombol start engine, sedangkan **mobil manual** harus menggunakan kunci. <br> <br>

Terdapat 2 jenis polymorphism, yaitu:

- **Dynamic Polymorphism (_Overriding method_)** → Method yang sama dengan perilaku berbeda dalam subclass.
- **Static Polymorphism (_Overloading method_)** → Dalam method overloading, kita bisa memiliki beberapa method dengan nama yang sama, tetapi parameter berbeda.

<br>

#### Contoh Dynamic Polymorphism

Pada kasus inheritance sebelumnya, sering terjadi kondisi di mana subclass akan memodifikasi isi/logika dari method di superclass-nya. Aktivitas tersebut bisa dilakukan dengan teknik `overriding`. Misalnya, ketika mobil listrik dipanggil maka method `tampilkanInfo()` maka detail dari kapasitasBaterai akan ditampilkan juga. Sehingga, kita perlu meng-_override_ (menimpa) method tampilkanInfo() agar menambahkan atribut baru untuk ditampilkan.

```java
// Subclass: MobilListrik (Mewarisi dari Mobil)
class MobilListrik extends Mobil {
    private int kapasitasBaterai;

    // constructor 1
    MobilListrik(){}

    // constructor 2
    MobilListrik(String merk, String warna, double harga, double kapasitasBaterai) {
        super(merk, warna, harga); // untuk memanggil constructor di superclass
        this.kapasitasBaterai = kapasitasBaterai;
    }

    @Override
     public void tampilkanInfo(){
        System.out.println("Mobil Listrik " + super.getMerk() + " berwarna " + super.getWarna()
        + ", dengan harga Rp " + super.getHarga() + " kapasitas baterai " + this.kapasitasBaterai);
    }

    void infoBaterai() {
        System.out.println("Baterai saat ini " + kapasitasBaterai + " persen");
    }

    // getter dan setter
    ...
}
```

Namun, perlu diingat bahwa jika suatu method dari superclass memiliki keyword `final` maka method tersebut tidak bisa di-_override_ oleh subclass-nya.

<br>

#### Contoh Static Polymorphism

Apabila kita ingin membuat method `infoBaterai()` baru dengan menambah parameter baru yaitu `tambahanDaya` dan `pengisianCepat` maka _overloading_ akan terjadi.

```java
// Subclass: MobilListrik (Mewarisi dari Mobil)
class MobilListrik extends Mobil {
    private int kapasitasBaterai;

    // constructor 1
    MobilListrik(){}

    // constructor 2
    MobilListrik(String merk, String warna, double harga, double kapasitasBaterai) {
        super(merk, warna, harga); // untuk memanggil constructor di superclass
        this.kapasitasBaterai = kapasitasBaterai;
    }

    @Override
     public void tampilkanInfo(){
        System.out.println("Mobil Listrik " + super.getMerk() + " berwarna " + super.getWarna()
        + ", dengan harga Rp " + super.getHarga() + " kapasitas baterai " + this.kapasitasBaterai);
    }

    void infoBaterai() {
        System.out.println("Baterai saat ini " + kapasitasBaterai + " persen");
    }

    // Overloaded method dengan satu parameter
    void infoBaterai(double tambahanDaya) {
        System.out.println("Baterai saat ini " + kapasitasBaterai +
        " persen, akan ditambah " + tambahanDaya + "%");
    }

    // Overloading method dengan dua parameter
    void infoBaterai(double tambahanDaya, boolean pengisianCepat) {
        String metode = pengisianCepat ? "Fast Charging" : "Normal Charging";
        System.out.println("Baterai saat ini " + kapasitasBaterai + " persen, akan ditambah "
                + tambahanDaya + "% menggunakan " + metode + ".");
    }

    // getter dan setter
    ...
}
```

<br>

Sehingga, saat method `infoBaterai()` dipanggil di `Main.java` maka Java akan memilih versi method yang sesuai berdasarkan jumlah dan tipe argumen yang diberikan.

```java
public class Main {
    public static void main(String[] args) {
        // Membuat objek MobilListrik
        MobilListrik tesla = new MobilListrik("Tesla", "Putih", 1500000000, 80);

        // Menampilkan informasi mobil listrik
        tesla.tampilkanInfo();

        // Memanggil metode infoBaterai dengan overloading
        tesla.infoBaterai(); // Tanpa parameter
        tesla.infoBaterai(15); // Dengan parameter tambahan daya
        tesla.infoBaterai(20, true); // Dengan parameter tambahan daya dan mode pengisian cepat
    }
}
```

### Abstraction

akan dibahas di pertemuan 2 (Minggu, 23 Maret 2025) 👋

# End

```

```
