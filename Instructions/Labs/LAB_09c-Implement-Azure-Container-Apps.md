---
lab:
    title: 'Lab 09c: Implement Azure Container Apps'
    module: 'Administer PaaS Compute Options'
---

# Lab 09c - Implement Azure Container Apps

## Lab introduction

Dalam lab ini, Anda akan mempelajari cara mengimplementasikan dan menerapkan Azure Container Apps.

Lab ini memerlukan langganan Azure. Jenis langganan Anda mungkin memengaruhi ketersediaan fitur dalam lab ini. Anda dapat mengubah region, tetapi langkah-langkah ditulis menggunakan **Indonesia Central**.

## Estimated timing: 15 minutes

## Lab scenario

Organisasi Anda memiliki aplikasi web yang berjalan di virtual machine di data center lokal. Organisasi ingin memindahkan seluruh aplikasi ke cloud tetapi tidak ingin mengelola banyak server. Anda memutuskan untuk mengevaluasi Azure Container Apps.

## Architecture diagram

![Diagram of the tasks.](../media/az104-lab09b-aca-architecture.png)

## Job skills

- Task 1: Membuat dan mengonfigurasi Azure Container App dan environment.
- Task 2: Menguji dan memverifikasi deployment dari Azure Container App.

## Task 1: Create and configure an Azure Container App and environment

Azure Container Apps membawa konsep managed Kubernetes cluster lebih jauh dengan mengelola environment cluster dan menyediakan layanan terkelola lainnya di atas cluster tersebut. Berbeda dengan Azure Kubernetes cluster, di mana Anda masih harus mengelola clusternya, Azure Container Apps menghilangkan sebagian kompleksitas dalam pengaturan Kubernetes cluster.

1. Dari Azure portal, cari dan pilih `Container Apps`.

2. Pilih **+ Create**, lalu dari drop-down menu, pilih **Container App**. Perhatikan juga pilihan lainnya.

3. Gunakan informasi berikut untuk mengisi bagian **Basics**.

    | Setting | Action |
    |---|---|
    | Subscription | Pilih langganan Azure Anda |
    | Resource group | `rg9-p1` |
    | Container app name | `my-app` |
    | Region | **Indonesia Central** |
    | Container Apps Environment | Pilih **Create new** > Isi nama environment dengan `my-environment` > Klik **Create** |

4. Klik tab **Next: Container** dan pastikan **Use quickstart image** dicentang. Anda mungkin perlu menggulir ke atas untuk melihat pengaturan ini.

5. Pastikan **Quickstart image** disetel ke **Simple hello world container**. Perhatikan pilihan lainnya.

6. Pilih **Review and create** lalu klik **Create**.

    >**Catatan:** Tunggu hingga container app selesai diterapkan. Proses ini akan memakan waktu beberapa menit.

## Task 2: Test and verify deployment of the Azure Container App

Secara default, Azure Container App yang Anda buat akan menerima trafik di port 80 menggunakan aplikasi contoh Hello World. Azure Container Apps akan menyediakan nama DNS untuk aplikasi tersebut. Salin dan buka URL tersebut untuk memastikan aplikasinya berjalan.

1. Pilih **Go to resource** untuk melihat container app yang baru saja dibuat.

2. Klik tautan di samping *Application URL* untuk membuka aplikasi Anda.

    ![Screenshot of the ACA overview page in the portal.](../media/az104-lab09b-aca-overview.png)

3. Verifikasi bahwa Anda menerima pesan **Your Azure Container Apps app is live**.

## Cleanup your resources

Jika Anda menggunakan **langganan milik sendiri**, luangkan waktu untuk menghapus resource lab. Hal ini memastikan resource dibebaskan dan biaya diminimalkan. Cara termudah untuk menghapus semua resource lab adalah dengan menghapus resource group-nya.

+ Di Azure portal, pilih resource group, pilih **Delete the resource group**, **Masukkan nama resource group**, lalu klik **Delete**.
+ Menggunakan Azure PowerShell:  
  ```powershell
  Remove-AzResourceGroup -Name resourceGroupName

## Extend your learning with Copilot

Copilot dapat membantu Anda mempelajari penggunaan alat scripting di Azure. Copilot juga dapat memberikan bantuan pada topik yang tidak tercakup dalam lab atau ketika Anda membutuhkan informasi tambahan. Buka browser Microsoft Edge dan pilih **Copilot** (kanan atas), atau kunjungi *copilot.microsoft.com*. Coba beberapa prompt berikut:

+ Ringkas langkah-langkah untuk membuat dan mengonfigurasi Azure Container App.
+ Bandingkan Azure Container Apps dengan Azure Kubernetes Service.

## Learn more with self-paced training

+ [Configure a container app in Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/)  
  Modul ini membahas fitur dan kapabilitas Azure Container Apps, serta fokus pada pembuatan, konfigurasi, scaling, dan pengelolaan container app menggunakan Azure Container Apps.

## Key takeaways

Selamat, Anda telah menyelesaikan lab ini. Berikut adalah poin-poin utama dari lab ini:

+ Azure Container Apps (ACA) adalah platform serverless yang memungkinkan Anda menjalankan aplikasi berbasis container tanpa harus mengelola infrastruktur secara langsung.
+ Container Apps menyediakan konfigurasi server, orkestrasi container, dan detail deployment sebagai layanan terkelola.
+ Beban kerja yang umum dijalankan di ACA adalah proses jangka panjang seperti aplikasi web (Web App).
