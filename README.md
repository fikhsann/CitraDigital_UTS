# Pengolahan Citra Digital: Segmentasi Lanjut

Tujuan utama segmentasi adalah menyederhanakan representasi citra agar lebih mudah dianalisis, biasanya dengan memisahkan **Objek (Foreground)** dari **Latar Belakang (Background)**.

Materi ini berfokus pada dua pendekatan utama:

## 1. Segmentasi Berbasis Tepi (Edge-Based)

Pendekatan ini bekerja dengan cara menemukan batas-batas objek.

- **Konsep Utama:** Mencari perubahan intensitas yang tajam dan tiba-tiba (disebut **gradient**) antar piksel. Garis-garis perubahan yang tajam ini dianggap sebagai "tepi" atau batas objek.
- **Tujuan:** Menghasilkan "garis luar" atau kontur dari objek.
- **Algoritma Populer:** Operator Sobel , Canny Edge Detector.

## 2. Segmentasi Berbasis Region (Region-Based)

Pendekatan ini bekerja dengan cara mengelompokkan piksel-piksel yang mirip menjadi satu wilayah (region).

- **Konsep Utama:** Bekerja berdasarkan prinsip **kesamaan**. Piksel-piksel yang berdekatan dan memiliki properti serupa (misalnya, intensitas, warna, atau tekstur yang sama) akan digabungkan menjadi satu segmen.
- **Tujuan:** Menghasilkan area objek yang utuh dan seragam (homogen).
- **Algoritma Populer:** Region Growing , Watershed Algorithm.

# Penjelasan Kode Segmentasi (Edge & Region)

Repositori ini berisi skrip Python yang mendemonstrasikan implementasi dari dua teknik segmentasi citra: **Segmentasi Berbasis Tepi (Canny)** dan **Segmentasi Berbasis Region (Region Growing)**.

### Deskripsi

Skrip ini tidak memuat citra dari file, melainkan membuat citra _dummy_ (buatan) sederhana menggunakan NumPy. Citra ini kemudian diproses menggunakan dua metode segmentasi yang berbeda untuk membandingkan hasilnya secara visual.

1.  **Segmentasi Berbasis Tepi (`custom_canny_segmentation`)**: Menggunakan algoritma Canny untuk mendeteksi batas-batas objek berdasarkan perubahan intensitas yang tajam.
2.  **Segmentasi Berbasis Region (`custom_region_growing`)**: Menggunakan algoritma Region Growing sederhana untuk mengelompokkan piksel berdasarkan kesamaan intensitas, dimulai dari satu titik awal (_seed_).

### Alur Kerja Skrip

Skrip ini dibagi menjadi beberapa bagian utama:

1.  **`create_dummy_image`**:

    - Membuat sebuah "kanvas" hitam (`np.zeros`).
    - Menggambar dua objek persegi dengan intensitas berbeda (nilai `180` dan `120`) untuk simulasi objek.
    - Menambahkan _noise_ Gaussian acak (`np.random.normal`) untuk membuat citra lebih realistis dan menantang.

2.  **`custom_canny_segmentation`**:

    - **Tahap 1 (Noise Reduction):** Menerapkan `cv2.GaussianBlur` untuk menghaluskan citra dan menghilangkan _noise_ yang dapat mengganggu deteksi tepi.
    - **Tahap 2-4 (Canny):** Memanggil fungsi `cv2.Canny` yang secara otomatis menjalankan perhitungan **Gradient**, **Non-maximum Suppression**, dan **Hysteresis Thresholding** untuk menghasilkan gambar tepi yang bersih dan tipis.

3.  **`custom_region_growing`**:

    - **Inisiasi:** Mempersiapkan sebuah `set` (untuk melacak piksel yang sudah dikunjungi) dan sebuah `queue` (untuk piksel yang akan diperiksa).
    - **Proses Tumbuh:** Memulai dari `seed_point`, algoritma memeriksa piksel tetangga.
    - **Kriteria:** Jika intensitas piksel tetangga "cukup mirip" (perbedaannya dengan intensitas _seed_ lebih kecil dari `threshold`), piksel tersebut ditambahkan ke _region_ dan antrian.
    - **Hasil:** Proses ini berulang hingga antrian kosong, menghasilkan satu _region_ utuh yang terhubung.

4.  **Visualisasi Hasil**:
    - Menggunakan `matplotlib.pyplot` untuk membuat tampilan _subplot_ 1x3.
    - Menampilkan "Citra Asli", hasil "Edge-Based (Canny)", dan hasil "Region-Based (Region Growing)" secara berdampingan untuk perbandingan.

### Persyaratan

- Python 3.x
- OpenCV (`opencv-python`)
- NumPy (`numpy`)
- Matplotlib (`matplotlib`)

### Instalasi

Anda dapat menginstal semua dependensi yang diperlukan menggunakan pip:

```bash
pip install opencv-python numpy matplotlib
```
