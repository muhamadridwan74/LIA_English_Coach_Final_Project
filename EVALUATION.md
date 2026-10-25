# Hasil evaluasi

## Sudah dijalankan
- 5 unit tests backend: lulus. Panggilan HTTP Gemini pada unit test menggunakan mock.
- Uji UI browser: halaman desktop, percakapan demo, pesan error key kosong, input untuk retry, reset, tampilan mobile tanpa overflow horizontal, serta tidak ada JavaScript error: lulus.
- Tidak ada API key yang disertakan dalam proyek.

## Perlu dijalankan dengan API key pengguna
| Skenario | Kriteria | Hasil live |
|---|---|---|
| Grammar correction | Mengoreksi go menjadi went | Belum diuji |
| Memory aktif | Mengingat pekerjaan dari pesan sebelumnya | Belum diuji |
| Memory mati | Tidak menerima konteks pesan sebelumnya | Belum diuji |
| Role-play | Mengambil satu peran dan menunggu respons | Belum diuji |
| Level dan gaya | Respons mengikuti pilihan pengguna | Belum diuji |
| Bahasa penjelasan | Indonesia/English sesuai pengaturan | Belum diuji |
| Variasi temperature | Catatan perbandingan beberapa respons | Belum diuji |

## Keterbatasan
- Tidak menilai suara, pronunciation, atau memberikan sertifikasi CEFR.
- Respons model dapat salah; koreksi penting perlu dicek kembali.
- Memory terbatas, tidak persisten, dan bukan RAG atau fine-tuning.
- Demo hanya tiga contoh terprogram; konfigurasi AI tidak mengubah respons demo.
- Model dan kuota tergantung akun/API yang digunakan.
- Belum ada autentikasi dan pembatasan trafik untuk hosting publik.
