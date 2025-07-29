---
title: Lab Instruksi Azure Administrator
permalink: index.html
layout: home
---

# 📘 Dokumentasi Lab AZ-Administrator

Halaman ini berisi dokumentasi lab yang telah **disederhanakan** dan **diadaptasi** dari sumber resmi Microsoft untuk kursus **AZ-104: Microsoft Azure Administrator**.

---

## 📄 Tentang Halaman Ini

Materi pada halaman ini berasal dari repositori resmi Microsoft Learning, namun telah dimodifikasi dengan tujuan:

- ✅ Mempermudah pemahaman bagi peserta pelatihan lokal
- ✅ Menyederhanakan langkah-langkah tanpa menghilangkan esensi
- ✅ Memberikan tambahan penjelasan kontekstual sesuai kebutuhan

> **Sumber Asli:**
> [github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator)

---

## 🧭 Tujuan Adaptasi

Modifikasi ini dilakukan untuk:

- Membantu pemula memahami konsep Azure lebih cepat
- Digunakan dalam workshop atau kelas pelatihan intensif
- Menyesuaikan istilah dan instruksi agar lebih familiar


---

Terima kasih telah menggunakan materi ini! Semoga bermanfaat dalam proses belajarmu tentang Azure ☁️



# Konten Folder

File lab yang diperlukan dapat [Download Disini](https://github.com/zaxrmdn/Lazidn25/archive/refs/heads/file.zip)


## Panduan Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Module | Lab |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

## Interaktif Lab (Non-Praktek)
[Interaktif Lab Guide](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator)

## Demonstrasi

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Module | Demonstration |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

---

## ⚠️ Hak Cipta

> Materi asli merupakan hak cipta © Microsoft.  
> Adaptasi ini dibuat hanya untuk keperluan pembelajaran, pelatihan.  

---

## 🙋 Kontak

Jika kamu memiliki pertanyaan, saran, atau masukan terkait lab ini, silakan hubungi:

**Zakaria Ramadan**  
📧 [zakaria@idn.id](mailto:zakaria@idn.id)  
🌐 [https://zakaria.web.id](https://zakaria.web.id)


> 🛈 Catatan: Lab ini merupakan versi modifikasi dari materi resmi Microsoft AZ-104. Beberapa bagian telah disederhanakan dan disesuaikan untuk pembelajaran lokal.
