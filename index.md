---
title: Lab Instruksi Azure Administrator
permalink: index.html
layout: home
---

# Tentang Halaman ini
Dokumentasi lab pada halaman ini merupakan versi yang telah disederhanakan dari Microsoft Official Course AZ-104: Microsoft Azure Administrator.
---

📝 Catatan Penting:

Konten disusun ulang agar lebih mudah dipahami oleh peserta pelatihan lokal.

Beberapa langkah telah diringkas, diberi penjelasan tambahan, atau diadaptasi untuk kebutuhan praktikum secara mandiri.

Semua hak cipta asli tetap dimiliki oleh Microsoft.

---
🔗 Sumber Asli:
AZ-104 Microsoft Learning GitHub Repository


# Konten Folder

File lab yang diperlukan dapat [Download Disini](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)

## Panduan Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Module | Lab |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

## Demonstrasi

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Module | Demonstration |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
