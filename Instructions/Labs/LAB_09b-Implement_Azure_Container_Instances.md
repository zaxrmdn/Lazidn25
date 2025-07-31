---
lab:
    title: 'Lab 09b: Implement Azure Container Instances'
    module: 'Administer PaaS Compute Options'
---

# Lab 09b - Implement Azure Container Instances

## Lab introduction

Dalam lab ini, Anda akan mempelajari cara mengimplementasikan dan melakukan deployment Azure Container Instances.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur dalam lab ini. Anda boleh mengganti region, tetapi langkah-langkah ditulis menggunakan **Indonesia Central**.

## Estimated timing: 15 minutes

## Lab scenario

Organisasi Anda memiliki aplikasi web yang berjalan pada virtual machine di data center lokal. Organisasi ingin memindahkan semua aplikasi ke cloud tanpa harus mengelola banyak server. Anda memutuskan untuk mengevaluasi Azure Container Instances dan Docker.

## Interactive lab simulations

>**Catatan**: Simulasi lab yang sebelumnya tersedia telah dihentikan.

## Architecture diagram

![Diagram of the tasks.](../media/az104-lab09b-aci-architecture.png)

## Job skills

- Task 1: Deploy an Azure Container Instance menggunakan Docker image.
- Task 2: Menguji dan memverifikasi deployment Azure Container Instance.

## Task 1: Deploy an Azure Container Instance menggunakan Docker image

Pada task ini, Anda akan membuat aplikasi web sederhana menggunakan Docker image. Docker adalah platform yang memungkinkan Anda mengemas dan menjalankan aplikasi dalam lingkungan terisolasi yang disebut container. Azure Container Instances menyediakan lingkungan komputasi untuk container image tersebut.

1. Masuk ke **Azure portal** - `https://portal.azure.com`.

2. Di Azure portal, cari dan pilih `Container instances`, lalu pada blade **Container instances**, klik **+ Create**.

3. Pada tab **Basics** di blade **Create container instance**, isi pengaturan berikut (biarkan lainnya default):

    | Setting | Value |
    | ------- | ----- |
    | Subscription | Pilih langganan Azure Anda |
    | Resource group | `rg9-p1` (Jika perlu, pilih **Create new**) |
    | Container name | `c1` |
    | Region | **Indonesia Central** (atau region lain yang tersedia) |
    | Image Source | **Quickstart images** |
    | Image | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

4. Klik **Next: Networking >** dan isi pengaturan berikut (biarkan lainnya default):

    | Setting | Value |
    | ------- | ----- |
    | DNS name label | nama host DNS unik secara global dan valid |

    >**Catatan**: Container Anda akan dapat diakses secara publik di dns-name-label.region.azurecontainer.io. Jika Anda menerima pesan kesalahan **DNS name label not available**, gunakan nilai lain.

5. Klik **Next: Monitoring >**, lalu hapus centang **Enable container instance logs**.

6. Klik **Next: Advanced >**, tinjau pengaturannya tanpa melakukan perubahan.

7. Klik **Review + Create**, pastikan validasi berhasil, lalu pilih **Create**.

    >**Catatan**: Tunggu hingga deployment selesai. Biasanya memakan waktu 2–3 menit.

    >**Catatan**: Sambil menunggu, Anda dapat melihat [kode sumber aplikasi contoh](https://github.com/Azure-Samples/aci-helloworld). Untuk melihat kodenya, buka folder `\app`.

## Task 2: Menguji dan memverifikasi deployment Azure Container Instance

Dalam task ini, Anda akan meninjau deployment container instance. Secara default, Azure Container Instance dapat diakses melalui port 80. Setelah instance berhasil dibuat, Anda dapat mengaksesnya menggunakan DNS name yang telah Anda tentukan sebelumnya.

1. Setelah deployment selesai, klik tautan **Go to resource**.

2. Di blade **Overview** dari container instance, pastikan nilai **Status** adalah **Running**.

3. Salin nilai **FQDN** dari container instance, buka tab browser baru, lalu navigasikan ke URL tersebut.

    ![Screenshot of the ACI overview page in the portal.](../media/az104-lab09b-aci-overview.png)

4. Pastikan halaman **Welcome to Azure Container Instance** tampil. Refresh beberapa kali untuk menghasilkan log, lalu tutup tab browser tersebut.

5. Di bagian **Settings** dari blade container instance, klik **Containers**, lalu klik **Logs**.

6. Pastikan Anda melihat entri log dari HTTP GET request yang dihasilkan saat membuka aplikasi melalui browser.

## Cleanup your resources

Jika Anda menggunakan **langganan sendiri**, luangkan waktu untuk menghapus resource dari lab ini. Ini akan membebaskan resource dan meminimalkan biaya. Cara termudah adalah dengan menghapus resource group lab.

+ Di Azure portal, pilih resource group, klik **Delete the resource group**, masukkan nama resource group, lalu klik **Delete**.
+ Menggunakan Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`
+ Menggunakan CLI: `az group delete --name resourceGroupName`

## Extend your learning with Copilot

Copilot dapat membantu Anda mempelajari cara menggunakan alat skrip Azure. Copilot juga dapat membantu pada bagian yang tidak dibahas dalam lab ini atau saat Anda membutuhkan informasi tambahan. Buka browser Edge dan klik Copilot (kanan atas), atau kunjungi *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah berikut:

+ Ringkas langkah-langkah untuk membuat dan mengonfigurasi Azure Container Instance.
+ Apa saja cara menjalankan container serverless di Azure?

## Learn more with self-paced training

+ [Run container images in Azure Container Instances](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/). Pelajari bagaimana Azure Container Instances dapat membantu Anda melakukan deployment container dengan cepat, mengatur environment variable, dan menentukan kebijakan restart container.

## Key takeaways

Selamat, Anda telah menyelesaikan lab ini. Berikut poin-poin penting yang perlu Anda ingat:

+ Azure Container Instances (ACI) adalah layanan yang memungkinkan Anda menjalankan container di public cloud Microsoft Azure.
+ ACI tidak memerlukan provisioning atau pengelolaan infrastruktur di belakang layar.
+ ACI mendukung container berbasis Linux maupun Windows.
+ Workload di ACI umumnya dijalankan berdasarkan pemicu tertentu dan bersifat sementara.
