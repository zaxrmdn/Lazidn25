---
lab:
    title: 'Lab 07: Manage Azure storage'
    module: 'Administer Azure Storage'
---

# Lab 07 - Manage Azure Storage

## Lab introduction

Dalam lab ini Anda akan mempelajari cara membuat storage account untuk Azure blobs dan Azure files. Anda akan belajar cara mengonfigurasi dan mengamankan blob containers. Anda juga belajar menggunakan Storage Browser untuk mengonfigurasi dan mengamankan Azure file shares.

Lab ini membutuhkan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur dalam lab ini. Anda boleh mengganti wilayah, namun langkah‑langkah ditulis menggunakan **Indonesia Central**.

## Estimated timing: 50 minutes

## Lab scenario

Organisasi Anda saat ini menyimpan data di penyimpanan on‑premises. Sebagian besar file jarang diakses. Anda ingin meminimalkan biaya penyimpanan dengan memindahkan file yang jarang diakses ke tier storage dengan harga lebih rendah. Anda juga berencana mengeksplorasi berbagai mekanisme proteksi yang tersedia di Azure Storage, termasuk akses jaringan, autentikasi, otorisasi, dan replikasi. Akhirnya, Anda ingin mengetahui seberapa cocok Azure Files sebagai host untuk file shares on‑premises Anda.

## Interactive lab simulations

>**Catatan**: Simulasi lab interaktif sebelumnya telah dihentikan.

## Architecture diagram

![Diagram of the tasks.](../media/az104-lab07-architecture.png)

## Job skills

+ Task 1: Create and configure a storage account.  
+ Task 2: Create and configure secure blob storage.  
+ Task 3: Create and configure secure Azure file storage.

## Task 1: Create and configure a storage account.

Dalam tugas ini Anda akan membuat dan mengonfigurasi storage account. Storage account akan menggunakan geo-redundant storage dan tanpa akses publik.

1. Masuk ke **Azure portal** – `https://portal.azure.com`.  
2. Cari dan pilih `Storage accounts`, lalu klik **+ Create**.  
3. Pada tab **Basics**, isi pengaturan berikut (biarkan sisa default):

    | Setting | Value |
    |--------|-------|
    | Subscription | nama langganan Azure Anda |
    | Resource group | **rg7-p1** (buat baru) |
    | Storage account name | nama unik global, 3–24 karakter huruf & angka |
    | Region | **Indonesia Central** |
    | Performance | **Standard** |
    | Redundancy | **Geo-redundant storage** |
    | Make read access… | Centang kotak |

    >**Catatan**: Gunakan tier Standard untuk aplikasi umum, Premium untuk kebutuhan performa tinggi.

4. Pada tab **Advanced**, baca info tambahan, biarkan default.  
5. Pada tab **Networking**, di bagian **Network access**, pilih **Disable public access and use private access**.  
6. Tinjau tab **Data protection**—retensi soft delete default 7 hari, dan versi blob bisa diaktifkan. Biarkan default.  
7. Tinjau tab **Encryption**, biarkan default.  
8. Klik **Review + Create**, tunggu validasi selesai, lalu klik **Create**.  
9. Setelah selesai, klik **Go to resource**.  
10. Tinjau blade **Overview** dan konfigurasi global lainnya. Perhatikan bahwa storage account bisa dipakai untuk Blob, File shares, Queues, dan Tables.  
11. Di blade **Security + networking**, pilih **Networking**. 
    + Perhatikan **Public network access** yang dinonaktifkan.  
    + Ubah ke **Enable from selected networks and IP addresses**.  
    + Di bagian **Firewall**, centang untuk **Add your client IP address**.  
    + Simpan perubahan.  
12. Di blade **Data management**, pilih **Redundancy**, perhatikan lokasi pusat data primer dan sekunder.  
13. Di blade **Data management**, pilih **Lifecycle management**, lalu klik **Add a rule**.

    + Namai rule `Movetocool`. Perhatikan pilihan cakupan rule. Klik **Next**.  
    + Di tab **Base blobs**, jika blob tidak diubah lebih dari `30 days` lalu **Move to cool storage**.  
    + Anda bisa menjelajahi kondisi lain. Setelah selesai, klik **Add**.

    ![Screenshot move to cool rule conditions.](../media/az104-lab07-movetocool.png)

## Task 2: Create and configure secure blob storage

Anda akan membuat container blob dan meng-upload file gambar. Blob container adalah struktur seperti direktori untuk menyimpan data tidak terstruktur.

### Create a blob container and a time-based retention policy

1. Di portal Azure, di dalam storage account Anda, pilih blade **Data storage** → **Containers**.  
2. Klik **+ Add container** dan buat container dengan:

    | Setting | Value |
    |---------|-------|
    | Name    | `data` |
    | Public access level | Private |

    ![Screenshot of create a container.](../media/az104-lab07-create-container.png)

3. Pada container, klik elipsis (…) di kanan, pilih **Access Policy**.  
4. Di area **Immutable blob storage**, klik **Add policy**:

    | Setting | Value |
    |---------|-------|
    | Policy type | Time-based retention |
    | Set retention period | 180 hari |

5. Klik **Save**.

### Manage blob uploads

1. Kembali ke halaman containers, pilih container **data**, lalu klik **Upload**.  
2. Di blade **Upload blob**, buka bagian **Advanced**.

    >**Catatan**: Pilih file kecil (misalnya dari direktori AllFiles).

    | Setting | Value |
    |---------|-------|
    | Browse for files | pilih file untuk di-upload |
    | Blob type | Block blob |
    | Block size | 4 MiB |
    | Access tier | Hot |
    | Upload to folder | `securitytest` |
    | Encryption scope | Gunakan default container scope |

3. Klik **Upload**.  
4. Konfirmasi folder baru dan file sudah ter-upload.  
5. Pilih file yang di-upload dan tinjau opsi seperti **Download**, **Delete**, **Change tier**, **Acquire lease**.  
6. Salin URL file (dari blade Properties), lalu buka di jendela browser InPrivate.  
7. Anda akan melihat pesan XML seperti **ResourceNotFound** atau **PublicAccessNotPermitted**.

    >**Catatan**: Ini normal karena container diatur sebagai private.

### Configure limited access to the blob storage

1. Kembali ke file yang di-upload, klik elipsis (…) → pilih **Generate SAS**, isi:

    | Setting | Value |
    |---------|-------|
    | Signing key | Key 1 |
    | Permissions | Read |
    | Start date | tanggal kemarin |
    | Start time | waktu sekarang |
    | Expiry date | tanggal besok |
    | Expiry time | waktu sekarang |
    | Allowed IP addresses | kosong |

2. Klik **Generate SAS token and URL**.  
3. Salin **Blob SAS URL** ke clipboard.  
4. Buka jendela InPrivate baru, tempel dan buka URL tersebut.

    >**Catatan**: Anda seharusnya dapat melihat isi file tersebut.

## Task 3: Create and configure an Azure File storage

Anda akan membuat dan mengonfigurasi file share serta menggunakan Storage Browser untuk mengelolanya.

### Create the file share and upload a file

1. Di portal Azure, kembali ke storage account Anda, di blade **Data storage**, pilih **File shares**.  
2. Klik **+ File share**, beri nama `share1`.  
3. Perhatikan opsi **Access tier**, biarkan default (Transaction optimized).  
   Di tab **Backup**, pastikan **Enable backup** tidak dicentang.  
4. Klik **Review + create**, lalu **Create**. Tunggu file share selesai dibuat.

    ![Screenshot of the create file share page.](../media/az104-lab07-create-share.png)

### Explore Storage Browser and upload a file

1. Kembali ke storage account, pilih **Storage browser**.  
2. Pilih **File shares** dan pastikan direktori `share1` muncul.  
3. Pilih direktori `share1`, Anda bisa **+ Add directory**.  
4. Klik **Upload**, pilih file, lalu klik **Upload**.

    >**Catatan**: Anda dapat melihat dan mengelola file share tanpa pembatasan di Storage Browser.

### Restrict network access to the storage account

1. Di portal, cari dan pilih **Virtual networks**.  
2. Klik **+ Create**, pilih resource group Anda, beri nama `vnet1`.  
3. Gunakan default setting lainnya, klik **Review + create**, lalu **Create**.  
4. Setelah selesai, klik **Go to resource**.  
5. Di bagian **Settings**, pilih blade **Service endpoints**:  
    + Klik **Add**.  
    + Pada drop‑down **Services**, pilih **Microsoft.Storage**.  
    + Pada drop‑down **Subnets**, centang subnet **Default**.  
    + Klik **Add**.  
6. Kembali ke storage account Anda.  
7. Di blade **Security + networking**, pilih **Networking**.  
8. Pilih **Add existing virtual network**, pilih `vnet1` dan subnet `default`, lalu klik **Add**.  
9. Di bagian **Firewall**, hapus alamat IP mesin Anda. Trafik diperbolehkan hanya dari virtual network.  
10. Simpan perubahan.  

    >**Catatan**: Sekarang storage account hanya bisa diakses dari virtual network yang dibuat.  
11. Buka **Storage browser**, klik **Refresh**, lalu navigasi ke file share atau blob.  

    >**Catatan**: Anda akan menerima pesan *not authorized to perform this operation* karena tidak sedang dari virtual network.

    ![Screenshot unauthorized access.](../media/az104-lab07-notauthorized.png)

## Cleanup your resources

Jika menggunakan langganan pribadi, hapus resource untuk menghindari biaya:

- Di portal: buka resource group, klik **Delete the resource group**, ketik nama resource group, lalu klik **Delete**.  
- PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`  
- CLI: `az group delete --name resourceGroupName`

## Extend your learning with Copilot

Coba prompt berikut di *copilot.microsoft.com*:

+ Sediakan skrip PowerShell Azure untuk membuat storage account dengan container blob.  
+ Buat checklist untuk memastikan storage account Azure Anda aman.  
+ Buat tabel perbandingan model redudansi Azure storage.

## Learn more with self‑paced training

+ [Optimize your cost with Azure Blob Storage](https://learn.microsoft.com/training/modules/optimize-your-cost-azure-blob-storage/)  
+ [Control access to Azure Storage with shared access signatures](https://learn.microsoft.com/training/modules/control-access-to-azure-storage-with-sas/)

## Key takeaways

+ Azure storage account mencakup objek penyimpanan: blobs, file shares, queues, dan tables. Memberi namespace unik yang dapat diakses dari mana saja via HTTP/HTTPS.  
+ Azure storage menawarkan beberapa model redudansi seperti LRS, ZRS, dan GRS.  
+ Azure blob storage memungkinkan penyimpanan data tidak terstruktur dalam jumlah besar—misalnya gambar atau multimedia.  
+ Azure File Storage menyediakan shared storage untuk data terstruktur yang dapat diatur dalam folder.  
+ Immutable storage memungkinkan penyimpanan dalam status write once, read many (WORM). Kebijakan bisa berbasis waktu atau legal-hold.
