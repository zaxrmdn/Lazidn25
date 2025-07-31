---
lab:
    title: 'Lab 10: Implement Data Protection'
    module: 'Administer Data Protection'
---

# Lab 10 - Implement Data Protection

## Lab introduction    

Dalam lab ini, Anda akan mempelajari tentang pencadangan dan pemulihan mesin virtual Azure. Anda akan belajar membuat Recovery Service vault dan kebijakan pencadangan untuk mesin virtual Azure. Anda juga akan mempelajari tentang pemulihan bencana dengan Azure Site Recovery.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur dalam lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkah ditulis menggunakan **Indonesia Central** dan **Southeast Asia**.

## Estimated timing: 50 minutes

## Lab scenario

Organisasi Anda sedang mengevaluasi cara mencadangkan dan memulihkan mesin virtual Azure dari kehilangan data yang tidak disengaja atau bersifat jahat. Selain itu, organisasi ingin mengeksplorasi penggunaan Azure Site Recovery untuk skenario pemulihan bencana.

## Interactive lab simulations

>**Catatan**: Simulasi lab yang sebelumnya disediakan telah dihentikan.

## Job skills

+ Task 1: Gunakan template untuk menyediakan infrastruktur.
+ Task 2: Buat dan konfigurasikan Recovery Services vault.
+ Task 3: Konfigurasikan backup tingkat mesin virtual Azure.
+ Task 4: Pantau Azure Backup.
+ Task 5: Aktifkan replikasi mesin virtual.

## Estimated timing: 40 minutes

## Architecture diagram

![Diagram of the architecture tasks.](../media/az104-lab10-architecture.png)

## Task 1: Use a template to provision an infrastructure

Pada tugas ini, Anda akan menggunakan template untuk menyebarkan sebuah mesin virtual. Mesin virtual ini akan digunakan untuk menguji berbagai skenario backup.

1. Unduh file lab dari **\\Allfiles\\Lab10\\**.

1. Masuk ke **Azure portal** - https://portal.azure.com.

1. Cari dan pilih `Deploy a custom template`.

1. Pada halaman custom deployment, pilih **Build your own template in the editor**.

1. Pada halaman edit template, pilih **Load file**.

1. Temukan dan pilih file **\\Allfiles\\Lab10\\az104-10-vms-edge-template.json** lalu klik **Open**.

   >**Catatan:** Luangkan waktu untuk meninjau template. Kita akan menyebarkan virtual network dan mesin virtual untuk mendemonstrasikan backup dan pemulihan.

1. Klik **Save**.

1. Pilih **Edit parameters** lalu **Load file**.

1. Muat dan pilih file **\\Allfiles\\Lab10\\az104-10-vms-edge-parameters.json**.

1. Klik **Save**.

1. Gunakan informasi berikut untuk mengisi kolom deployment:

    | Setting       | Value         | 
    | ---           | ---           |
    | Subscription  | Langganan Azure Anda |
    | Resource group| `rg-region-1-p1` (Jika perlu, pilih **Create new**) |
    | Region        | **Indonesia Central**   |
    | Username      | **localadmin**   |
    | Password      | Gunakan sandi kompleks |

1. Pilih **Review + Create**, lalu klik **Create**.

    >**Catatan:** Tunggu hingga template selesai disebarkan, lalu pilih **Go to resource**. Anda akan memiliki satu mesin virtual di satu virtual network.

## Task 2: Create and configure a Recovery Services vault

Pada tugas ini, Anda akan membuat Recovery Services vault. Vault ini akan menjadi tempat penyimpanan data dari mesin virtual.

1. Di Azure portal, cari dan pilih `Recovery Services vaults`, lalu klik **+ Create**.

1. Isi pengaturan berikut:

    | Settings | Value |
    | --- | --- |
    | Subscription | Nama langganan Azure Anda |
    | Resource group | `rg-region-1-p1` |
    | Vault Name | `az104-rsv-region1` |
    | Region | **Indonesia Central** |

    >**Catatan**: Pastikan wilayah sama dengan tempat Anda menyebarkan mesin virtual pada tugas sebelumnya.

1. Klik **Review + Create**, pastikan validasi berhasil, lalu klik **Create**.

1. Setelah penyebaran selesai, klik **Go to Resource**.

1. Di bagian **Settings**, klik **Properties**.

1. Klik tautan **Update** di bawah label **Backup Configuration**.

1. Tinjau opsi **Storage replication type**. Biarkan pada pengaturan default yaitu **Geo-redundant** lalu tutup halaman tersebut.

1. Klik tautan **Update** di bawah **Security Settings > Soft Delete and security settings**.

1. Perhatikan bahwa **Soft Delete** dalam keadaan **Enabled** dengan masa retensi **14 hari**.

>**Tahukah Anda?** Azure memiliki dua jenis vault: Recovery Services vault dan Backup vault. Perbedaannya ada pada jenis sumber data yang dapat dicadangkan. [Pelajari lebih lanjut.](https://learn.microsoft.com/answers/questions/405915/what-is-difference-between-recovery-services-vault)

## Task 3: Configure Azure virtual machine-level backup

Pada tugas ini, Anda akan mengimplementasikan backup tingkat mesin virtual Azure. Anda juga akan membuat kebijakan backup dan retensi.

1. Pada blade Recovery Services vault, klik **Overview**, lalu klik **+ Backup**.

1. Atur konfigurasi berikut:

    | Setting | Value |
    | --- | --- |
    | Where is your workload running? | **Azure** |
    | What do you want to backup? | **Virtual machine** |

1. Klik **Backup**.

1. Pilih subtipe kebijakan **Standard**.

1. Pada **Backup policy**, klik **Create a new policy** lalu isi:

    | Setting | Value |
    | ---- | ---- |
    | Policy name | `az104-backup` |
    | Frequency | **Daily** |
    | Time | **12:00 AM** |
    | Timezone | Zona waktu lokal Anda |
    | Retain instant recovery snapshot(s) for | **2 Days** |

1. Klik **OK** untuk membuat kebijakan. Pada bagian **Virtual Machines**, klik **Add**.

1. Pilih **az-104-10-vm0**, klik **OK**, lalu klik **Enable backup**.

1. Setelah selesai, pilih **Go to resource**.

1. Pada bagian **Protected items**, klik **Backup items**, lalu klik entri **Azure virtual machine**.

1. Klik **View details** untuk **az104-10-vm0** dan tinjau nilai dari **Backup Pre-Check** dan **Last Backup Status**.

1. Klik **Backup now**, biarkan nilai default untuk **Retain Backup Till**, lalu klik **OK**.

## Task 4: Monitor Azure Backup

Pada tugas ini, Anda akan menyebarkan akun penyimpanan Azure dan mengkonfigurasikan vault untuk mengirim log dan metrik ke akun tersebut.

1. Dari portal Azure, cari dan pilih `Storage accounts`.

1. Klik **Create**.

1. Isi dengan data berikut:

    | Setting | Value |
    | --- | --- | 
    | Subscription          | Langganan Anda    |
    | Resource group        | `rg-region-1-p1`        |
    | Storage account name  | Nama unik global   |
    | Region                | **Indonesia Central**   |

1. Klik **Create** dan tunggu penyebaran selesai.

1. Kembali ke Recovery Services vault Anda.

1. Di blade **Monitoring**, pilih **Diagnostic Settings** lalu klik **Add diagnostic setting**.

1. Beri nama: `Logs and Metrics to storage`.

1. Centang kategori berikut:

    - Azure Backup Reporting Data
    - Addon Azure Backup Job Data
    - Addon Azure Backup Alert Data
    - Azure Site Recovery Jobs
    - Azure Site Recovery Events

1. Di bagian tujuan, centang **Archive to a storage account**.

1. Pilih akun penyimpanan yang telah Anda buat, lalu klik **Save**.

1. Kembali ke Recovery Services vault, pada blade **Monitoring**, pilih **Backup jobs**.

1. Cari dan **View details** dari operasi backup untuk mesin virtual **az104-10-vm0**.

## Task 5: Enable virtual machine replication

1. Cari dan pilih `Recovery Services vaults`, lalu klik **+ Create**.

1. Isi pengaturan berikut:

    | Settings | Value |
    | --- | --- |
    | Subscription | Nama langganan Anda |
    | Resource group | `az104-rg-region2` (buat baru jika perlu) |
    | Vault Name | `az104-rsv-region2` |
    | Region | **Southeast Asia** |

1. Klik **Review + Create** lalu **Create**.

1. Cari dan pilih mesin virtual **az104-10-vm0**.

1. Pada blade **Backup + Disaster recovery**, pilih **Disaster recovery**.

1. Tinjau **Target region**.

1. Klik **Next: Advanced settings** dan buat akun otomatisasi jika diminta.

1. Klik **Review + Start replication**, lalu **Enable replication**.

1. Setelah replikasi selesai (sekitar 10–15 menit), cari Recovery Services vault `az104-rsv-region2`.

1. Klik **Protected items** > **Replicated items**.

1. Tinjau status kesehatan replikasi. Status akan berubah menjadi **Protected** setelah sinkronisasi selesai.

1. Klik nama mesin virtual untuk melihat detail.

>**Tahukah Anda?** Sebaiknya lakukan [uji failover VM yang dilindungi](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm).

## Cleanup your resources

Jika Anda menggunakan **langganan Anda sendiri**, pastikan untuk menghapus resource lab agar tidak dikenakan biaya tambahan. Cara termudah adalah dengan menghapus resource group:

+ Di Azure portal: pilih resource group, pilih **Delete**, masukkan namanya, lalu konfirmasi.
+ Menggunakan PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`
+ Menggunakan CLI: `az group delete --name resourceGroupName`

## Extend your learning with Copilot

Copilot dapat membantu Anda mempelajari penggunaan alat skrip Azure. Coba beberapa prompt berikut di *copilot.microsoft.com*:

+ What products does Azure Backup support?
+ Summarize the steps for backing up and restoring an Azure virtual machine with Azure Backup.
+ How can I use Azure PowerShell or the CLI to check the status of an Azure Backup job?
+ Provide at least five best practices for configuring Azure virtual machine backups.  

## Learn more with self-paced training

+ [Protect your virtual machines by using Azure Backup](https://learn.microsoft.com/training/modules/protect-virtual-machines-with-azure-backup/)
+ [Protect your Azure infrastructure with Azure Site Recovery](https://learn.microsoft.com/en-us/training/modules/protect-infrastructure-with-site-recovery/)

## Key takeaways

Selamat, Anda telah menyelesaikan lab ini. Berikut ringkasan hal-hal penting:

+ Azure Backup adalah layanan sederhana, aman, dan hemat biaya untuk mencadangkan dan memulihkan data Anda.
+ Azure Backup dapat melindungi sumber daya on-premises dan cloud termasuk mesin virtual dan file share.
+ Kebijakan backup menentukan frekuensi pencadangan dan masa retensi.
+ Azure Site Recovery adalah solusi pemulihan bencana untuk melindungi VM dan aplikasi Anda.
+ Site Recovery mereplikasi beban kerja ke wilayah sekunder, sehingga Anda dapat melakukan failover dengan downtime minimal.
+ Recovery Services vault menyimpan data backup dan menyederhanakan pengelolaan.
