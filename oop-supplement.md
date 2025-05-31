# OOP: Supplement

composed by [_Bimo Ade Budiman Fikri_](https://www.linkedin.com/in/bimoadee/)

![alt text](4.png)

<!-- [TOC] -->

## **Table of Contents**

<!-- - [Overview](#overview)
- [Graphical User Interface (GUI)](#graphical-user-interface-gui)
- [Java Database Connectivity (JDBC)](#java-database-connectivity-jdbc)
- [Membangun Aplikasi GUI di Swing](#membangun-aplikasi-gui-di-swing)
  - [Step 1: Buat Frame](#step-1-buat-frame)
  - [Step 2: Tambahkan Control](#step-2-tambahkan-control)
  - [Step 3: Tentukan Layout](#step-3-tentukan-layout)
  - [Step 4: Pasang Event Handling](#step-4-pasang-event-handling)
  - [Step 5: Buat Class Mahasiswa](#step-5-buat-class-mahasiswa)
  - [Step 6: Pasang Event Handling pada Form](#step-6-pasang-event-handling)
  - [Step 7: Jalankan di EDT](#step-7-jalankan-di-edt)
- [Membangun Koneksi ke Database](#membangun-koneksi-ke-database)
  - [Apa itu JDBC?](#apa-itu-jdbc)
  - [Komponen Utama JDBC](#komponen-utama-jdbc)
  - [Alur Kerja JDBC](#alur-kerja-jdbc)
  - [Step-by-Step Koneksi dan Menyimpan Data Mahasiswa](#step-by-step-koneksi-dan-menyimpan-data-mahasiswa)
    - [Step 1: Persiapkan Database](#step-1-persiapkan-database)
    - [Step 2: Tambahkan JDBC Driver ke Project](#step-2-tambahkan-jdbc-driver-ke-project)
    - [Step 3: Buat Kelas Koneksi Database](#step-3-buat-kelas-koneksi-database)
    - [Step 4: Modifikasi Form Mahasiswa untuk Simpan ke Database](#step-4-modifikasi-form-mahasiswa-untuk-simpan-ke-database)
    - [Step 5: Jalankan Program](#step-5-jalankan-program) -->

---

```mermaid
classDiagram

%% === ABSTRAKSI ===
class Account {
  <<abstract>>
  - id: String
  - name: String
  - address: String
  - email: String
  - phone: String
  - password: String
  + searchBook(criteria: SearchCriteria): List~BookItem~
}

class Payment {
  <<interface>>
  + pay(amount: double): void
}

class NotificationSender {
  <<interface>>
  + send(msg: String, to: String): void
}

%% === IMPLEMENTASI / TURUNAN ===
Account <|-- Member
Account <|-- Librarian
Payment <|.. CreditCardPayment
Payment <|.. CashPayment
Payment <|.. BankTransferPayment
NotificationSender <|.. EmailNotification

%% === CLASS UTAMA ===
class Member {
  - card: LibraryCard
  + checkoutBook(item: BookItem): Loan
  + returnBook(item: BookItem): void
  + pay(payment: Payment): void
  + viewLoanHistory(): List~Loan~
}

class Librarian {
  + addBookItem(item: BookItem): void
  + updateBookItem(item: BookItem): void
  + blockMember(member: Member): void
  + unblockMember(member: Member): void
}

class Book {
  - ISBN: String
  - title: String
  - subject: String
  - publisher: String
  - pages: int
}

class BookItem {
  - barcode: String
  - format: FormatType
  - status: Status
  - price: double
}

class Shelf {
  - shelfNo: String
  - location: String
}

class Loan {
  - borrowDate: Date
  - dueDate: Date
  - returnDate: Date
  - fee: double
}

class LibraryCard {
  - cardNumber: String
  - status: Status
  - issuedDate: Date
}

%% === RELASI ASOSIASI ===
Member --> LibraryCard : has
Member --> Loan : creates
Loan --> BookItem : borrows
Loan --> Member : belongs to
BookItem --> Book : refers to
BookItem --> Shelf : located in
Librarian --> BookItem : manages
Loan --> Payment : uses
Member --> NotificationSender : notifies
```

---

# The End

```
Have a nice day 👋
```
