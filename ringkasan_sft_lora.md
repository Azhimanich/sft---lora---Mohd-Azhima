# Ringkasan Proyek: Supervised Fine-Tuning (SFT) dengan LoRA

Dokumen ini berisi deskripsi singkat, informasi dataset, dan model yang dipilih berdasarkan proyek eksperimen *Fine-Tuning* Language Model untuk bahasa daerah Indonesia.

---

## 1. Deskripsi Singkat
Proyek ini berfokus pada penerapan **Supervised Fine-Tuning (SFT)** menggunakan metode **Parameter-Efficient Fine-Tuning (PEFT)**, secara spesifik menggunakan **LoRA (Low-Rank Adaptation)**. 

* **Tujuan Utama:** Melatih atau mengadaptasi *small language model* agar mampu memahami dan mengikuti instruksi (*instruction-following*) dalam berbagai bahasa daerah di Indonesia yang sebelumnya kurang dikuasai oleh *base model*.
* **Teknologi Stack:** Proyek ini dibangun sepenuhnya menggunakan library standar dari Hugging Face ekosistem, yaitu:
  * `transformers` (untuk memuat model dan tokenizer)
  * `peft` (untuk implementasi arsitektur LoRA)
  * `trl` (untuk menyediakan `SFTTrainer` dan konfigurasi pelatihan)
  * `datasets` (untuk memuat dan memproses data)
* **Karakteristik Implementasi:** Proyek ini murni menggunakan pustaka standar Hugging Face tanpa mengandalkan layanan eksternal atau optimasi *as-a-service* seperti Unsloth.

---

## 2. Dataset
Dataset yang digunakan dalam proyek ini dirancang khusus untuk instruksi multibahasa daerah di Indonesia.

* **Nama Pengenal Dataset:** `bryandts/instruction-dataset-indo-java-sunda-bali-gayo-batak-alas-minang-betawi`
* **Cakupan Bahasa:** Meliputi berbagai bahasa daerah di Indonesia, termasuk bahasa Jawa, Sunda, Bali, Gayo, Batak, Alas, Minang, dan Betawi.
* **Ukuran & Pembagian Data:**
  * Jumlah total sampel yang digunakan dalam eksperimen dibatasi maksimal **1.000 baris** demi efisiensi komputasi.
  * Data dipecah secara internal dengan proporsi **90% untuk Data Latih (*Train Set*)** sebanyak 900 contoh dan **10% untuk Data Uji/Validasi (*Eval Set*)** sebanyak 100 contoh.
* **Format Data:** Data dipetakan (*mapping*) secara adaptif ke dalam struktur percakapan (*role-based*) yang terdiri dari komponen `user` dan `assistant`, kemudian dibungkus menggunakan **ChatML template** bawaan resmi dari model yang dipilih (Qwen).

---

## 3. Model yang Dipilih
Model yang dipilih sebagai pondasi (*base model*) adalah model bahasa berukuran kecil yang efisien namun memiliki performa tinggi di kelasnya.

* **ID Model (Hugging Face):** `Qwen/Qwen2.5-0.5B`
* **Karakteristik Base Model:**
  * Arsitektur: `Qwen2ForCausalLM`
  * Jumlah Parameter Asli: **494.032.768 parameter** (~494M)
  * Presisi Data: Ditentukan menggunakan `torch.float16` untuk efisiensi memori pada GPU (seperti Tesla T4).
* **Konfigurasi LoRA (Adapter):**
  Untuk mengadaptasi bahasa daerah tanpa merusak pengetahuan dasar model, ditambahkan matriks dimensi rendah (LoRA) pada layer proyeksi linear utama dengan spesifikasi berikut:
  * **Rank ($r$):** 16
  * **LoRA Alpha ($lpha$):** 32
  * **Target Modules:** Menyasar hampir seluruh modul proyeksi utama, yaitu `q_proj`, `k_proj`, `v_proj`, `o_proj`, `gate_proj`, `up_proj`, dan `down_proj`.
  * **LoRA Dropout:** 0.05
  * **Parameter yang Dilatih (*Trainable*):** Setelah dipasang adapter, parameter yang dilatih hanya sebesar **8.798.208 parameter** (atau sekitar **1,75%** dari total keseluruhan model sebesar 502,8M), menjadikan proses *fine-tuning* sangat ringan dan cepat.
