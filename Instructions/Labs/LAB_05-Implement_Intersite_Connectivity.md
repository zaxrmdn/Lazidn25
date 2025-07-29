---
lab:
  title: 'Lab 05: Implement Intersite Connectivity'
  module: 'Administer Intersite Connectivity'
---

# Lab 05 - Implement Intersite Connectivity

## Pendahuluan

Dalam lab ini, Anda akan mengeksplorasi komunikasi antara Virtual Network di Azure. Anda akan mengimplementasikan Virtual Network Peering dan menguji koneksi antar Virtual Machine. Anda juga akan membuat custom route untuk mengontrol lalu lintas jaringan.

> ⚠️ Lab ini memerlukan Azure Subscription. Langkah-langkah menggunakan region **East US**, tetapi Anda dapat memilih region lain.

---

## Estimasi Waktu  
🕒 Sekitar **50 menit**

---

## Skenario

Organisasi Anda memisahkan layanan inti TI (seperti DNS dan security services) dari bagian bisnis lainnya seperti departemen manufaktur. Namun, beberapa aplikasi di kedua area tersebut perlu saling berkomunikasi. Lab ini mensimulasikan skenario umum seperti pemisahan produksi dan pengembangan atau antar anak perusahaan.

---

## Diagram Arsitektur

![Architecture Diagram](../media/az104-lab05-architecture.png)

---

## Keterampilan yang Dipelajari

- Membuat Virtual Machine di Virtual Network
- Membuat VM di Virtual Network lain
- Menguji koneksi antar VM menggunakan Network Watcher
- Mengonfigurasi Virtual Network Peering
- Menggunakan Azure PowerShell untuk menguji koneksi
- Membuat Custom Route dengan Route Table

---

## Task 1 - Membuat CoreServices VM dan Virtual Network

### Langkah-langkah:

1. Masuk ke **Azure Portal**: [https://portal.azure.com](https://portal.azure.com)
2. Cari dan pilih **Virtual Machines**
3. Klik **Create > Azure Virtual Machine**
4. Pada tab **Basics**, isi formulir sebagai berikut:

   | Setting | Value |
   |--------|-------|
   | Subscription | *your subscription* |
   | Resource Group | `az104-rg5` |
   | Virtual machine name | `CoreServicesVM` |
   | Region | East US |
   | Availability options | No infrastructure redundancy required |
   | Security type | Standard |
   | Image | Windows Server 2019 Datacenter: x64 Gen2 |
   | Size | Standard_DS2_v3 |
   | Username | `localadmin` |
   | Password | *password kompleks* |
   | Public inbound ports | None |

5. Klik **Next: Disks >**, biarkan default
6. Klik **Next: Networking >**
7. Klik **Create new** untuk Virtual Network dan isi:

   | Setting | Value |
   |--------|-------|
   | Name | `CoreServicesVnet` |
   | Address range | `10.0.0.0/16` |
   | Subnet name | `Core` |
   | Subnet address range | `10.0.0.0/24` |

8. Klik **Next: Monitoring >** dan nonaktifkan **Boot diagnostics**
9. Klik **Review + Create > Create**

---

## Task 2 - Membuat Manufacturing VM di Virtual Network Berbeda

1. Kembali ke **Virtual Machines** dan klik **Create > Azure Virtual Machine**
2. Isi tab **Basics**:

   | Setting | Value |
   |--------|-------|
   | Subscription | *your subscription* |
   | Resource Group | `az104-rg5` |
   | Virtual machine name | `ManufacturingVM` |
   | Region | East US |
   | Availability options | No infrastructure redundancy required |
   | Security type | Standard |
   | Image | Windows Server 2019 Datacenter: x64 Gen2 |
   | Size | Standard_DS2_v3 |
   | Username | `localadmin` |
   | Password | *password kompleks* |
   | Public inbound ports | None |

3. Klik **Next: Disks >**, biarkan default
4. Klik **Next: Networking >**, lalu pilih **Create new** Virtual Network:

   | Setting | Value |
   |--------|-------|
   | Name | `ManufacturingVnet` |
   | Address range | `172.16.0.0/16` |
   | Subnet name | `Manufacturing` |
   | Subnet address range | `172.16.0.0/24` |

5. Nonaktifkan **Boot diagnostics**
6. Klik **Review + Create > Create**

---

## Task 3 - Menguji Koneksi dengan Network Watcher

> 🧪 Pastikan kedua VM telah berjalan (*Running*)

1. Di Azure Portal, cari dan buka **Network Watcher**
2. Pilih **Connection troubleshoot**
3. Isi sebagai berikut:

   | Field | Value |
   |-------|-------|
   | Source type | Virtual machine |
   | Virtual machine | `CoreServicesVM` |
   | Destination type | Virtual machine |
   | Virtual machine | `ManufacturingVM` |
   | Preferred IP Version | Both |
   | Protocol | TCP |
   | Destination port | 3389 |

4. Klik **Run diagnostic tests**

> 💡 Koneksi akan gagal karena belum ada peering antar Virtual Network.

---

## Task 4 - Konfigurasi Virtual Network Peering

### Peering dari CoreServicesVnet ke ManufacturingVnet

1. Buka **CoreServicesVnet** > **Peerings** > **+ Add**
2. Konfigurasi:

   - Peering link name: `CoreServicesVnet-to-ManufacturingVnet`
   - Remote virtual network: `ManufacturingVnet`
   - Aktifkan akses dua arah dan forwarded traffic

### Peering dari ManufacturingVnet ke CoreServicesVnet

3. Ulangi langkah yang sama untuk `ManufacturingVnet-to-CoreServicesVnet`

4. Pastikan **Peering status** pada keduanya menjadi **Connected**

---

## Task 5 - Menguji Koneksi Menggunakan PowerShell

### Cari Alamat IP Privat CoreServicesVM

1. Buka **CoreServicesVM** > tab **Networking**
2. Catat **Private IP address**

### Jalankan Test dari ManufacturingVM

1. Buka **ManufacturingVM**
2. Masuk ke **Run command** > **RunPowerShellScript**
3. Jalankan perintah berikut:

```powershell
Test-NetConnection <PRIVATE-IP-CORESERVICESVM> -Port 3389

### Hasil Uji Koneksi

Tes koneksi seharusnya berhasil karena **Virtual Network Peering** telah dikonfigurasi.

> 🖥️ Nama komputer dan alamat IP remote Anda mungkin berbeda dengan gambar berikut.

![Test-NetConnection success](../media/az104-lab05-success.png)

---


## Task 6 - Membuat Custom Route

Dalam task ini, Anda akan mengontrol lalu lintas jaringan antara subnet `perimeter` dan subnet internal `Core`. Sebuah **Network Virtual Appliance (NVA)** akan dipasang di subnet inti, dan semua trafik dari perimeter akan diarahkan ke sana.

### Langkah-langkah:

#### 1. Tambahkan Subnet `perimeter` ke Virtual Network

1. Cari dan pilih **CoreServicesVnet** di Azure portal  
2. Buka menu **Subnets**, lalu klik **+ Subnet**
3. Isi konfigurasi berikut, lalu klik **Add**

   | Setting | Value |
   |--------|-------|
   | Name | `perimeter` |
   | Address range | `10.0.1.0/24` |

---

#### 2. Buat Route Table

1. Di Azure portal, cari dan pilih **Route tables**
2. Klik **+ Create**
3. Isi formulir berikut:

   | Setting | Value |
   |--------|-------|
   | Subscription | *your subscription* |
   | Resource group | `az104-rg5` |
   | Region | East US |
   | Name | `rt-CoreServices` |
   | Propagate gateway routes | No |

4. Klik **Review + Create** > **Create**

---

#### 3. Tambahkan Route Baru

1. Setelah deployment selesai, buka **Route tables**, lalu pilih **rt-CoreServices** (klik nama, bukan checkbox)
2. Di bagian **Settings**, buka **Routes** > klik **+ Add**
3. Masukkan konfigurasi berikut:

   | Setting | Value |
   |--------|-------|
   | Route name | `PerimetertoCore` |
   | Destination type | IP Addresses |
   | Destination IP addresses | `10.0.0.0/16` |
   | Next hop type | Virtual appliance |
   | Next hop address | `10.0.1.7` |

4. Klik **Add**

---

#### 4. Asosiasikan Route Table ke Subnet

1. Di halaman **rt-CoreServices**, buka tab **Subnets** > klik **+ Associate**
2. Isi sebagai berikut:

   | Setting | Value |
   |--------|-------|
   | Virtual network | `CoreServicesVnet` |
   | Subnet | `Core` |

> 📌 Anda telah membuat **User Defined Route (UDR)** untuk mengarahkan trafik dari subnet perimeter ke appliance jaringan virtual di subnet inti.

---

## Pembersihan Sumber Daya

Jika Anda menggunakan **subscription pribadi**, pastikan untuk menghapus resource setelah selesai agar biaya tidak terus berjalan.

### Menghapus dari Portal

- Buka **Resource Group** `az104-rg5`
- Klik **Delete resource group**
- Masukkan nama resource group untuk konfirmasi, lalu klik **Delete**

### Menghapus via PowerShell

```powershell
Remove-AzResourceGroup -Name az104-rg5

## Extend your learning with Copilot

Copilot dapat membantu Anda mempelajari cara menggunakan alat skrip Azure. Copilot juga dapat membantu pada area yang tidak dibahas di lab atau jika Anda memerlukan informasi tambahan. Buka browser Edge dan pilih Copilot (pojok kanan atas) atau buka *copilot.microsoft.com*. Luangkan waktu beberapa menit untuk mencoba prompt berikut:

+ Bagaimana cara menggunakan perintah Azure PowerShell atau Azure CLI untuk menambahkan peering jaringan virtual antara vnet1 dan vnet2?
+ Buat tabel yang menyoroti berbagai alat pemantauan Azure dan pihak ketiga yang didukung di Azure. Sorot kapan harus menggunakan masing-masing alat tersebut.
+ Kapan saya harus membuat rute jaringan kustom di Azure?

## Learn more with self-paced training

+ [Distribute your services across Azure virtual networks and integrate them by using virtual network peering](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/). Gunakan peering jaringan virtual untuk memungkinkan komunikasi antar jaringan virtual dengan cara yang aman dan sederhana.
+ [Manage and control traffic flow in your Azure deployment with routes](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/). Pelajari cara mengontrol lalu lintas jaringan virtual Azure dengan menerapkan rute kustom.

## Key takeaways

Selamat telah menyelesaikan lab. Berikut adalah hal-hal penting yang dapat dipelajari dari lab ini:

+ Secara default, sumber daya di jaringan virtual yang berbeda tidak dapat berkomunikasi.
+ Peering jaringan virtual memungkinkan Anda menghubungkan dua atau lebih jaringan virtual di Azure secara mulus.
+ Jaringan virtual yang telah di-peer dianggap satu jaringan untuk keperluan konektivitas.
+ Lalu lintas antara mesin virtual di jaringan virtual yang di-peer menggunakan infrastruktur backbone Microsoft.
+ Rute sistem ditentukan secara otomatis untuk setiap subnet dalam jaringan virtual. Rute yang ditentukan pengguna menggantikan atau menambah rute sistem default.
+ Azure Network Watcher menyediakan serangkaian alat untuk memantau, mendiagnosis, dan melihat metrik serta log untuk sumber daya IaaS Azure.