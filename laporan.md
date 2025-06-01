# Laporan Proyek Machine Learning - Yebi Depriansyah

## Project Overview

Dalam era digital saat ini, industri game telah berkembang pesat dan menawarkan ribuan judul game dari berbagai genre dan platform. Akibatnya, banyak pengguna merasa kesulitan untuk menemukan game yang sesuai dengan preferensi mereka. Untuk menjawab permasalahan tersebut, diperlukan sistem rekomendasi yang mampu menyaring dan menyarankan game-game relevan secara personal. Sistem rekomendasi dapat membantu pengguna menemukan game baru yang sesuai dengan minat dan gaya bermain mereka, sekaligus meningkatkan keterlibatan pengguna dan potensi keuntungan bagi platform penyedia game.

Proyek ini bertujuan untuk membangun **Sistem Rekomendasi Game** menggunakan dua pendekatan populer, yaitu **Content-Based Filtering** dan **Collaborative Filtering**. Pendekatan content-based akan merekomendasikan game berdasarkan kemiripan konten dari game yang pernah dimainkan atau disukai pengguna, seperti genre, rating, dan fitur lainnya. Sementara itu, collaborative filtering akan merekomendasikan game berdasarkan perilaku pengguna lain yang memiliki pola kesukaan serupa.

Dengan mengombinasikan kedua pendekatan ini, sistem diharapkan mampu memberikan hasil rekomendasi yang lebih akurat dan memuaskan bagi pengguna.

### Referensi:
- Ricci, F., Rokach, L., & Shapira, B. (2015). *Recommender Systems Handbook*. Springer. DOI: [10.1007/978-1-4899-7637-6](https://doi.org/10.1007/978-1-4899-7637-6)
- Aggarwal, C. C. (2016). *Recommender Systems: The Textbook*. Springer. DOI: [10.1007/978-3-319-29659-3](https://doi.org/10.1007/978-3-319-29659-3)
- Gómez-Uribe, C. A., & Hunt, N. (2015). The Netflix Recommender System: Algorithms, Business Value, and Innovation. *ACM Transactions on Management Information Systems (TMIS)*, 6(4), 1–19. DOI: [10.1145/2843948](https://doi.org/10.1145/2843948)


## Business Understanding

Platform distribusi game digital seperti Steam memiliki ribuan game dari berbagai genre dan kategori. Hal ini membuat pengguna seringkali mengalami kebingungan dalam memilih game yang sesuai dengan preferensi mereka. Banyaknya pilihan menyebabkan pengguna menghabiskan waktu lebih lama dalam pencarian, dan seringkali mendapat rekomendasi yang tidak relevan atau kurang personal. Oleh karena itu, diperlukan sebuah sistem rekomendasi yang dapat membantu pengguna menemukan game yang sesuai secara otomatis dan efisien.

### Problem Statements

- Pengguna mengalami kesulitan dalam menemukan game yang sesuai dengan preferensi mereka karena terlalu banyak pilihan.
- Rekomendasi game yang ditampilkan oleh platform tidak selalu relevan atau sesuai dengan selera pengguna.
- Tidak adanya sistem rekomendasi personalisasi yang dapat menyesuaikan saran berdasarkan riwayat interaksi pengguna atau kesamaan konten.

### Goals

- Mengembangkan sistem rekomendasi game berbasis **content-based filtering** yang menyarankan game berdasarkan kemiripan konten (seperti genre dan usia game).
- Membangun sistem rekomendasi berbasis **collaborative filtering** yang menyarankan game berdasarkan perilaku dan preferensi pengguna lain dengan pola yang mirip.
- Membandingkan performa kedua pendekatan tersebut berdasarkan metrik evaluasi untuk mengetahui pendekatan mana yang lebih efektif dalam memberikan rekomendasi.

### Solution Statements

Untuk mencapai tujuan di atas, dua pendekatan sistem rekomendasi akan digunakan secara terpisah:

1. **Content-Based Filtering**  
   Sistem ini akan merekomendasikan game berdasarkan atribut konten dari game yang pernah disukai pengguna. Fitur yang digunakan dalam pendekatan ini antara lain:
   - `genres`: genre atau kategori dari game.
   - `game_age_years`: usia game dalam tahun.

2. **Collaborative Filtering**  
   Sistem ini akan merekomendasikan game berdasarkan kesamaan preferensi di antara pengguna. Fitur yang digunakan dalam pendekatan ini antara lain:
   - `game_id`, `user_id`, `rating`, `review`, `name`: yang mencerminkan interaksi pengguna terhadap berbagai game.

Dengan membandingkan dua pendekatan ini, proyek ini bertujuan untuk memberikan insight mengenai metode mana yang lebih efektif dalam menyajikan rekomendasi game personal yang relevan kepada pengguna Steam.


## Data Understanding

Proyek ini menggunakan dua dataset utama dari platform game **Steam**, yang diperoleh melalui situs [Kaggle](https://www.kaggle.com/datasets/tmesalles/steam-recommender). Dataset ini mencakup informasi terkait game yang tersedia di Steam dan data ulasan pengguna terhadap game-game tersebut. Dataset ini digunakan untuk membangun dan membandingkan dua pendekatan sistem rekomendasi: *content-based filtering* dan *collaborative filtering*.

### Informasi Dataset

1. **steam_games_data.csv**  
   Dataset ini berisi informasi tentang game yang tersedia di Steam. Terdapat **943 entri (baris)** dan **8 kolom**.  
   Beberapa data tidak lengkap, seperti kolom `genres` yang memiliki 897 nilai non-null, serta `average_rating_score` yang hanya tersedia untuk 503 game.  
   Sebanyak **216 game tidak memiliki tanggal rilis** yang valid karena adanya entri seperti "Coming soon", "To be announced", atau format tanggal tidak standar.

2. **steam_reviews.csv**  
   Dataset ini memuat ulasan dan penilaian dari pengguna terhadap game tertentu. Terdapat **8.473 entri** dan **4 kolom**. Hampir seluruh data memiliki nilai lengkap, kecuali kolom `review` yang kehilangan 14 data.

### Variabel dalam Dataset

#### steam_games_data.csv:
- `game_id` : ID unik untuk setiap game.
- `name` : Nama game.
- `genres` : Genre game, dapat berisi lebih dari satu genre.
- `current_price` : Harga terkini game dalam USD.
- `original_price` : Harga asli sebelum diskon.
- `discount_percent` : Persentase diskon yang berlaku.
- `average_rating_score` : Rata-rata skor rating dari pengguna (tidak tersedia untuk banyak game).
- `release_date` : Tanggal rilis game.

#### steam_reviews.csv:
- `game_id` : ID game yang direview.
- `user_id` : ID unik pengguna yang memberikan ulasan.
- `rating` : Nilai rating yang diberikan pengguna (biasanya dalam skala 1–10).
- `review` : Teks ulasan yang diberikan pengguna.

### Exploratory Data Analysis (EDA)

Beberapa temuan penting dari eksplorasi data awal:

- **Kekosongan Data**:
  - Terdapat **440 game yang tidak memiliki nilai `average_rating_score`**, dan hanya sekitar **40** dari mereka memiliki ulasan yang dapat digunakan sebagai alternatif. Oleh karena itu, fitur ini tidak digunakan dalam *content-based filtering* untuk menjaga konsistensi.
  - Terdapat **216 entri dengan tanggal rilis tidak valid** seperti "Coming soon" atau "To be announced", yang menunjukkan sebagian game belum dirilis atau informasi rilis tidak jelas.

- **Korelasi Harga**:
  - Korelasi antara `current_price` dan `original_price` sangat tinggi (**0.99**), artinya harga saat ini hampir selalu mencerminkan harga aslinya.
  - Sebaliknya, `discount_percent` memiliki korelasi rendah dengan harga, menunjukkan bahwa diskon diberikan secara independen dari harga dasar game.

- **Distribusi Review**:
  - Sebanyak **202 game memiliki kurang dari 5 review**, mengindikasikan bahwa banyak game kurang populer atau jarang dimainkan.
  - Hanya sebagian kecil game yang menerima review dalam jumlah besar, menandakan dominasi oleh game-game populer.

- **Genre Populer dan Langka**:
  - Genre **"Indie"** paling mendominasi dalam dataset.
  - Diikuti oleh genre **"Action"** dan **"Adventure"**.
  - Beberapa genre seperti **"Animation & Modeling"**, **"Design & Illustration"**, dan **"Utilities"** sangat jarang ditemukan, menunjukkan minat atau produksi yang rendah.


## Data Preparation

Pada bagian ini dijelaskan teknik-teknik persiapan data yang dilakukan secara berurutan untuk dua pendekatan sistem rekomendasi, yaitu content-based filtering dan collaborative filtering.

### Content-Based Filtering

1. **Pembersihan dan perbaikan kolom `release_date`**  
   Kolom `release_date` awalnya mengandung banyak nilai kosong (NaT) karena format tanggal yang tidak standar, seperti kuartal tahun ("Q3 2025"), hanya tahun ("2025"), atau format nama bulan dengan tahun ("May 2025"). Fungsi `clean_release_date` dibuat untuk mengubah format-format tersebut menjadi tanggal yang lebih terstruktur. Tahap ini penting agar data tanggal rilis dapat digunakan secara konsisten dalam analisis dan pemodelan.

2. **Penghitungan usia game dalam tahun (`game_age_years`)**  
   Setelah tanggal rilis dibersihkan, dihitung usia game dalam satuan tahun dengan mengambil selisih antara tanggal saat ini dan tanggal rilis, kemudian dibagi dengan 365.25 hari untuk memperoleh umur game secara desimal. Fitur ini menambah informasi relevan terkait umur game yang dapat berpengaruh pada rekomendasi.

3. **Penanganan data kosong dan pemilihan fitur**  
   Data yang memiliki nilai kosong (NaN) pada kolom `game_age_years`, `name`, dan `genres` dihapus untuk memastikan analisis menggunakan data yang valid dan lengkap. Hasilnya adalah dataset bersih dengan 681 baris yang siap diproses lebih lanjut.

4. **Transformasi fitur genre dengan TF-IDF**  
   Fitur `genres` yang berupa teks diubah menjadi representasi numerik menggunakan metode TF-IDF (Term Frequency-Inverse Document Frequency). Transformasi ini memungkinkan pengukuran kemiripan antar game berdasarkan genre secara matematis, yang menjadi dasar pembuatan rekomendasi.

5. **Normalisasi fitur usia game dan penggabungan fitur**  
   Fitur usia game (`game_age_years`) dinormalisasi dengan MinMaxScaler agar skala nilainya antara 0 hingga 1. Normalisasi dilakukan untuk menyamakan skala dengan fitur TF-IDF genre sehingga tidak ada fitur yang mendominasi dalam proses pemodelan. Kemudian, matriks TF-IDF dan data usia game yang sudah dinormalisasi digabungkan menjadi satu fitur numerik lengkap yang digunakan dalam model content-based.

### Collaborative Filtering

1. **Penggabungan data ulasan dan data deskripsi game**  
   Data review yang berisi rating dan ulasan pengguna digabungkan dengan data deskripsi game berdasarkan `game_id`. Penggabungan ini memudahkan pemetaan rating ke game tertentu yang akan digunakan dalam pemodelan.

2. **Encoding user dan game ID**  
   Untuk keperluan pemodelan, `user_id` dan `game_id` diubah menjadi format numerik berurutan mulai dari 0, agar sesuai dengan kebutuhan algoritma machine learning yang umumnya membutuhkan input numerik. Kolom baru `user` dan `game` berisi ID terencode ini ditambahkan ke dataset review.

3. **Normalisasi rating**  
   Rating diubah menjadi tipe data float32 dan dinormalisasi ke rentang [0,1] untuk memudahkan dan menstabilkan proses pelatihan model.

4. **Pengacakan dan pembagian data latih dan validasi**  
   Data review diacak secara acak untuk menghindari bias urutan data asli. Selanjutnya data dibagi menjadi 80% untuk pelatihan (train) dan 20% untuk validasi (test) untuk memastikan model dapat diuji secara objektif pada data yang belum pernah dilihat sebelumnya.

---

Tahapan-tahapan data preparation di atas dilakukan untuk memastikan data yang digunakan valid, konsisten, dan berada dalam format yang sesuai agar model rekomendasi dapat belajar secara optimal serta menghasilkan output yang akurat dan relevan.


## Modeling

Pada tahap ini, saya membangun dua sistem rekomendasi dengan pendekatan yang berbeda, yaitu Content-based Filtering dan Collaborative Filtering, untuk menyelesaikan permasalahan rekomendasi game.

### 1. Content-based Filtering

Metode ini menggunakan fitur gabungan berupa representasi genre game (dengan TF-IDF) dan usia game (setelah dinormalisasi) untuk menghitung kemiripan antar game. Saya menggunakan fungsi **cosine similarity** untuk menghasilkan matriks kemiripan antar game dalam dataset.

Setiap elemen matriks tersebut menunjukkan tingkat kemiripan antara dua game berdasarkan genre dan usia game. Matriks ini kemudian digunakan dalam fungsi rekomendasi `game_recommendations`, yang menerima nama game input dan mengembalikan daftar 5 game paling mirip beserta skor similarity-nya.

Sebagai contoh, rekomendasi 5 game yang paling mirip dengan game **"Metal Punch"** ditampilkan dengan nilai similarity yang mendekati 1, menandakan kesamaan konten yang tinggi. Sistem ini efektif untuk memberikan rekomendasi berdasarkan fitur game yang sudah diketahui, tanpa perlu data pengguna.

**Kelebihan:**
- Tidak memerlukan data interaksi pengguna, sehingga bisa digunakan walau data rating pengguna terbatas.
- Rekomendasi transparan karena didasarkan pada fitur konten yang dapat dianalisis.

**Kekurangan:**
- Terbatas pada fitur yang digunakan (genre dan usia), sehingga tidak menangkap preferensi pengguna secara personal.
- Rentan terhadap masalah "cold start" untuk game baru yang minim fitur.

---

### 2. Collaborative Filtering

Model Collaborative Filtering dibangun menggunakan pendekatan embedding dengan TensorFlow Keras. Model ini mempelajari representasi laten (embedding) untuk masing-masing user dan game, sehingga dapat memprediksi rating yang mungkin diberikan user pada game tertentu.

Model menggunakan dua embedding layer (untuk user dan game), ditambah bias masing-masing, kemudian menghitung dot product dan menerapkan fungsi sigmoid untuk prediksi rating ter-normalisasi [0,1]. Model dilatih dengan loss binary crossentropy, optimizer Adam, serta metrik evaluasi RMSE.

Untuk rekomendasi, saya memilih satu user secara acak, kemudian memprediksi rating untuk semua game yang belum dimainkan user tersebut. Top-10 game dengan rating prediksi tertinggi diberikan sebagai rekomendasi. Contoh hasil rekomendasi untuk user tersebut menampilkan daftar game seperti *Tales of Kenzera™: ZAU*, *The Last Campfire*, dan *LIMBO*.

**Kelebihan:**
- Menangkap preferensi pengguna secara personal berdasarkan interaksi historis.
- Mampu memberikan rekomendasi yang lebih relevan dan beragam.

**Kekurangan:**
- Membutuhkan data rating pengguna yang cukup banyak dan lengkap.
- Mengalami kesulitan pada kasus cold start untuk pengguna atau game baru yang belum memiliki interaksi.

---

### Output Top-N Recommendation

- **Content-based Filtering:**
  
  ![image](https://github.com/user-attachments/assets/989038d4-b7c0-4d43-9e35-5b342ca68ae6)
  
  Rekomendasi 5 game paling mirip dengan "Metal Punch" beserta skor similarity.

- **Collaborative Filtering:**
  
  ![image](https://github.com/user-attachments/assets/1a575401-dad8-43c9-ab9c-eb154f263c0d)

  Rekomendasi 10 game yang diprediksi paling disukai oleh user tertentu berdasarkan model embedding, beserta 5 game favorit yang sudah dimainkan user tersebut.

---

Dengan dua pendekatan ini, sistem rekomendasi dapat memberikan pilihan yang baik antara rekomendasi berdasarkan karakteristik game dan preferensi pengguna secara individual.


## Evaluation

Pada tahap evaluasi, saya menggunakan metrik yang sesuai dengan jenis model rekomendasi yang dibuat, yaitu Content-Based Filtering dan Collaborative Filtering.

### 1. Content-Based Filtering  
Metrik evaluasi yang digunakan adalah **Coverage@k**. Coverage mengukur seberapa banyak item (dalam hal ini game) dari seluruh koleksi yang dapat direkomendasikan dalam top-k rekomendasi. Coverage yang tinggi menunjukkan sistem rekomendasi dapat menjangkau berbagai macam item, sehingga memberikan variasi rekomendasi yang luas.

Hasil evaluasi menunjukkan Coverage@5 mencapai **98.38%**, yang berarti hampir seluruh game dalam dataset dapat muncul sebagai rekomendasi dalam daftar 5 game teratas. Ini menandakan model content-based efektif dalam memanfaatkan fitur genre dan usia game untuk merekomendasikan game-game yang relevan dan beragam.

---

### 2. Collaborative Filtering  
Untuk model collaborative filtering, evaluasi dilakukan menggunakan dua metrik utama: **Loss** dan **Root Mean Squared Error (RMSE)** pada data validasi.

- **Loss** adalah ukuran dari perbedaan rata-rata antara nilai rating yang sebenarnya diberikan oleh pengguna dan rating yang diprediksi oleh model. Loss yang lebih kecil menunjukkan prediksi model lebih akurat. Dalam kasus ini, digunakan fungsi loss binary crossentropy yang mengukur seberapa baik model memprediksi probabilitas preferensi user terhadap game.

- **RMSE (Root Mean Squared Error)** mengukur akar dari rata-rata kuadrat selisih antara rating asli dan rating prediksi. RMSE memberikan gambaran seberapa besar kesalahan prediksi secara umum, dengan satuan yang sama dengan data asli sehingga mudah diinterpretasikan. Nilai RMSE yang lebih kecil menunjukkan model memberikan prediksi yang lebih dekat dengan rating asli.

Hasil evaluasi menunjukkan model mencapai nilai validation loss terbaik sebesar **0.5200** dan RMSE sebesar **0.4082** pada epoch ke-100. Grafik performa model memperlihatkan penurunan loss dan RMSE selama pelatihan, menandakan model belajar dan meningkatkan akurasi prediksinya. Namun, setelah titik tertentu, nilai loss dan RMSE pada data validasi mulai stabil dan sedikit naik, yang mengindikasikan adanya potensi overfitting.

Walaupun demikian, nilai evaluasi ini menunjukkan model berhasil menangkap pola interaksi pengguna dan memberikan prediksi rating yang cukup akurat, sehingga dapat digunakan untuk merekomendasikan game yang sesuai preferensi pengguna.

---

### Kesimpulan Evaluasi  
- **Content-Based Filtering** memberikan rekomendasi berdasarkan kesamaan fitur eksplisit dengan cakupan luas, namun kurang memahami preferensi tersembunyi dari pengguna.  
- **Collaborative Filtering** mempelajari pola rating dan interaksi pengguna sehingga dapat memberikan rekomendasi yang lebih personal, meskipun perlu diwaspadai potensi overfitting dan kebutuhan data interaksi yang cukup.

Kedua pendekatan ini saling melengkapi dan dapat digabungkan untuk meningkatkan performa sistem rekomendasi secara keseluruhan.

---

*Ini menandai akhir laporan evaluasi sistem rekomendasi.*
