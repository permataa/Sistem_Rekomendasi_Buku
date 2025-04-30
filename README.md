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
- `title`: Judul buku
- `authors`: Nama penulis
- `average_rating`: Rata-rata rating pengguna
- `ratings_count`: Jumlah rating
- `text_reviews_count`: Jumlah ulasan teks
- `publisher`: Nama penerbit
- `language_code`: Kode bahasa

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

---

## Evaluation

Evaluasi dilakukan menggunakan presisi sederhana:

- Input: `Harry Potter`
- Output: 5 buku rekomendasi
- Dari 5, sebanyak 4 buku dianggap relevan
- Presisi = 4/5 = 80%

Visualisasi hasil evaluasi disajikan dalam bentuk pie chart di notebook.

> Metrik presisi sederhana cukup merepresentasikan kualitas sistem rekomendasi ini dalam konteks user-facing interface.

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

![Screenshot (1071)](https://github.com/user-attachments/assets/2ca9d35d-5048-4cf1-8411-7fbde001f240)

> Pengguna cukup memasukkan judul buku favorit, lalu sistem akan merekomendasikan buku-buku serupa berdasarkan metadata.


## Penutup

Proyek ini berhasil membangun sistem rekomendasi buku sederhana namun efektif berdasarkan metadata buku. Dengan menggunakan teknik *content-based filtering*, sistem mampu memberikan rekomendasi yang relevan dan dapat diakses melalui antarmuka interaktif.

---

## Penulis

Proyek ini dikerjakan oleh Permata Ayu sebagai submission Proyek Akhir Machine Learning - Dicoding Academy. Terima kasih telah membaca 
