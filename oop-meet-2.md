# OOP: Chapter 2

composed by [_Bimo Ade Budiman Fikri_](https://www.linkedin.com/in/bimoadee/)

![alt text](2.png)

<!-- [TOC] -->

## **Table of Contents**

<!-- - [Hi, I'm Java!](#hi-im-java)
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
  - [Abstraction](#abstraction) -->

---

# Four Pillars of OOP (Continue)

Sejauh ini kita sudah belajar tentang konsep-konsep dalam OOP yaitu [encapsulation](https://github.com/kkokonatsu/OOP-Java/blob/main/oop-meet-1.md#encapsulation), [inheritance](https://github.com/kkokonatsu/OOP-Java/blob/main/oop-meet-1.md#inheritance), dan [polymorphism](https://github.com/kkokonatsu/OOP-Java/blob/main/oop-meet-1.md#polymorphism). Berbekal hal tersebut ternyata masih belum cukup untuk bisa mengakomodasi kebutuhan di dunia nyata. Pemanfaatan _inheritance_ sebagai hubungan antar*class* justru menimbulkan berbagai kompleksitas dalam praktiknya karena keeratan hubungan yang kuat antara _superclass_ dan _subclass_. Maka, dari itu kondsep abstraction hadir untuk bisa memberikan fleksibilitas dalam mendefinisikan hubungan antar*class*.

## Concept of Abstraction

Abstraksi adalah konsep OOP dengan menyembunyikan sebagian atau keseluruhan detail implementasi dari suatu hal (_attribute_ atau _method_) dari suatu objek agar memudahkan pengembangan. Dengan abstraksi, kita akan fokus pada **_apa yang dilakukan oleh suatu objek, bukan bagaimana cara kerjanya_**.

> <br> Bayangkan jika Anda memiliki sebuah mobil Toyota (bensin). Sebagai pengemudi awam, Anda pastinya tahu bagaimana cara menyalakan mobil atau mengerem tanpa harus tahu secara mendalam tentang bagaimana mesin bekerja atau bagaimana sistem transmisi mengubah gigi. Apabila, suatu ketika Anda diharuskan mengendarai mobil Hyundai (listrik) yang memiliki teknologi berbeda. Meskipun berbeda, Anda tentu tetap mengetahui bagaimana cara menghidupkan mesin padahal mobil bensin menggunakan pembakaran internal, mobil listrik menggunakan motor listrik. Bagaimana cara mobil hidup di belakang layar tentunya tidak akan berefek apapun ke bagaimana pengemudi menghidupkan mobil. _Nah_, hal tersebut merupakan konsep dari **_abstraction_**. Dengan abstraksi, pengemudi hanya berfokus pada bagaimana mengoperasikan mobil entah bagaimana pun caranya, sementara detail teknis di balik sistem kendaraan disembunyikan sehingga memberikan pengalaman pengguna yang lebih sederhana dan efektif. <br><br>

Di _Java_, konsep _abstraction_ diwujudkan dengan menggunakan dua buah _keyword_ yaitu `abstract` dan `interface`. Bagaimana perbedaannya?! Simak _section_ berikut.

## Abstract

Sejalan dengan konsep di atas bahwa _abstraction_ fokus pada **_apa yang dilakukan oleh suatu objek, bukan bagaimana cara kerjanya_**. Maka, Kita bisa membuat sebuah `abstract class` yang menyediakan kebutuhan dasar dari suatu mobil seperti atribut dasar (mesin, transmisi, dst) ataupun perilaku dasar (menghidupkan mesin, berhenti, dst). Perilaku dasar tersebut kemudian akan disebut sebagai `abstract method`. Jika kita rangkum maka:

- **`abstract class`** akan menjadi cetakan (_blueprint_) untuk kelas-kelas turunannya. Mirip seperti class pada umumnya (_concrete class_), cetakan tersebut bisa berupa _attribute_, _constructor_, hingga _method_. Namun, perbedaannya adalah `abstract class` tidak dapat di-instansiasi (dibuat objeknya) seperti pada _concrete class_.
- **`abstract method`** merupakan method spesial yang hanya ada pada `abstract class` yang mana tidak memiliki detail implementasi. Artinya, method ini hanya mendefinisikan nama method tanpa memberikan detail implementasinya (kode). Kelas turunannya lah yang harus merinci kode yang sesuai dengan kebutuhan mereka.

Terlihat serupa dengan _inheritance_ bukan? namun, dengan tambahan keyword `abstract` kita bisa menyembunyikan beberapa detail kode agar bisa ditulis di subclass-nya. Biar lebih paham mari simak kode berikut.

Pertama, kita akan membuat `abstract class Mobil`

```java
abstract class Mobil {

    // abstract method
    abstract void hidupkanMesin();

    // concrete method
    void rem() {
        System.out.println("Mobil berhenti...");
    }
}
```

Kedua, kita akan membuat subclass `MobilBensin` dan `MobilListrik` yang meng-extend `abstract class Mobil` seperti _inheritance_ pada umumnya. Asumsikan bahwa mobil bensin dihidupkan dengan kunci (tradisional) dan mobil listrik dengan tombol _engine start_ (modern).

```java
class MobilBensin extends Mobil {
    @Override
    void hidupkanMesin() {
        masukkanKunci();
        System.out.println("Mobil hidup.");
    }

    void masukkanKunci(){
        System.out.println("Kunci dimasukkan.");
    }
}

class MobilListrik extends Mobil {
    @Override
    void hidupkanMesin() {
        pencetTombol();
        System.out.println("Mobil hidup.");
    }

    void pencetTombol(){
        System.out.println("Tombol dipencet.");
    }
}
```

Kode di atas mirip bukan dengan inheritance? Meskipun sama, sebenarnya dengan memanfaatkan `abstract` kita tidak perlu membuat detail kode di superclass (`class Mobil`) sehingga ketika ada perubahan kode kita cukup mengubahnya di _subclass_ (`MobilBesin` atau `MobilListrik`) bukan di _superclass_. Selain itu, karena kita hampir tidak pernah meng-instansiasi `Mobil` sehingga lebih tepat jika Mobil kita buat sebagai **abstract class**.

Maka, jika kita coba di `Main.java` akan seperti berikut.

```java
public class Main {
    public static void main(String[] args) {

        Mobil mobil1 = new MobilBensin();
        mobil1.hidupkanMesin();
        mobil1.rem();

        /*
        * Output:
        * Kunci dimasukkan.
        * Mobil hidup.
        * Mobil berhenti...
        * */

        Mobil mobil2 = new MobilListrik();
        mobil2.hidupkanMesin();
        mobil2.rem();

        /*
        * Output:
        * Tombol dipencet.
        * Mobil hidup.
        * Mobil berhenti...
        * */
    }
}
```

<br>

## Interface

Selanjutnya adalah `interface`. _Interface_ merupakan versi lebih sederhana dari `abstract class` yang mana file ini hanya menyimpan **_abstract method_** tanpa atribut apapun. Konsep `interface` ini lahir sebagai solusi karena _Java_ tidak mendukung adanya konsep _multiple inheritance_ atau suatu subclass meng-_extend_ dua superclass sekaligus. Dengan `interface`, suatu class bisa mengimplementasikan banyak _method_ dari berbagai _interface_ yang mana hal ini tidak bisa dilakukan oleh `abstract class` (karena adanya hubungan _inheritance_). Dalam mendefinisikan hubungannya, interface akan menggunakan keyword **`implements`**.

> <br> Bayangkan kini Anda sudah memiliki mobil listrik. Suatu ketika Anda ingin menambahkan fitur-fitur tambahan yang bersifat opsional seperti kamera belakang atau peningkatan audio. Jika Anda menggunakan _multiple inheritance_ langsung untuk menambahkan fitur-fitur ini, akan ada beberapa masalah yang muncul yaitu **Tidak fleksibel**—Anda harus mengubah struktur kelas mobil yang sudah ada untuk menambahkan fitur-fitur baru, yang membuat desain lebih kaku. **Kompleksitas**—Jika Anda memiliki banyak fitur yang ingin ditambahkan, Anda akan kesulitan untuk memelihara atau memperluas kode karena terikat pada pewarisan kelas yang kompleks .<br><br>

Menggunakan `interface` sebagai cara untuk menambahkan fitur opsional sebagai dekorasi akan sangat efektif karena `interface` berfungsi sebagai kontrak untuk fitur tertentu yang dapat diimplementasikan oleh mobil jenis apapun tanpa memodifikasi desain dasar mobil tersebut.

Misalkan kita ingin memasang 2 fitur tambahan yaitu `SistemAudioTambahan` dan `KameraBelakang`.

```java
// Interface untuk fitur audio tambahan
interface SistemAudioTambahan {
    void naikkanVolume();
    void turunkanVolume();
}

// Interface untuk fitur kamera belakang
interface KameraBelakang {
    void aktifkanKamera();
}
```

Kemudian, fitur tambahan tersebut kita tambahkan ke `MobilListrik` kita dengan _keyword_ `implements`.

```java
class MobilListrik extends Mobil
    implements SistemAudioTambahan, KameraBelakang {

    @Override
    void hidupkanMesin() {
        pencetTombol();
        System.out.println("Mobil hidup.");
    }

    void pencetTombol(){
        System.out.println("Tombol dipencet.");
    }
}
```

Lalu, kita akan mencoba fitur yang baru ditambahkan di `Main.java` seperti berikut.

```java
public class Main {
    public static void main(String[] args) {

        MobilBensin mobil1 = new MobilBensin();
        mobil1.hidupkanMesin();
        mobil1.rem();

        /*
        * Output:
        * Kunci dimasukkan.
        * Mobil hidup.
        * Mobil berhenti...
        * */

        MobilListrik mobil2 = new MobilListrik();
        mobil2.info();
        mobil2.hidupkanMesin();
        mobil2.aktifkanKamera();
        mobil2.naikkanVolume();
        mobil2.turunkanVolume();
        mobil2.rem();

        /*
        * Output:
        * Tombol dipencet.
        * Mobil hidup.
        * kamera hidup...
        * Volume audio naik++
        * Volume audio turun--
        * Mobil berhenti...
        * */
    }
}
```

---

# Generic Type

---

# Exception Handling

---

# End

```

```
