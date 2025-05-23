## Sistem-Bantuan-Pasien

Sistem bantuan pasien ini dirancang untuk digunakan di lingkungan rumah sakit atau panti perawatan, di mana pasien dapat dengan mudah meminta bantuan dari perawat dengan hanya menekan sebuah tombol. Sistem ini terdiri dari dua tombol untuk dua pasien, satu tombol untuk perawat, dua lampu indikator (LED), sebuah buzzer (alarm), dan layar LCD untuk menampilkan informasi secara langsung.
Terima kasih atas klarifikasinya! Berikut penjelasan lengkap mengenai **cara kerja sistem bantuan pasien** berdasarkan kode Arduino yang sudah kamu berikan:

---

## Tujuan Sistem

Sistem ini dibuat untuk:

* **Memantau tombol dari 2 pasien** (pasien 1 dan pasien 2).
* **Menyalakan LED dan buzzer** saat ada permintaan bantuan.
* **Menampilkan informasi pada LCD I2C** tentang siapa yang minta bantuan dan berapa kali.
* **Membiarkan perawat merespon** dengan tombol untuk mematikan LED dan buzzer.

---

## Komponen & Fungsi

| Komponen        | Fungsi                                                        |
| --------------- | ------------------------------------------------------------- |
| LCD 16x2 I2C    | Menampilkan pesan bantuan dan jumlah panggilan                |
| Buzzer          | Mengeluarkan bunyi saat ada panggilan                         |
| LED (2 buah)    | Memberi sinyal visual untuk tiap pasien                       |
| Tombol pasien 1 | Pasien 1 menekan saat butuh bantuan                           |
| Tombol pasien 2 | Pasien 2 menekan saat butuh bantuan                           |
| Tombol perawat  | Untuk mematikan buzzer dan LED, serta menandai sudah merespon |

---

## Alur Kerja Sistem

1. **Inisialisasi (setup)**

   * LCD menyala dan menampilkan "Sistem Bantuan" selama 2 detik.
   * Semua pin (tombol dan output) dikonfigurasi.
   * LED dan buzzer dimatikan.

2. **Loop utama (loop)**
   Sistem terus memantau input dari tombol pasien dan tombol perawat.

---

### Jika Pasien 1 atau 2 Menekan Tombol:

* Sistem mengecek apakah tombol ditekan **(LOW)** dan apakah telah melewati waktu debounce (300ms).
* Jika ya:

  * Jumlah panggilan untuk pasien itu ditambah 1 (`patient1CallCount` atau `patient2CallCount`).
  * LCD akan menampilkan:

    ```
    Pasien 01: Minta bantuan
    Panggilan ke-<jumlah>
    ```
  * LED pasien menyala.
  * Buzzer menyala.
  * Status `nurseResponded` di-set ke `false` (belum ditangani).

---

### Jika Perawat Menekan Tombol:

* Dicek apakah tombol perawat ditekan dan `nurseResponded == false`.
* Jika ya:

  * Buzzer dimatikan.
  * Semua LED dimatikan.
  * **Jumlah panggilan di-reset ke 0**.
  * Status `nurseResponded` di-set ke `true`.
  * LCD menampilkan:

    ```
    Perawat merespon
    ```


