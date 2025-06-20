# Laporan Praktikum: Uji Keamanan Server HTTPS

- **GitHub:** eakmat303

---

## 1. Tujuan Proyek

Tujuan proyek ini adalah untuk melakukan simulasi pengujian keamanan (penetration testing) pada sebuah server HTTPS. Pengujian ini tidak dilakukan pada situs web asli, melainkan pada server lokal yang sengaja dikonfigurasi dengan kelemahan untuk tujuan pembelajaran.

## 2. Alat yang Digunakan

- **Kali Linux:** Sebagai sistem operasi utama.
- **Docker:** Untuk membuat dan menjalankan lingkungan server uji yang terisolasi.
- **Nginx:** Sebagai perangkat lunak web server.
- **testssl.sh:** Sebagai alat utama untuk menganalisis konfigurasi SSL/TLS server.

## 3. Langkah-langkah Pengujian

1.  **Membangun Server:** Sebuah server Nginx dengan konfigurasi SSL/TLS yang lemah dibangun menggunakan Docker. Konfigurasi sengaja mengizinkan protokol lama (`TLSv1`, `TLSv1.1`) dan enkripsi lemah (`RC4`). File konfigurasi ada di direktori `lab-setup`.
2.  **Menjalankan Server:** Container Docker dijalankan di `localhost` (127.0.0.1) pada port 443.
3.  **Melakukan Scan:** Alat `testssl.sh` digunakan untuk memindai server. Hasil pemindaian lengkap disimpan dalam file di direktori `hasil-scan`.

## 4. Hasil Temuan

Berdasarkan pemindaian menggunakan `testssl.sh`, ditemukan beberapa kelemahan konfigurasi kritis:

-   **Ditemukan! Protokol Lama Aktif:** Server ditemukan mendukung `TLS 1.0` dan `TLS 1.1`. Keduanya adalah protokol kuno yang rentan diserang (seperti POODLE, BEAST).
-   **Ditemukan! Enkripsi Lemah Aktif:** Server ditemukan mendukung cipher suite `RC4-SHA` yang sudah tidak aman dan bisa dipecahkan secara kriptografi.
-   **Ditemukan! Sertifikat Self-Signed:** Server menggunakan sertifikat yang tidak dipercaya oleh Otoritas Sertifikat (CA), sehingga rentan terhadap serangan Man-in-the-Middle.

## 5. Kesimpulan

Proyek ini berhasil membuktikan bahwa sebuah website yang menggunakan HTTPS (memiliki "gembok") belum tentu aman. Konfigurasi yang salah pada level server dapat membuka celah keamanan yang serius.
