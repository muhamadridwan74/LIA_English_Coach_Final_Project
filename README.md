# LIA English Coach — Final Project

Chatbot latihan bahasa Inggris menggunakan Gemini API, Python, HTML, CSS, dan JavaScript. Prototipe tugas mandiri, bukan layanan resmi LIA.

## Use case dan tujuan
Pelajar sering membutuhkan partner latihan di luar kelas. Chatbot ini memberikan latihan percakapan, koreksi grammar, dan role-play yang disesuaikan dengan level pengguna. Domain dibatasi pada pembelajaran bahasa Inggris.

## Fitur dan parameter kreatif
| Fitur | Pengaturan | Tujuan |
|---|---|---|
| Mode | Conversation / Grammar clinic / Role-play | Bentuk interaksi sesuai tujuan latihan |
| Level | A1, A2, B1, B2, C1 | Mengarahkan kompleksitas bahasa; bukan hasil tes level |
| Gaya | Santai / Formal | Menyesuaikan nada respons |
| Bahasa penjelasan | Indonesia / English | Mendukung pemahaman pengguna |
| Temperature | 0–2; default 0.7 | Mengatur variasi sampling; bukan jaminan akurasi |
| maxOutputTokens | 512–4096; default 1024 | Membatasi panjang keluaran model |
| Memory | Aktif/nonaktif; 6 pasang pesan terakhir | Membantu pertanyaan lanjutan tetap kontekstual |
| Ekspor | JSON | Mengunduh percakapan untuk refleksi belajar |

Level, gaya, dan mode diterjemahkan menjadi system instruction. Temperature dan maxOutputTokens dikirim sebagai generationConfig. Model menerima history sebagai contents dengan role user/model. Model yang mendukung reasoning dapat menggunakan sebagian anggaran output untuk reasoning; naikkan batas token jika respons terpotong.

## Menjalankan (Python 3.10+)
Tidak memerlukan pip install: server memakai standard library Python.

1. Ekstrak ZIP atau clone repositori, lalu masuk folder proyek.
2. Salin `.env.example` menjadi `.env`.
3. Isi `GEMINI_API_KEY` menggunakan key dari https://aistudio.google.com/apikey. Jangan membagikan key di chat atau memasukkannya ke screenshot/GitHub.
4. Sesuaikan `GEMINI_MODEL` dengan model yang tersedia di akun Anda. Default konfigurasi: `gemini-2.5-flash`; ketersediaannya perlu diuji pada akun Anda. Jika mendapat 404, gunakan ID model generateContent yang tersedia.
5. Jalankan:

```bash
python app.py
```

6. Buka http://127.0.0.1:8000 di browser.
7. Matikan **Mode demo tanpa API** untuk menggunakan Gemini. Restart server setiap mengubah `.env`.

Di Windows, gunakan `py app.py` bila perintah `python` tidak tersedia. Untuk mencoba UI tanpa key, jalankan langkah 5–6 dan biarkan demo aktif.

## Arsitektur
Browser → POST /api/chat → validasi input + system instruction + memory → Gemini generateContent → jawaban teks → browser.

API key hanya dibaca di server dari environment / `.env`, tidak dikirim ke browser. Server hanya melayani tiga berkas publik yang diizinkan. Tampilan pesan menggunakan textContent sehingga teks model tidak dieksekusi sebagai HTML. Server lokal hanya bind 127.0.0.1. Ini bukan server produksi; hosting publik memerlukan autentikasi, pembatasan permintaan, HTTPS, dan server produksi.

Memory berada di RAM tab browser, hilang saat refresh atau menekan Percakapan baru. Mode memory nonaktif tidak mengirim history ke Gemini, tetapi riwayat masih terlihat di layar. Pergantian demo/AI mereset percakapan untuk mencegah contoh demo masuk sebagai konteks AI. Ekspor memuat maksimal 50 pasang pesan terakhir. Pesan mode AI dikirim ke Google; jangan memasukkan data sensitif.

## Screenshot
- `screenshots/01-home-desktop.png`: halaman awal.
- `screenshots/02-grammar-demo.png`: interaksi grammar menggunakan respons demo terprogram.
- `screenshots/03-mobile.png`: tampilan pada layar kecil.

**Screenshot demo bukan bukti panggilan Gemini berhasil.** Untuk final submission, tambahkan screenshot interaksi nyata setelah API key dipasang dan demo dimatikan. Tidak ada respons contoh yang diklaim sebagai keluaran Gemini.

## Pengujian
```bash
python -m unittest discover -s tests -v
```

Pengujian mencakup payload parameter/history, input tidak valid, key kosong, header key dan parsing respons (mock HTTP), serta penanganan kuota 429. Mock bukan pengujian live Gemini.

### Uji penerimaan live (perlu API key)
1. Grammar clinic: `Yesterday I go to the office. Is this correct?` → harapkan koreksi went dan alasan past tense.
2. Conversation: `My name is Dina. I work as a teacher.` lalu `What is my job?` → cek konteks saat memory aktif. Ulangi di percakapan baru dengan memory mati; model tidak boleh mengklaim mengetahui pekerjaan.
3. Role-play: `Let's practice a job interview.` → bot menjadi pewawancara dan menunggu jawaban.
4. Bandingkan pertanyaan sama pada level A1 dan B2; cek perbedaan kompleksitas. Bandingkan gaya Formal/Santai dan bahasa penjelasan.
5. Ubah temperature 0.2 dan 1.2 lalu ulangi prompt beberapa kali. Catat variasi, jangan menyimpulkan dari satu respons.
6. Catat hasil nyata dalam `EVALUATION.md` tanpa API key.

## Struktur
- `app.py`: backend lokal, validasi, prompt, panggilan REST Gemini.
- `static/`: UI responsif.
- `tests/`: unit tests.
- `screenshots/`: screenshot aplikasi berjalan.
- `.env.example`: contoh konfigurasi tanpa secret.
- `EVALUATION.md`: lembar uji dan keterbatasan.

## Menyiapkan deliverable GitHub
Buat repositori kosong bernama `lia-english-coach` pada https://github.com/new. Gunakan Public jika penilai perlu mengakses tanpa undangan. Dari folder proyek:

```bash
git init
git add .
git commit -m "Add Gemini English coach final project"
git branch -M main
git remote add origin https://github.com/USERNAME/lia-english-coach.git
git push -u origin main
```

Ganti USERNAME dengan akun Anda. `.env` diabaikan melalui `.gitignore`; periksa `git status` sebelum commit. Jika mengunggah melalui website GitHub, pilih berkas proyek dan jangan pilih `.env`. URL repositori baru menjadi deliverable setelah repo benar-benar dibuat dan push berhasil. GitHub Pages saja tidak dapat menjalankan server Python proyek ini.

## Ringkasan presentasi
“LIA English Coach adalah chatbot edukasi untuk latihan bahasa Inggris. Aplikasi memanfaatkan Gemini API untuk memahami pesan natural language dan menghasilkan respons sesuai level, mode belajar, dan gaya bahasa. Keunikannya adalah dukungan penjelasan bilingual, grammar feedback, role-play, dan memory percakapan terbatas. Konfigurasi temperature serta output token dapat diubah melalui UI. API key disimpan di backend. Evaluasi dilakukan melalui unit test dan skenario uji live.”

## Referensi
- https://ai.google.dev/api/generate-content
- https://ai.google.dev/gemini-api/docs/api-key
- https://ai.google.dev/gemini-api/docs/models
