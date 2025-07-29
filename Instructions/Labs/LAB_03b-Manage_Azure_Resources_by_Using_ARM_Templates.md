---
lab:
    title: 'Lab 03: Manage Azure resources by using Azure Resource Manager Templates'
    module: 'Administer Azure Resources'
---

# Lab 03 - Manage Azure resources by using Azure Resource Manager Templates

## Lab introduction

Dalam lab ini, kamu akan mempelajari cara mengotomatisasi deployment resource. Kamu akan belajar tentang Azure Resource Manager templates dan Bicep templates. Kamu juga akan mempelajari berbagai cara untuk melakukan deployment template.

Lab ini membutuhkan langganan Azure. Jenis langgananmu dapat memengaruhi fitur yang tersedia di lab ini. Kamu bisa mengubah region, namun langkah-langkah ditulis menggunakan **Indonesia Central**.

## Estimated timing: 50 minutes

## Interactive lab simulations

>**Catatan**: Simulasi lab sebelumnya sudah tidak tersedia.

## Lab scenario

Tim kamu ingin mengeksplorasi cara untuk mengotomatisasi dan menyederhanakan deployment resource. Organisasi kamu ingin mengurangi beban administratif, mengurangi kesalahan manusia, dan meningkatkan konsistensi.

## Architecture diagram

![Diagram of the tasks.](../media/az104-lab03-architecture.png)

## Job skills

+ Task 1: Membuat Azure Resource Manager template.
+ Task 2: Mengedit Azure Resource Manager template dan melakukan redeploy template.
+ Task 3: Mengonfigurasi Cloud Shell dan melakukan deployment template dengan Azure PowerShell.
+ Task 4: Melakukan deployment template dengan CLI.
+ Task 5: Melakukan deployment resource menggunakan Azure Bicep.

---

## Task 1: Create an Azure Resource Manager template

1. Masuk ke **Azure portal** - `https://portal.azure.com`.

2. Cari dan pilih `Disks`.

3. Pada halaman Disks, pilih **Create**.

4. Di halaman **Create a managed disk**, konfigurasikan pengaturannya:

    | Setting | Value |
    | --- | --- |
    | Subscription | *your subscription* | 
    | Resource Group | `rg3-p1` |
    | Disk name | `p1-disk1` | 
    | Region | **Indonesia Central** |
    | Availability zone | **No infrastructure redundancy required** | 
    | Source type | **None** |
    | Performance | **Standard HDD** |
    | Size | **32 GiB** |

5. Klik **Review + Create**, lalu pilih **Create**.

6. Setelah deployment selesai, pilih **Go to resource**.

7. Di bagian **Automation**, pilih **Export template**.

8. Tinjau file **Template** dan **Parameters**.

9. Klik **Download** untuk menyimpan template (dalam file zip).

10. Ekstrak isi file ke folder **Downloads** di komputermu.

---

## Task 2: Edit an Azure Resource Manager template dan redeploy

1. Di Azure portal, cari dan pilih `Deploy a custom template`.

2. Di panel **Custom deployment**, pilih **Build your own template in the editor**.

3. Klik **Load file** dan unggah file `template.json`.

4. Lakukan perubahan berikut:

    + Ganti `disks_disk1_name` menjadi `disk_name` (dua tempat).
    + Ganti `p1-disk1` menjadi `p1-disk2`.

5. Simpan perubahan (klik **Save**).

6. Edit parameter dengan klik **Edit parameters** → **Load file** → upload `parameters.json`.

7. Ganti `disks_disk1_name` menjadi `disk_name`.

8. Simpan perubahan dan lanjutkan deployment:

    | Setting | Value |
    | --- |--- |
    | Subscription | *your subscription* |
    | Resource Group | `rg3-p1` |
    | Region | **Indonesia Central** |
    | Disk_name | `p1-disk2` |

9. Klik **Review + Create** lalu **Create**.

10. Pastikan **p1-disk2** telah dibuat di **rg3-p1**.

11. Buka **Deployments** pada resource group tersebut untuk memverifikasi.

---

## Task 3: Configure Cloud Shell dan deploy template dengan PowerShell

1. Klik ikon **Cloud Shell** di pojok kanan atas Azure Portal atau buka `https://shell.azure.com`.

2. Pilih **PowerShell** saat diminta.

3. Klik **Mount storage account**, pilih subscription kamu, lalu klik **Apply**.

4. Pilih **Create a storage account** dan lengkapi:

    | Setting | Value |
    | -- | -- |
    | Resource Group | `rg3-p1` |
    | Region | *your region* |
    | Storage account | *unik (lowercase dan angka, 3-24 karakter)* |
    | File share | `fs-cloudshell` |

5. Setelah selesai, upload file `template.json` dan `parameters.json`.

6. Buka file di editor (ikon `{}`) dan ubah nama disk menjadi `p1-disk3`, simpan dengan **Ctrl + S**.

7. Jalankan perintah berikut:

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName rg3-p1 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```

8. Verifikasi disk:

    ```powershell
    Get-AzDisk
    ```

---

## Task 4: Deploy template dengan CLI

1. Di **Cloud Shell**, pilih **Bash**.

2. Cek file:

    ```bash
    ls
    ```

3. Edit `template.json`, ubah nama disk ke `p1-disk4`, lalu simpan.

4. Jalankan deployment:

    ```bash
    az deployment group create --resource-group rg3-p1 --template-file template.json --parameters parameters.json
    ```

5. Verifikasi disk:

    ```bash
    az disk list --output table
    ```

---

## Task 5: Deploy resource dengan Azure Bicep

1. Temukan file `azuredeploydisk.bicep` di folder `\Allfiles\Lab03\`.

2. Di **Cloud Shell** (Bash), upload file tersebut.

3. Buka editor dan ubah:

    + `managedDiskName`: `p1-disk5`
    + `sku name`: `StandardSSD_LRS`
    + `diskSizeinGiB`: `32`

4. Simpan dengan **Ctrl + S**.

5. Jalankan deployment:

    ```bash
    az deployment group create --resource-group rg3-p1 --template-file azuredeploydisk.bicep
    ```

6. Verifikasi disk:

    ```bash
    az disk list --output table
    ```

---

## Cleanup your resources

Jika menggunakan langganan sendiri, hapus resource group:

+ Azure portal: pilih resource group → **Delete**.
+ PowerShell:

    ```powershell
    Remove-AzResourceGroup -Name rg3-p1
    ```

+ CLI:

    ```bash
    az group delete --name rg3-p1
    ```

---

## Extend your learning with Copilot

Coba gunakan prompt berikut di Copilot:

+ Apa format file Azure Resource Manager template?
+ Bagaimana cara menggunakan template ARM yang sudah ada?
+ Bandingkan ARM template dan Bicep template.

---

## Learn more with self-paced training

+ [Deploy Azure infrastructure by using JSON ARM templates](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)
+ [Review the features and tools for Azure Cloud Shell](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/)
+ [Manage Azure resources with Windows PowerShell](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/)
+ [Introduction to Bash](https://learn.microsoft.com/training/modules/bash-introduction/)
+ [Build your first Bicep template](https://learn.microsoft.com/training/modules/build-first-bicep-template/)

---

## Key takeaways

+ ARM template memungkinkan deployment dan manajemen resource secara deklaratif.
+ File ARM berbentuk JSON dan dapat menggunakan file parameter terpisah.
+ ARM template dapat di-deploy lewat portal, PowerShell, dan CLI.
+ Bicep adalah alternatif ARM yang lebih ringkas, type-safe, dan reusable.
+ Kamu berhasil deploy lima disk dengan lima metode berbeda. Kerja bagus!
