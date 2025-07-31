---
lab:
    title: 'Lab 09a: Implement Web Apps'
    module: 'Administer PaaS Compute Options'
---

# Lab 09a - Implement Web Apps

## Lab introduction

Dalam lab ini, Anda akan mempelajari tentang Azure Web Apps. Anda akan mengonfigurasi Web App untuk menampilkan aplikasi Hello World dari repositori GitHub eksternal. Anda juga akan membuat *staging slot* dan melakukan *swap* dengan *production slot*. Selain itu, Anda akan mempelajari tentang *autoscaling* untuk mengakomodasi perubahan permintaan.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur dalam lab ini. Anda dapat mengubah region, tetapi langkah-langkah ditulis menggunakan **Indonesia Central**.

## Estimasi waktu: 20 menit

## Lab scenario

Organisasi Anda tertarik dengan Azure Web Apps untuk menghosting situs web perusahaan. Saat ini, situs web dihosting di *on-premises data center* menggunakan Windows Server dan *runtime stack* PHP. Perangkat kerasnya mendekati akhir masa pakai dan perlu diganti. Organisasi Anda ingin menghindari biaya perangkat keras baru dengan memanfaatkan Azure sebagai tempat hosting situs web.

## Interactive lab simulations

>**Catatan**: Simulasi lab sebelumnya sudah tidak lagi disediakan.

## Architecture diagram

![Diagram of the tasks.](../media/az104-lab09a-architecture.png)

## Job skills

+ Task 1: Membuat dan mengonfigurasi Azure Web App
+ Task 2: Membuat dan mengonfigurasi *deployment slot*
+ Task 3: Mengonfigurasi pengaturan *deployment* untuk Web App
+ Task 4: Melakukan *swap* antara deployment slots
+ Task 5: Mengonfigurasi dan menguji *autoscaling* pada Azure Web App

## Task 1: Create and configure an Azure web app

1. Masuk ke **Azure portal** - https://portal.azure.com.
2. Cari dan pilih **App Services**.
3. Klik **+ Create**, lalu pilih **Web App**.
4. Pada tab **Basics** di blade **Create Web App**, isikan pengaturan berikut (biarkan nilai lainnya default):

    | Pengaturan | Nilai |
    |------------|-------|
    | Subscription | langganan Azure Anda |
    | Resource group | `rg9-p1` (jika perlu, klik **Create new**) |
    | Web app name | nama unik global |
    | Publish | **Code** |
    | Runtime stack | **PHP 8.2** |
    | Operating system | **Linux** |
    | Region | **Indonesia Central** |
    | Pricing plans | **Premium V3 P1V3** |
    | Zone redundancy | gunakan default |

5. Klik **Review + create**, lalu **Create**.

   >**Catatan**: Tunggu hingga Web App selesai dibuat sebelum melanjutkan. Biasanya memerlukan waktu sekitar satu menit.

   >**Catatan**: Jika gagal, coba gunakan region lain karena perbedaan kuota di tiap region.

6. Setelah deployment selesai, klik **Go to resource**.

## Task 2: Create and configure a deployment slot

1. Pada blade Web App yang baru saja dibuat, klik link **Default domain** untuk membuka laman default di tab baru.
2. Tutup tab tersebut, kembali ke portal, buka bagian **Deployment** di blade Web App, klik **Deployment slots**.
3. Klik **Add slot**, isi pengaturan berikut:

    | Pengaturan | Nilai |
    |------------|-------|
    | Name | `staging` |
    | Clone settings from | **Do not clone settings** |

4. Klik **Add** untuk membuat slot baru.
5. Refresh halaman untuk melihat slot Production dan Staging.
6. Klik entri slot staging yang baru dibuat.

   >**Catatan**: Ini akan membuka blade properti dari staging slot.

7. Amati URL dari staging slot yang berbeda dengan URL production slot.

## Task 3: Configure Web App deployment settings

1. Pada staging slot, buka **Deployment Center** lalu pilih **Settings**.

   >**Catatan**: Pastikan Anda sedang berada di blade staging slot, bukan production slot.

2. Pada dropdown **Source**, pilih **External Git**.
3. Di kolom repository, masukkan:  
   `https://github.com/Azure-Samples/php-docs-hello-world`
4. Di kolom branch, masukkan:  
   `master`
5. Klik **Save**.
6. Buka **Overview** lalu klik link **Default domain**, buka di tab baru.
7. Verifikasi bahwa halaman menampilkan teks **Hello World**.

   >**Catatan**: Deployment mungkin membutuhkan waktu satu menit. Lakukan **Refresh** jika perlu.

## Task 4: Swap deployment slots

1. Kembali ke blade **Deployment slots**, lalu klik **Swap**.
2. Tinjau pengaturan default dan klik **Start Swap**. Tunggu hingga muncul notifikasi bahwa swap selesai.
3. Kembali ke halaman utama portal. Anda akan memiliki web app produksi dan staging slot.
4. Cari App Services dan pilih App Service Web App Anda.
5. Pada blade **Overview**, klik link **Default domain**.
6. Verifikasi halaman produksi sekarang menampilkan **Hello World!**.

   >**Catatan**: Salin **URL Default domain** untuk digunakan dalam pengujian beban (*load test*) pada task berikutnya.

## Task 5: Configure and test autoscaling of the Azure Web App

1. Pada bagian **Settings**, pilih **Scale out (App Service plan)**.

   >**Catatan**: Pastikan Anda sedang bekerja di production slot.

2. Pada bagian **Scaling**, pilih **Automatic**.
3. Pada kolom **Maximum burst**, masukkan nilai: **2**.

    ![Screenshot of the autoscale page.](../media/az104-lab09a-autoscale.png)

4. Klik **Save**.
5. Klik **Diagnose and solve problems** di panel kiri.
6. Pada bagian **Load Test your App**, klik **Create Load Test**.

    + Klik **+ Create** dan beri nama unik untuk load test.
    + Klik **Review + create**, lalu **Create**.

7. Setelah selesai dibuat, klik **Go to resource**.
8. Dari tab **Overview** | **Create by adding HTTP requests**, klik **Create**.
9. Pada tab **Test plan**, klik **Add request**. Di kolom **URL**, tempelkan **Default domain URL** Anda (pastikan format **https://**). Klik **Add**.
10. Klik **Review + create** lalu **Create**.

    >**Catatan**: Dibutuhkan beberapa menit untuk membuat tes. Lihat notifikasi.

11. Navigasi ke daftar tes (ada di halaman utama).
12. Klik **Refresh** dan tinjau metrik langsung seperti **Virtual users**, **Response time**, dan **Requests/sec**.
13. Klik **Stop** untuk menyelesaikan uji beban. Tidak perlu menunggu hingga selesai sepenuhnya.

## Cleanup your resources

Jika Anda menggunakan **langganan pribadi**, hapus resource lab untuk menghindari biaya tambahan. Cara termudah adalah menghapus resource group:

+ Di Azure portal: pilih resource group > klik **Delete the resource group** > ketik nama resource group > klik **Delete**.
+ Azure PowerShell:  
  `Remove-AzResourceGroup -Name resourceGroupName`
+ Azure CLI:  
  `az group delete --name resourceGroupName`

## Extend your learning with Copilot

Copilot dapat membantu Anda mempelajari penggunaan alat scripting Azure. Copilot juga dapat membantu di area yang tidak dibahas di lab. Coba buka Edge browser, pilih Copilot (pojok kanan atas) atau akses *copilot.microsoft.com* dan coba beberapa perintah berikut:

+ Ringkas langkah-langkah untuk membuat dan mengonfigurasi Azure Web App.
+ Apa saja cara saya dapat menskalakan Azure Web App?

## Learn more with self-paced training

+ [Stage a web app deployment for testing and rollback by using App Service deployment slots](https://learn.microsoft.com/training/modules/stage-deploy-app-service-deployment-slots/)
+ [Scale an App Service web app to efficiently meet demand with App Service scale up and scale out](https://learn.microsoft.com/training/modules/app-service-scale-up-scale-out/)

## Key takeaways