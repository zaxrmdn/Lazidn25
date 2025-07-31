---
lab:
    title: 'Lab 08: Manage Virtual Machines'
    module: 'Administer Virtual Machines'
---

# Lab 08 - Manage Virtual Machines

## Lab introduction

Pada lab ini, kamu akan membuat dan membandingkan Virtual Machine (VM) dengan Virtual Machine Scale Set. Kamu akan mempelajari cara membuat, mengkonfigurasi, dan mengubah ukuran satu VM, serta membuat dan mengkonfigurasi Virtual Machine Scale Set dengan autoscaling.

Lab ini memerlukan langganan Azure. Jenis langganan kamu mungkin mempengaruhi ketersediaan fitur. Langkah-langkah dalam lab ini menggunakan wilayah **Indonesia Central**.

## Estimasi waktu: 50 menit

## Lab scenario

Organisasi kamu ingin mengeksplorasi penerapan dan konfigurasi VM di Azure. Pertama, kamu akan membuat VM dengan skala manual, kemudian membuat Virtual Machine Scale Set dan mengeksplorasi fitur autoscaling.

## Interactive lab simulations

>**Catatan**: Simulasi lab yang sebelumnya tersedia sudah dihentikan.

## Job skills

+ Task 1: Deploy VM Azure yang tahan terhadap zona menggunakan Azure portal.
+ Task 2: Kelola skala compute dan storage untuk VM.
+ Task 3: Buat dan konfigurasikan Azure Virtual Machine Scale Sets.
+ Task 4: Lakukan skala pada Virtual Machine Scale Sets.
+ Task 5: Buat VM menggunakan Azure PowerShell (opsional 1).
+ Task 6: Buat VM menggunakan Azure CLI (opsional 2).

## Azure Virtual Machines Architecture Diagram

![Diagram arsitektur VM.](../media/az104-lab08-vm-architecture.png)

## Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal

Dalam tugas ini, kamu akan membuat dua VM Azure di dua Availability Zone yang berbeda menggunakan Azure portal. Availability Zone menawarkan SLA uptime tertinggi sebesar 99,99%. Untuk memenuhi SLA ini, minimal kamu harus menerapkan dua VM di dua zone berbeda.

1. Masuk ke Azure portal - `https://portal.azure.com`.

2. Cari dan pilih `Virtual machines`, kemudian klik **+ Create**, pilih **Azure virtual machine**.

3. Di tab **Basics**, pada menu drop-down **Availability zone**, centang **Zone 2**. Ini otomatis akan memilih **Zone 1** dan **Zone 2**.

   >**Catatan**: Ini akan menerapkan dua VM, masing-masing di zone berbeda. Hal ini memungkinkan SLA 99,99%. Meskipun hanya memerlukan satu VM, sebaiknya tetap memilih zone yang berbeda.

4. Lanjutkan mengisi pengaturan di tab **Basics**:

    | Pengaturan | Nilai |
    |------------|-------|
    | Subscription | Nama langganan Azure kamu |
    | Resource group | `rg8-p1` (buat baru jika perlu) |
    | Virtual machine names | `vm1` dan `vm2` (edit nama setelah memilih kedua zone) |
    | Region | `Indonesia Central` |
    | Availability options | `Availability zone` |
    | Availability zone | `Zone 1, 2` |
    | Security type | `Standard` |
    | Image | `Windows Server 2019 Datacenter - x64 Gen2` |
    | Azure Spot instance | Tidak dicentang |
    | Size | `Standard D2s v3` |
    | Username | `localadmin` |
    | Password | Buat password aman |
    | Public inbound ports | `None` |
    | Existing Windows Server license | Tidak dicentang |

    ![Tampilan halaman create VM](../media/az104-lab08-create-vm.png)

5. Klik **Next: Disks >**, atur pengaturan berikut:

    | Pengaturan | Nilai |
    |------------|-------|
    | OS disk type | `Premium SSD` |
    | Delete with VM | Dicentang |
    | Enable Ultra Disk compatibility | Tidak dicentang |

6. Klik **Next: Networking >**, gunakan pengaturan default, tanpa load balancer.

    | Pengaturan | Nilai |
    |------------|-------|
    | Delete public IP and NIC when VM is deleted | Dicentang |
    | Load balancing options | `None` |

7. Klik **Next: Management >**, atur:

    | Pengaturan | Nilai |
    |------------|-------|
    | Patch orchestration options | `Azure orchestrated` |

8. Klik **Next: Monitoring >**, atur:

    | Pengaturan | Nilai |
    |------------|-------|
    | Boot diagnostics | `Disable` |

9. Klik **Next: Advanced >**, biarkan default, lalu klik **Review + Create**.

10. Setelah validasi selesai, klik **Create**.

    >**Catatan**: Saat VM dibuat, resource seperti NIC, disk, dan public IP dibuat secara terpisah.

11. Tunggu hingga deployment selesai, lalu klik **Go to resource**.

## Task 2: Manage compute and storage scaling for virtual machines

Dalam tugas ini, kamu akan melakukan scaling VM dengan mengganti SKU (ukuran). Kamu juga akan menambahkan disk baru dan meningkatkan performa disk.

1. Buka VM `vm1`, lalu ke menu **Availability + scale**, pilih **Size**.

2. Pilih ukuran **D2ds_v4**, lalu klik **Resize**. Konfirmasi perubahan.

   >**Catatan**: Jika D2ds_v4 tidak tersedia, pilih yang lain. Proses ini disebut vertical scaling.

    ![Resize VM](../media/az104-lab08-resize-vm.png)

3. Buka menu **Disks**.

4. Di bagian **Data disks**, pilih **+ Create and attach a new disk**, isi:

    | Pengaturan | Nilai |
    |------------|-------|
    | Disk name | `vm1-disk1` |
    | Storage type | `Standard HDD` |
    | Size (GiB) | `32` |

5. Klik **Apply**.

6. Setelah disk selesai dibuat, klik **Detach**, lalu klik **Apply**.

   >**Catatan**: Disk tetap tersimpan meski tidak terpasang pada VM.

7. Cari dan pilih `Disks`, pilih **vm1-disk1**.

8. Pada **Settings**, pilih **Size + performance**.

9. Ganti ke **Standard SSD**, lalu klik **Save**.

10. Kembali ke VM `vm1`, buka **Disks**, lalu pilih **Attach existing disks**.

11. Pilih `vm1-disk1`, klik **Apply**.

## Azure Virtual Machine Scale Sets Architecture Diagram

![Diagram arsitektur VMSS](../media/az104-lab08-vmss-architecture.png)

## Task 3: Create and configure Azure Virtual Machine Scale Sets

1. Buka **Virtual machine scale sets** > klik **+ Create**.

2. Di tab **Basics**, isi pengaturan berikut:

    | Pengaturan | Nilai |
    |------------|-------|
    | Resource group | `rg8-p1` |
    | Name | `vmss1` |
    | Region | `Indonesia Central` |
    | Availability zone | `Zones 1, 2, 3` |
    | Orchestration mode | `Uniform` |
    | Image | `Windows Server 2019 Datacenter - x64 Gen2` |
    | Size | `Standard D2s_v3` |
    | Username | `localadmin` |
    | Password | Buat password aman |

    ![Buat VMSS](../media/az104-lab08-create-vmss.png)

3. Pada tab **Spot**, klik **Next: Disks >**, lanjutkan hingga **Networking**.

4. Klik **Edit virtual network**, ubah:

    | Pengaturan | Nilai |
    |------------|-------|
    | Name | `vmss-vnet` |
    | Address range | `10.82.0.0/20` |
    | Subnet name | `subnet0` |
    | Subnet range | `10.82.0.0/24` |

5. Edit network interface, lalu buat Network Security Group baru dengan rule:

    | Pengaturan | Nilai |
    |------------|-------|
    | Source | `Any` |
    | Service | `HTTP` |
    | Action | `Allow` |
    | Priority | `1010` |
    | Name | `allow-http` |

6. Aktifkan **Public IP address**, klik **OK**.

7. Di bagian **Load balancing**, pilih **Azure load balancer**, lalu buat dengan nama `vmss-lb`.

8. Di tab **Management**, nonaktifkan **Boot diagnostics**.

9. Klik **Review + create**, lalu **Create**.

## Task 4: Scale Azure Virtual Machine Scale Sets

### Scale out rule

1. Buka resource `vmss1` > pilih **Scaling** > ubah **Scale mode** ke **Scale based on metric**.

2. Tambahkan rule:

    | Pengaturan | Nilai |
    |------------|-------|
    | Metric name | `Percentage CPU` |
    | Operator | `Greater than` |
    | Threshold | `70` |
    | Duration | `10` menit |
    | Operation | `Increase percent by 50%` |
    | Cool down | `5` menit |

    ![Scaling Rule](../media/az104-lab08-scale-rule.png)

3. Klik **Save**.

### Scale in rule

Tambahkan rule:

    | Pengaturan | Nilai |
    |------------|-------|
    | Operator | `Less than` |
    | Threshold | `30` |
    | Operation | `decrease percentage by 50%` |

Klik **Save**.

### Instance limits

    | Pengaturan | Nilai |
    |------------|-------|
    | Minimum | `2` |
    | Maximum | `10` |
    | Default | `2` |

Klik **Save**. Lihat **Instances** untuk memantau VM.

## Task 5: Create a virtual machine using Azure PowerShell (opsional)

```powershell
New-AzVm `
-ResourceGroupName 'rg8-p1' `
-Name 'myPSVM' `
-Location 'Indonesia Central' `
-Image 'Win2019Datacenter' `
-Zone '1' `
-Size 'Standard_D2s_v3' `
-Credential (Get-Credential)

## Task 6: Create a virtual machine using the CLI (opsional 2)

1. Gunakan ikon di kanan atas untuk membuka sesi **Cloud Shell**. Alternatifnya, buka langsung `https://shell.azure.com`.

2. Pastikan kamu memilih **Bash**. Jika diminta, konfigurasikan penyimpanan shell.

3. Jalankan perintah berikut untuk membuat VM. Saat diminta, berikan `username` dan `password`. Sambil menunggu proses selesai, kamu dapat melihat dokumentasi resmi perintah [az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create).

    ```sh
    az vm create \
      --name myCLIVM \
      --resource-group rg8-p1 \
      --image Ubuntu2204 \
      --admin-username localadmin \
      --generate-ssh-keys
    ```

4. Setelah perintah selesai, gunakan perintah berikut untuk memverifikasi VM telah berhasil dibuat:

    ```sh
    az vm show --name myCLIVM --resource-group rg8-p1 --show-details
    ```

5. Pastikan nilai `powerState` adalah **VM Running**.

6. Gunakan perintah berikut untuk menghentikan dan mendealokasi VM:

    ```sh
    az vm deallocate --resource-group rg8-p1 --name myCLIVM
    ```

7. Jalankan lagi perintah `az vm show` untuk memastikan bahwa status `powerState` adalah **VM deallocated**.

    >**Tahukah kamu?** Ketika kamu menghentikan VM menggunakan Azure CLI atau portal, status VM menjadi *deallocated*, artinya public IP non-statis akan dilepaskan dan biaya komputasi akan dihentikan.

---

## Cleanup your resources

Jika kamu menggunakan **langganan pribadi**, sebaiknya kamu menghapus resource setelah selesai menggunakan lab ini agar tidak menimbulkan biaya tambahan.

**Cara membersihkan resource:**

- Melalui portal Azure:
  + Masuk ke resource group `rg8-p1`
  + Klik **Delete resource group**
  + Ketik ulang nama resource group, lalu klik **Delete**

- Menggunakan Azure PowerShell:
    ```powershell
    Remove-AzResourceGroup -Name rg8-p1
    ```

- Menggunakan Azure CLI:
    ```sh
    az group delete --name rg8-p1
    ```

---

## Extend your learning with Copilot

Kamu dapat menggunakan **Microsoft Copilot** untuk memperdalam pembelajaran di luar lab ini. Beberapa contoh prompt yang bisa kamu coba:

+ Jelaskan langkah dan perintah Azure CLI untuk membuat Linux Virtual Machine.
+ Jelaskan berbagai metode scaling VM di Azure dan bagaimana meningkatkan performanya.
+ Apa itu storage lifecycle management di Azure dan bagaimana hal ini bisa mengoptimalkan biaya?

Akses Copilot melalui browser Edge atau kunjungi [copilot.microsoft.com](https://copilot.microsoft.com).

---

## Learn more with self-paced training

Ingin belajar lebih lanjut? Coba beberapa modul pelatihan berikut:

- [Create a Windows virtual machine in Azure](https://learn.microsoft.com/training/modules/create-windows-virtual-machine-in-azure/)  
  Pelajari cara membuat Windows VM dan terhubung menggunakan Remote Desktop.

- [Build a scalable application with Virtual Machine Scale Sets](https://learn.microsoft.com/training/modules/build-app-with-scale-sets/)  
  Otomatiskan skala aplikasi berdasarkan beban kerja dan minimalkan biaya.

- [Connect to virtual machines through the Azure portal by using Azure Bastion](https://learn.microsoft.com/en-us/training/modules/connect-vm-with-azure-bastion/)  
  Gunakan Azure Bastion untuk koneksi VM yang aman langsung dari portal Azure.

---

## Key takeaways

Selamat! Kamu telah menyelesaikan lab ini. Berikut adalah poin-poin penting yang perlu diingat:

+ Azure Virtual Machine adalah sumber daya komputasi yang fleksibel dan sesuai permintaan.
+ VM mendukung dua jenis scaling: **vertical scaling** (mengubah ukuran VM) dan **horizontal scaling** (menambah atau mengurangi jumlah VM).
+ Virtual Machine Scale Set memudahkan pengelolaan dan autoscaling banyak VM berdasarkan beban kerja.
+ VM pada Scale Set dibuat dari image dan konfigurasi yang sama, mendukung skalabilitas dan konsistensi tinggi.
+ Kamu dapat menggunakan portal Azure, PowerShell, maupun CLI untuk membuat dan mengelola VM.
+ Status *deallocated* menghentikan biaya komputasi dan melepaskan public IP yang tidak statis.

---

