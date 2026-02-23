# OOP: Chapter 1

composed by [_Bimo Ade Budiman Fikri_](https://www.linkedin.com/in/bimoadee/)

![alt text](../1.png)

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

Halo! Aku adalah **Java**. Aku adalah **bahasa pemrograman** yang terkenal dengan prinsip **_"Write Once, Run Anywhere (WORA)"_**. Artinya, kamu cukup menulis kode sekali, dan aku bisa berjalan di Laptop Windows, Macbook, bahkan HP Android!

Agar tidak pusing, bayangkan aku bekerja seperti Produksi Film:

1. **Kamu (Developer)** menulis naskah film (Koding).
2. **JDK (Studio)**: _Java Development Kit_ -> merekam naskah itu menjadi gulungan film.
3. **JVM (Proyektor)**: _Java Virtual Machine_ -> memutar film itu di bioskop mana saja (Laptop/HP).

![](https://miro.medium.com/v2/resize:fit:1400/1*lLBwoFv6RAZ15vRuK6yFQg.png)

> 💡 **Poin Penting:** Kamu tidak perlu pusing memikirkan bagaimana cara kerja proyektornya (JVM) sekarang. Mari kita fokus belajar menulis naskahnya (Koding)!

## Fundamental OOP

Pemrograman Berorientasi Objek (OOP/PBO) itu sebenarnya cara kita meniru dunia nyata ke dalam komputer.

**Analogi Utama: Mobil** 🚗

Sepanjang materi ini, kita akan menggunakan analogi **MOBIL**.

1. **Class vs Object**

Bayangkan kamu datang ke pabrik mobil.

![](https://nonlineardata.com/wp-content/uploads/2020/11/Car_Class.png)

- **Class** adalah **Gambar Rancangan (Blueprint)** di kertas. Di kertas itu tertulis: "Mobil ini punya 4 roda, warna bisa diubah, punya mesin". Kertas ini bukan mobil, tidak bisa dikendarai.
- **Object** adalah **Mobil Asli** yang sudah dirakit di pabrik berdasarkan rancangan tadi. Mobil ini bisa dipegang dan dikendarai.

| Konsep | Analogi                        | Di Java               |
| ------ | ------------------------------ | --------------------- |
| Class  | Kertas Rancangan (Blueprint)   | `class Mobil { ... }` |
| Object | Mobil Fisik (Avanza B 1234 CD) | `new Mobil()`         |

2. **Constructor: "Pesanan Khusus"**

Saat kamu membeli mobil, kamu bilang ke sales: "Saya mau Avanza, warna Putih, tipe G". Permintaan awal saat mobil pertama kali dibuat inilah yang disebut Constructor.

**Ciri-ciri Constructor:**

1. Namanya **SAMA PERSIS** dengan nama Class.
2. Tidak punya return type (tidak ada void, int, dll).
3. Dipanggil otomatis saat ada kata kunci new.

```java
class Mobil {
String warna; // Atribut

        // CONSTRUCTOR
        // Dipanggil saat: new Mobil("Merah");
        Mobil(String warnaInput) {
            this.warna = warnaInput;
        }
}
```

3.  **Methods: "Apa yang Bisa Dilakukan?"**

Mobil yang sudah dibeli bisa ngapain aja?

- Maju?
- Mundur?
- Nyalakan Klakson?

Aksi-aksi ini disebut Method.

Di Java, **method** biasanya menggunakan kata kerja.

> 🚀 **PENTING: Membaca Class Diagram**

Class Diagram adalah jembatan antara soal cerita dengan kode program. Kita harus bisa menerjemahkan simbol menjadi kode.

**Rumus Menerjemahkan Diagram ke Kode**

Perhatikan tabel "Kamus Penerjemah" berikut:

| Simbol di Diagram    | Arti (Visibility) | Kode Java yang Harus Ditulis |
| -------------------- | ----------------- | ---------------------------- |
| +                    | Public            | `public`                     |
| -                    | Private           | `private`                    |
| #                    | Protected         | `protected`                  |
| (tidak ada)          | Default           | _(kosongkan saja)_           |
| Miring / Garis Bawah | Static            | `static`                     |
| HURUF_KAPITAL        | Final             | `final`                      |

<br>

**Latihan Studi Kasus: Employee 👷**

Lihat diagram ini dan perhatikan cara menerjemahkannya ke kode:

| Employee                            |
| ----------------------------------- |
| - name: String                      |
| - payRate: double                   |
| - nextID: int                       |
| ----------                          |
| + getName(): String                 |
| + changeName(newName: String): void |

**Terjemahan ke Kode:**

```java
public class Employee {
// 1. Lihat tanda minus (-) artinya private
// 2. Lihat nama variabel dan tipe datanya
private String name;
private double payRate;

        // 3. Lihat tanda garis bawah (underline) artinya static
        private static int nextID;

        // 4. Lihat tanda plus (+) artinya public
        // 5. Perhatikan tipe data kembalian (String)
        public String getName() {
            return name;
        }

        // 6. Perhatikan parameter (newName: String)
        public void changeName(String newName) {
            this.name = newName;
        }

    }
```

**Cek Pemahaman:**
Jika di soal ada kotak kosong ... String name; dan di diagram tertulis - name: String, apa yang harus kamu isi di titik-titik itu?

**Jawab:** private

---

## Four Pillars of OOP (Part 1)

### 1. **Encapsulation: "Jangan Sentuh Mesin!" 🛡️**

Konsepnya melindungi data sensitif agar tidak diubah sembarangan.

**Cara:**

1. Buat variabel jadi private (Hanya bisa diakses di dalam class itu sendiri).
2. Sediakan jalur resmi lewat Setter (untuk mengubah) dan Getter (untuk melihat).

**Analogi:**
Kamu tidak boleh menyiram bensin langsung ke mesin (Bahaya!). Kamu harus lewat lubang tangki bensin yang sudah disediakan.

**Kode:**

```java
class Mobil {
// DATA DIKUNCI (Private)
private int kecepatan;

        // JALUR RESMI (Setter)
        public void tambahKecepatan(int nilai) {
            // Kita bisa kasih filter/validasi di sini
            if (nilai > 0) {
                kecepatan = kecepatan + nilai;
            } else {
                System.out.println("Tidak bisa nambah kecepatan negatif!");
            }
        }

        // JALUR RESMI (Getter)
        public int getKecepatan() {
            return kecepatan;
        }

}
```

### 2. Inheritance: "Warisan Bapak" 👨‍👦

**Konsep:**

Tidak perlu menulis kode ulang. Class baru (Anak) bisa mewarisi fitur dari Class lama (Induk).

**Keyword:** `extends`.

**Analogi:** Mobil (Induk): Punya Roda, Punya Stir.MobilListrik (Anak): Punya semua yang dimiliki Mobil, TAPI ditambah Baterai.

```java
// 1. Class Induk
class Mobil {

    String merk;
    void jalan() {
        System.out.println("Mobil berjalan ngengg...");
    }
}

// 2. Class Anak (Mewarisi Mobil)
// Otomatis punya 'merk' dan bisa 'jalan()'
class MobilListrik extends Mobil {
int baterai; // Fitur tambahan khusus anak

    void isiDaya() {
        System.out.println("Sedang charge...");
    }

}

public class Main {
    public static void main(String[] args) {
        MobilListrik tesla = new MobilListrik();
        tesla.jalan(); // Bisa dipanggil (Warisan dari Mobil)
        tesla.isiDaya(); // Bisa dipanggil (Milik sendiri)
    }
}
```

### 3. **Polymorphism: "Satu Tombol, Beda Aksi" 🎭**

**Konsep:** Satu nama method yang sama, tapi perilakunya bisa berbeda.

**A. Overloading (Satu Class, Beda Parameter)**

Seperti colokan listrik. Ada yang lubang 2, ada yang lubang 3. Namanya sama-sama "Colokan", tapi bentuk lubangnya (parameter) beda.

```java
void pesanMakan() { ... } // Pesan biasa
void pesanMakan(String menu) { ... } // Pesan spesifik
void pesanMakan(String menu, int jumlah) { ... } // Pesan lengkap
```

**B. Overriding (Beda Class, Beda Rasa)**

Ini terjadi pada hubungan Inheritance. Anak mengubah perilaku yang diwariskan Bapak.

**Contoh:**

- Mobil (Induk): Kalau klakson bunyinya "Tin Tin".
- Bus (Anak): Mewarisi Mobil, tapi klaksonnya diubah jadi "Telolet Telolet".

```java
class Mobil {
    void bunyikanKlakson() {
        System.out.println("Tin Tin!");
    }
}

class Bus extends Mobil {
    // Menimpa (Override) perilaku induk
    @Override
    void bunyikanKlakson() {
        System.out.println("Telolet Telolet!");
    }
}
```

## Rangkuman Pertemuan 1

1. Class itu cetakan, Object itu barang jadinya.
2. Constructor itu method khusus untuk inisialisasi awal (namanya sama dgn Class).
3. Diagram - artinya private, + artinya public.
4. Encapsulation = Variabel Private + Getter/Setter.
5. Inheritance = extends.
6. Polymorphism = Overloading (Beda param) & Overriding (Ganti isi method induk).Sampai jumpa di pertemuan selanjutnya! 👋

## Strategi Mengajar Tambahan (Saran Praktis)

Selain materi di atas, lakukan ini di kelas untuk memenuhi objektif Anda:

1.  **Latihan Diagram "Terbalik":**
    - Berikan kode Java yang sudah jadi (sederhana saja, misal class `Kucing`).
    - Minta mahasiswa menggambar Class Diagram-nya di kertas.
    - _Tujuannya:_ Melatih logika dua arah (Diagram ke Kode, dan Kode ke Diagram).

2.  **Fill-in-the-Blank Live Coding:**
    - Siapkan file `.java` yang kode intinya dihapus.
    - Contoh soal:

      ```java
      class Mahasiswa {
          // SOAL 1: Buat atribut private nama bertipe String sesuai diagram!
          _________________________;

          // SOAL 2: Buat setter untuk nama!
          public void setNama(_______ inputNama) {
              this.nama = _______;
          }
      }
      ```
