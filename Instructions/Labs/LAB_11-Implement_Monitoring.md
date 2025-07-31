```markdown
---
lab:
    title: 'Lab 11: Implement Monitoring'
    module: 'Administer Monitoring'
---

# Lab 11 - Implement Monitoring

## Lab introduction

Dalam lab ini, Anda akan mempelajari tentang Azure Monitor. Anda akan membuat sebuah alert dan mengirimkannya ke action group. Anda juga akan memicu dan menguji alert tersebut serta memeriksa activity log.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur dalam lab ini. Anda dapat mengganti region, namun langkah-langkah ditulis dengan menggunakan **Indonesia Central**.

## Estimasi waktu: 40 menit

## Lab scenario

Organisasi Anda telah memigrasikan infrastrukturnya ke Azure. Penting agar Administrator diberi tahu tentang perubahan infrastruktur yang signifikan. Anda berencana mengevaluasi kemampuan Azure Monitor, termasuk Log Analytics.

## Interactive lab simulations

>**Catatan**: Simulasi lab yang sebelumnya tersedia telah dihentikan.

## Architecture diagram

![Diagram of the architecture tasks](../media/az104-lab11-architecture.png)

## Job skills

+ Task 1: Use a template to provision an infrastructure.
+ Task 2: Create an alert.
+ Task 3: Configure action group notifications.
+ Task 4: Trigger an alert and confirm it is working.
+ Task 5: Configure an alert processing rule.
+ Task 6: Use Azure Monitor log queries.

## Extend your learning with Copilot
Copilot dapat membantu Anda mempelajari cara menggunakan alat scripting Azure. Copilot juga dapat membantu dalam area yang tidak dibahas dalam lab atau ketika Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasikan ke *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba prompt berikut:

+ Apa saja langkah konfigurasi dasar agar mendapatkan pemberitahuan ketika sebuah virtual machine di Azure mati?
+ Bagaimana saya bisa diberi tahu saat sebuah Azure alert terpicu?
+ Susun query Azure Monitor untuk memberikan informasi performa CPU dari virtual machine.

## Learn more with self-paced training

+ [Improve incident response with alerting on Azure](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/). Respon terhadap insiden dan aktivitas dalam infrastruktur Anda melalui kemampuan alerting di Azure Monitor.
+ [Monitor your Azure virtual machines with Azure Monitor](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/). Pantau VM Azure Anda menggunakan Azure Monitor untuk mengumpulkan dan menganalisis metrik dan log dari host dan client VM.

## Key takeaways

Selamat telah menyelesaikan lab ini. Berikut adalah poin-poin penting dari lab ini:

+ Alerts membantu Anda mendeteksi dan menangani masalah sebelum pengguna menyadari adanya masalah pada infrastruktur atau aplikasi Anda.
+ Anda dapat membuat alert berdasarkan metrik atau sumber data log mana pun dalam platform data Azure Monitor.
+ Alert rule memonitor data Anda dan menangkap sinyal yang menunjukkan sesuatu sedang terjadi pada resource yang ditentukan.
+ Sebuah alert akan terpicu jika kondisi pada alert rule terpenuhi. Beberapa aksi (email, SMS, push, voice) dapat dijalankan.
+ Action group mencakup individu-individu yang seharusnya diberi tahu tentang alert.
```
