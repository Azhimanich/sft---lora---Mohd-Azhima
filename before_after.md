# Tabel Perbandingan Respons: Base Model vs. Fine-Tuned Model

Dokumen ini memuat tabel komparasi hasil keluaran (*output*) antara **Base Model (Qwen2.5-0.5B)** sebelum latihan dan **Fine-Tuned Model** setelah melalui proses *Supervised Fine-Tuning* (SFT) dengan adapter LoRA, menggunakan contoh uji bahasa daerah (Kasus: Bahasa Kerinci).

---

| No | Prompt / Instruksi (Bahasa Kerinci) | Respons Base Model (Sebelum Latihan) | Respons Fine-Tuned Model (Setelah Latihan) |
|:--:|:---|:---|:---|
| **1** | **Camano caro mamasak gulai pakis khas Kerinci?** | - Lihat Video... *(Respons tidak lengkap/terpotong, cenderung memberikan pintasan teks internet).* | Menguraikan langkah demi langkah memasak gulai pakis khas Kerinci menggunakan kosakata campuran/bahasa daerah terstruktur. |
| **2** | **Apo bae tempat wisata nan rancak di daerah Kerinci?** | Memberikan jawaban acak atau gagal mengenali konteks entitas lokal secara spesifik dalam format instruksi bahasa daerah yang natural. | Mampu melist destinasi wisata di Kerinci (seperti Gunung Kerinci, Danau Kaco, atau Danau Kerinci) dengan gaya bahasa instruksional yang lebih selaras. |
| **3** | **Sebutkan tradisi unik nan ado di Kerinci!** | Sering kali mengalami halusinasi bahasa (tercampur dengan bahasa asing/Tagalog/karakter acak) atau gagal memahami semantik "nan ado di Kerinci". | Menghasilkan penjelasan mengenai tradisi lokal atau adat istiadat khas Kerinci dengan struktur kalimat yang lebih konsisten mengikuti gaya bahasa daerah. |
| **4** | **Tolong buatkan pantun sederhana dalam bahasa Kerinci.** | Mengeluarkan teks repetitif atau teks tidak bermakna yang tidak mengikuti struktur rima pantun sama sekali. | Berusaha menyusun baris-baris kalimat berima yang menyerupai struktur pantun/puisi rakyat sesuai perintah instruksi. |
| **5** | **Camano caro kito iduik rukun jo tatangga?** | *"1. Caman angin ngkakneng kung manggagawa ay tinayon ang katubo sa pag-aaral. 2. Napa ang angin ngkakneng naman, tayo lang at maaari..."*<br><br>*(Gagal total, terjadi kerusakan struktur bahasa/bahasa asing/Tagalog).* | *"Kapingan bisa mambuekke ngepiku kepalake, tapi menehi kota ya tiosan wengi..."*<br><br>*(Meskipun masih terdapat repetisi token paska-pelatihan karena keterbatasan dataset/kapasitas model 0.5B, model sudah berusaha berpindah ke kosakata berbasis ragam bahasa lokal/daerah).* |

---

### Catatan Evaluasi & Analisis:
* **Base Model Performa:** Sebelum dilatih, model `Qwen2.5-0.5B` cenderung mengalami *language drifting* (berpindah ke bahasa lain seperti Tagalog atau Inggris) ketika dipicu menggunakan instruksi bahasa daerah yang sangat spesifik seperti bahasa Kerinci. Hal ini dikarenakan representasi korpus data bahasa daerah tersebut sangat minim di dalam data *pre-training* aslinya.
* **Fine-Tuned Model Performa:** Setelah penerapan SFT LoRA dengan dataset instruksi bahasa daerah, model menunjukkan perubahan arah pembentukan token (*token distribution*). Model secara konsisten berusaha merespons menggunakan struktur kalimat lokal atau kosakata daerah Indonesia barat/melayu, meminimalkan interferensi bahasa asing eksternal yang tidak relevan, meskipun pada model sekecil 0.5B masih rentan dijumpai gejala repetisi teks apabila parameter generasi (*decoding strategy*) kurang optimal.
