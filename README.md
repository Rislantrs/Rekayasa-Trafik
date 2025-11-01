# 🚦 **Traffic Engineering Simulation with Machine Learning**
---

## 👥 **Team Members**

| Nama                              | Peran                      |
| --------------------------------- | -------------------------- |
| M. Rislan Tristansyah             | Pengembang Dataset & Model |
---

## 🎯 **Tujuan Proyek**

Proyek ini bertujuan untuk **mensimulasikan kondisi jaringan dan volume trafik secara realistis** menggunakan data sintetis (*dummy data*) serta **menerapkan model Machine Learning (LSTM)** untuk **memprediksi beban trafik jaringan berdasarkan pola waktu**.

Pendekatan ini menggambarkan proses **rekayasa trafik modern** di mana data digunakan untuk menganalisis perilaku jaringan, mendeteksi anomali, dan memperkirakan lonjakan trafik yang dapat mengganggu performa sistem.

---

## 📊 **Deskripsi Dataset**

Dataset dibuat secara sintetis menggunakan Python dan mencerminkan perilaku jaringan harian selama **30 hari (720 jam)**.
Berikut fitur-fitur yang dihasilkan:

| Kolom                  | Deskripsi                                              |
| ---------------------- | ------------------------------------------------------ |
| `Hour`                 | Indeks waktu dalam jam                                 |
| `User`                 | ID pengguna acak (User_1 – User_100)                   |
| `Day`                  | Hari dalam seminggu                                    |
| `TrafficVolume`        | Volume trafik utama per jam                            |
| `BandwidthConsumption` | Konsumsi bandwidth aktual                              |
| `BandwidthResources`   | Sumber daya bandwidth yang tersedia                    |
| `SuddenSpike`          | Lonjakan trafik mendadak                               |
| `Overloading`          | Boolean: TRUE jika trafik melebihi kapasitas           |
| `Latency`              | Latensi jaringan (ms), meningkat saat overloading      |
| `CyberAttack`          | 1 jika terjadi serangan siber, 0 jika tidak            |
| `PhysicalDisruption`   | 1 jika terjadi gangguan fisik/elektronik, 0 jika tidak |

Dataset disimpan sebagai:

```
enhanced_simulated_traffic_data.csv
```

---

## ⚙️ **Metodologi Proyek**

### 1️⃣ Pembuatan Data Dummy

* Menggunakan **fungsi sinusoidal** untuk mensimulasikan pola trafik harian.
* Menambahkan **noise Gaussian** untuk variasi realistis.
* Menyertakan **anomali acak (Sudden Spike)** untuk menguji ketahanan model.
* Memasukkan kondisi **overloading**, **serangan siber**, dan **gangguan fisik**.

### 2️⃣ Pra-Pemrosesan Data

* **Normalisasi** fitur `TrafficVolume` menggunakan `MinMaxScaler` agar berada dalam rentang [0, 1].
* Pembagian data:

  * **80%** untuk training
  * **20%** untuk testing
* Membuat **sekuens waktu (windowing)** berdurasi 24 jam untuk masukan model LSTM.

### 3️⃣ Model Machine Learning

Menggunakan **Recurrent Neural Network (RNN)** berbasis **LSTM (Long Short-Term Memory)** untuk memprediksi `TrafficVolume` berikutnya berdasarkan pola 24 jam sebelumnya.

**Arsitektur model:**

```python
Sequential([
    LSTM(50, return_sequences=True, input_shape=(24, 1)),
    LSTM(50, return_sequences=False),
    Dense(20, activation='relu'),
    Dense(1)
])
```

**Kompilasi:**

```python
optimizer = 'adam'
loss = 'mean_squared_error'
```

---

## 📈 **Evaluasi Model**

| Metrik                            | Nilai      |
| --------------------------------- | ---------- |
| Mean Squared Error (MSE)          | **5.3261** |
| Mean Absolute Error (MAE)         | **1.8892** |
| R² (Coefficient of Determination) | **0.9883** |

📊 **Interpretasi:**

* Model mampu menjelaskan **98.8% variabilitas trafik**.
* Error relatif kecil terhadap rentang volume trafik, menandakan prediksi cukup stabil.
* Grafik aktual vs prediksi menunjukkan pola sinusoidal yang serupa — model berhasil menangkap siklus harian trafik.

---

## 💡 **Analisis & Insight**

1. **Pola periodik 24 jam** berhasil ditangkap oleh LSTM, menandakan model memahami siklus penggunaan jaringan harian.
2. **Noise dan lonjakan trafik (spikes)** menjadi tantangan bagi model, namun tetap dapat diestimasi dengan baik.
3. Fitur tambahan seperti **Overloading**, **Latency**, dan **CyberAttack** dapat dimanfaatkan untuk klasifikasi risiko jaringan di tahap lanjut.

---

## 🚀 **Rencana Pengembangan Selanjutnya**

1. **Klasifikasi Anomali Trafik**

   * Gunakan model **Random Forest / XGBoost** untuk mendeteksi kondisi abnormal seperti serangan siber atau lonjakan mendadak.
2. **Optimasi Model LSTM**

   * Lakukan *hyperparameter tuning* (learning rate, batch size, layer depth).
   * Gunakan **GRU** atau **Bidirectional LSTM** untuk membandingkan performa.
3. **Integrasi dengan Simulasi Jaringan Nyata**

   * Menghubungkan data dummy ini ke simulasi jaringan SDN/IoT.
4. **Visualisasi Dashboard**

   * Buat **dashboard interaktif (Streamlit / Plotly)** untuk memantau volume trafik secara real-time.

---

## 🧰 **Teknologi yang Digunakan**

| Kategori         | Tools / Library                  |
| ---------------- | -------------------------------- |
| Bahasa           | Python 3.11                      |
| Data Processing  | Pandas, NumPy                    |
| Visualisasi      | Matplotlib                       |
| Machine Learning | TensorFlow / Keras, Scikit-learn |
| Environment      | Jupyter Notebook                 |

---

## 📂 **Struktur Repository**

```
Traffic-Engineering-ML/
├── volumeTrafik.ipynb                  # Notebook utama proyek
├── simulated_traffic_data.csv          # Dataset dasar
├── enhanced_simulated_traffic_data.csv # Dataset dengan fitur tambahan
├── README.md                           # Dokumentasi proyek
└── requirements.txt                    # Daftar pustaka Python
```

---

## 🏁 **Kesimpulan**

Proyek ini membuktikan bahwa:

* Model **LSTM** efektif untuk **prediksi trafik jaringan berbasis waktu**.
* Dataset sintetis dapat menjadi **alat pembelajaran praktis** untuk memahami dinamika rekayasa trafik.
* Penggunaan *Machine Learning* dalam bidang **Network Engineering** membuka peluang besar untuk optimasi dan prediksi performa jaringan di masa depan.
