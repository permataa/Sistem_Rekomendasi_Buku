# Laporan Proyek Machine Learning - Permata Ayu

## Project Overview

Dalam era informasi saat ini, volume buku yang tersedia secara daring sangatlah besar. Pembaca membutuhkan sistem cerdas yang dapat membantu mereka menemukan buku yang sesuai dengan preferensi mereka. Oleh karena itu, proyek ini bertujuan membangun sistem rekomendasi buku berbasis konten (Content-Based Filtering) menggunakan dataset dari Goodreads.

Dataset ini dikembangkan melalui API Goodreads oleh pengguna Kaggle karena kelangkaan dataset buku yang bersih, terstruktur, dan kaya informasi numerik seperti jumlah ulasan, rating, serta penerbit. Proyek ini mengombinasikan analisis fitur seperti nama penulis, penerbit, dan bahasa untuk menghasilkan rekomendasi.

Referensi:
- [Goodreads Books Dataset - JealousLeopard (Kaggle)](https://www.kaggle.com/datasets/jealousleopard/goodreadsbooks)
- Aggarwal, C. C. (2016). *Recommender Systems: The Textbook*. Springer.
- Adomavicius, G., & Tuzhilin, A. (2005). *Toward the next generation of recommender systems: A survey of the state-of-the-art and possible extensions*. IEEE Transactions on Knowledge and Data Engineering. [DOI](https://doi.org/10.1109/TKDE.2005.99)

---

## Business Understanding

### Problem Statements
- Bagaimana memberikan rekomendasi buku berdasarkan kesamaan karakteristik buku?
- Bagaimana meningkatkan pengalaman pengguna dalam menemukan buku yang relevan tanpa harus mencari secara manual?

### Goals
- Mengembangkan sistem rekomendasi berbasis konten yang dapat memberikan saran buku serupa berdasarkan input pengguna.
- Memberikan hasil rekomendasi yang relevan berdasarkan kesamaan penulis, penerbit, dan bahasa.

### Solution Statements
- Menggunakan pendekatan Content-Based Filtering dengan TF-IDF Vectorizer.
- Mengukur kemiripan antar buku dengan cosine similarity berdasarkan metadata yang digabungkan.

---

## Data Understanding

Dataset yang digunakan berasal dari Kaggle, terdiri dari lebih dari 10.000 buku. Dataset ini merupakan hasil scraping API Goodreads.

[Dataset Link](https://www.kaggle.com/datasets/jealousleopard/goodreadsbooks)

### Fitur-fitur:
- BookID: Identifier unik untuk setiap buku (tipe data: integer).
- Title: Judul buku (tipe data: string/object).
- Authors: Nama penulis buku (tipe data: string/object).
- Average_rating: Rata-rata rating yang diberikan oleh pengguna (tipe data: float).
- Isbn: Nomor ISBN buku (tipe data: string/object).
- Isbn13: Nomor ISBN 13-digit buku (tipe data: integer).
- Language_code: Kode bahasa yang digunakan dalam buku (tipe data: string/object).
- Num_pages: Jumlah halaman buku (tipe data: integer).
- Ratings_count: Jumlah total rating yang diberikan oleh pengguna (tipe data: integer).
- Text_reviews_count: Jumlah ulasan teks yang diberikan oleh pengguna (tipe data: integer).
- Publication_date: Tanggal publikasi buku (tipe data: string/object).
- Publisher: Nama penerbit buku (tipe data: string/object).

### Exploratory Data Analysis (EDA)
- Distribusi nilai `average_rating`
- Top 10 penulis dengan jumlah buku terbanyak
- Korelasi antar fitur numerik dengan heatmap

---

## Data Preparation

Langkah-langkah preprocessing:
- Mengisi nilai kosong pada kolom `authors` dan `publisher` dengan label "Unknown".
- Membuat fitur gabungan `content_features` dari `authors`, `publisher`, dan `language_code`.
- Menerapkan TF-IDF Vectorizer (`ngram_range=(1,2)`, `stop_words='english'`).
- Menghasilkan matriks kemiripan menggunakan cosine similarity dari TF-IDF.

---

## Modeling

Model rekomendasi dibangun menggunakan Content-Based Filtering:
- Sistem menerima input berupa judul buku.
- Sistem mencari indeks buku terkait dan menghitung similarity terhadap buku lain.
- Output berupa top-N buku serupa berdasarkan skor kemiripan tertinggi.
- Hasil difilter menggunakan threshold minimal `average_rating` dan duplikat dihapus.

### Cara Kerja Model
1. Pengguna memasukkan judul buku yang diinginkan.
2. Sistem mencari indeks buku tersebut di dataset.
3. Menghitung skor kemiripan (cosine similarity) antara buku input dengan semua buku lainnya.
4. Hanya buku dengan:
   - Skor similarity tertinggi
   - Rata-rata rating minimal 4.0
   - Tidak duplikat
   yang direkomendasikan ke pengguna.
5. Sistem menampilkan top-N rekomendasi buku mirip dan berkualitas.

6. Evaluasi dilakukan menggunakan presisi sederhana:
- Input: `Harry Potter`
- Output: 5 buku rekomendasi
- Dari 5, sebanyak 4 buku dianggap relevan
- Presisi = 4/5 = 80%

> Metrik presisi sederhana cukup merepresentasikan kualitas sistem rekomendasi ini dalam konteks user-facing interface.


#### Contoh Output Rekomendasi (Input: *Harry Potter*)

| Judul Buku                                                                 | Penulis       | Penerbit        | Rating | Similarity |
|----------------------------------------------------------------------------|---------------|------------------|--------|------------|
| Harry Potter and the Order of the Phoenix (Harry Potter #5)               | J.K. Rowling  | Scholastic Inc. | 4.5    | 0.88       |
| Harry Potter and the Prisoner of Azkaban (Harry Potter #3)                | J.K. Rowling  | Scholastic Inc. | 4.6    | 0.86       |
| Harry Potter and the Chamber of Secrets (Harry Potter #2)                 | J.K. Rowling  | Scholastic Inc. | 4.4    | 0.85       |
| Harry Potter and the Sorcerer's Stone (Harry Potter #1)                   | J.K. Rowling  | Scholastic Inc. | 4.7    | 0.84       |
| [Fallback Buku Populer] (jika input tidak ditemukan)                      | -             | -               | ≥4.0   | -          |


Preview visualisasi:
![Screenshot (1131)](https://github.com/user-attachments/assets/8b5e93f7-d3f3-49a7-8700-f84a5af16a85)
---

## Evaluation
### Keterkaitan dengan Business Understanding

Sistem rekomendasi ini dibangun untuk menjawab kebutuhan berikut:
- Membantu pengguna menemukan buku yang relevan dan berkualitas berdasarkan kesukaan mereka.
- Meningkatkan pengalaman pengguna dalam platform penyedia buku digital.
- Meningkatkan waktu interaksi dan potensi pembelian pengguna terhadap buku-buku yang disarankan.

### Evaluasi Model
- **Presisi Manual**: 80% (4 dari 5 hasil rekomendasi dianggap relevan oleh pengguna).
- **Problem Statement**:
  - Apakah pengguna kesulitan menemukan buku serupa dari buku favorit? → Ya.
  - Apakah sistem mampu memberikan rekomendasi berkualitas tinggi? → Ya, karena hasil disaring berdasarkan rating ≥ 4.0.
- **Goal Tercapai**:
  - Pengguna mendapatkan buku mirip secara isi dan popularitas.
  - Model tetap memberikan hasil meskipun input pengguna tidak eksak (melalui fallback logic).
- **Dampak Solusi**:
  - Membantu pengguna menemukan buku yang sesuai dengan selera mereka, meningkatkan retensi dan kepuasan pengguna terhadap layanan.
---

## Deployment

Sistem ini dijalankan menggunakan Gradio:

1. Install Gradio:
```bash
pip install gradio
```

2. Jalankan UI:
```python
iface.launch(share=True)
```

3. Antarmuka akan muncul dan dapat diakses melalui link publik.

---

## Preview Gradio UI  
Antarmuka berikut dibangun menggunakan Gradio dan berjalan secara interaktif:

![Screenshot (1071)](https://github.com/user-attachments/assets/5a96e31d-5487-4ecc-8ade-b79af7fe1ea3)

> Pengguna cukup memasukkan judul buku favorit, lalu sistem akan merekomendasikan buku-buku serupa berdasarkan metadata.


## Penutup

Proyek ini berhasil membangun sistem rekomendasi buku sederhana namun efektif berdasarkan metadata buku. Dengan menggunakan teknik *content-based filtering*, sistem mampu memberikan rekomendasi yang relevan dan dapat diakses melalui antarmuka interaktif.

---

## Penulis

Proyek ini dikerjakan oleh Permata Ayu sebagai submission Proyek Akhir Machine Learning Terapan - Dicoding Academy. Terima kasih telah membaca 
