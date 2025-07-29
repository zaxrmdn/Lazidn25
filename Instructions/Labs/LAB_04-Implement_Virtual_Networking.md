---
lab:
    title: 'Lab 04: Implement Virtual Networking'
    module: 'Implement Virtual Networking'
---

# Lab 04 - Implement Virtual Networking

## Lab introduction

Lab ini adalah yang pertama dari tiga lab yang berfokus pada virtual networking. Di lab ini, Anda akan mempelajari dasar-dasar virtual networking dan subnetting. Anda akan belajar cara melindungi jaringan menggunakan Network Security Groups dan Application Security Groups. Anda juga akan mempelajari tentang DNS zones dan record-nya.

Lab ini memerlukan langganan Azure. Jenis langganan Anda mungkin mempengaruhi ketersediaan fitur dalam lab ini. Anda boleh mengubah region, namun langkah-langkah ditulis menggunakan **Indonesia Central**.

## Estimated time: 50 minutes

## Lab scenario 

Organisasi global Anda berencana untuk menerapkan virtual network. Tujuan awalnya adalah mengakomodasi semua resource yang ada. Namun, karena organisasi sedang dalam fase pertumbuhan, maka kapasitas tambahan perlu disiapkan untuk mendukung ekspansi tersebut.

Virtual network **CoreServicesVnet** memiliki jumlah resource terbesar. Karena pertumbuhan yang besar diantisipasi, maka address space yang besar diperlukan.

Virtual network **ManufacturingVnet** berisi sistem-sistem untuk operasi fasilitas manufaktur. Organisasi mengantisipasi banyak perangkat internal yang terhubung dan akan digunakan untuk mengambil data dari sistem.

## Interactive lab simulations

>**Catatan**: Simulasi lab yang sebelumnya disediakan telah dihentikan.

## Architecture diagram

![Network layout](../media/az104-lab04-architecture.png)

Virtual network dan subnet diatur dengan cara yang dapat mengakomodasi resource yang sudah ada dan tetap memungkinkan pertumbuhan di masa depan. Mari kita buat virtual network dan subnet ini sebagai dasar infrastruktur jaringan kita.

>**Tahukah Anda?**: Praktik yang baik adalah menghindari IP address range yang saling tumpang tindih agar mengurangi masalah dan menyederhanakan troubleshooting. Overlapping ini merupakan perhatian di seluruh jaringan, baik di cloud maupun on-premises. Banyak organisasi merancang skema IP address enterprise-wide untuk menghindari overlapping dan merencanakan pertumbuhan di masa depan.

## Job skills

+ Task 1: Create a virtual network with subnets using the portal.
+ Task 2: Create a virtual network and subnets using a template.
+ Task 3: Create and configure communication between an Application Security Group and a Network Security Group.
+ Task 4: Configure public and private Azure DNS zones.

---

## Task 1: Create a virtual network with subnets using the portal

Organisasi merencanakan pertumbuhan besar untuk core services. Di task ini, Anda akan membuat virtual network dan subnet terkait untuk mengakomodasi resource yang ada dan rencana ekspansi. Gunakan Azure portal untuk task ini.

1. Masuk ke **Azure portal** - `https://portal.azure.com`

2. Cari dan pilih `Virtual Networks`.

3. Klik **Create** di halaman Virtual networks.

4. Lengkapi tab **Basics** untuk CoreServicesVnet.

    | Option           | Value                         |
    |------------------|-------------------------------|
    | Resource Group   | `rg4-p1`                   |
    | Name             | `CoreServicesVnet`            |
    | Region           | **Indonesia Central**                   |

5. Pindah ke tab **IP Addresses**.

    | Option               | Value         |
    |----------------------|---------------|
    | IPv4 address space   | `10.20.0.0/16` |

6. Klik **+ Add a subnet** dan isi nama dan alamat untuk setiap subnet. Pastikan Anda menghapus default subnet dan klik **Add** untuk setiap subnet yang ditambahkan.

    | Subnet Name          | Starting address | Size  |
    |----------------------|------------------|-------|
    | SharedServicesSubnet | `10.20.10.0`     | `/24` |
    | DatabaseSubnet       | `10.20.20.0`     | `/24` |

>**Catatan**: Setiap virtual network harus memiliki minimal satu subnet. Lima IP akan selalu dicadangkan, jadi perhitungkan dalam perencanaan Anda.

7. Klik **Review + create**, pastikan validasi berhasil, lalu klik **Create**.

8. Setelah selesai, pilih **Go to resource**.

9. Verifikasi konfigurasi **Address space** dan **Subnets**.

10. Di bagian **Automation**, pilih **Export template**, lalu tunggu hingga file template dibuat.

11. **Download** file tersebut.

12. Buka folder **Downloads** dan lakukan **Extract all** pada file ZIP yang diunduh.

13. Pastikan Anda memiliki file **template.json** untuk digunakan pada task berikutnya.

---

## Task 2: Create a virtual network and subnets using a template

1. Temukan file **template.json** yang diunduh dari task sebelumnya.

2. Buka file tersebut dengan editor teks pilihan Anda.

### Ubah untuk virtual network ManufacturingVnet

- Ganti semua `CoreServicesVnet` menjadi `ManufacturingVnet`.
- Ganti semua `10.20.0.0` menjadi `10.30.0.0`.

### Ubah untuk subnets ManufacturingVnet

- Ganti `SharedServicesSubnet` menjadi `SensorSubnet1`.
- Ganti `10.20.10.0/24` menjadi `10.30.20.0/24`.
- Ganti `DatabaseSubnet` menjadi `SensorSubnet2`.
- Ganti `10.20.20.0/24` menjadi `10.30.21.0/24`.

3. Baca kembali file dan pastikan semua sudah benar, lalu simpan.

### Ubah file parameters

- Buka file **parameters.json**.
- Ganti `CoreServicesVnet` dengan `ManufacturingVnet`.
- Simpan file tersebut.

### Deploy custom template

1. Di portal, cari dan pilih `Deploy a custom template`.

2. Pilih **Build your own template in the editor** lalu klik **Load file**.

3. Pilih file **template.json**, klik **Save**.

4. Klik **Edit parameters**, lalu klik **Load file**.

5. Pilih file **parameters.json**, klik **Save**.

6. Pilih resource group: `rg4-p1`.

7. Klik **Review + create**, lalu klik **Create**.

8. Tunggu hingga deployment selesai, lalu verifikasi di portal bahwa `ManufacturingVnet` dan subnet telah dibuat.

>**Catatan**: Jika deployment gagal sebagian, Anda dapat menghapus resource yang berhasil dibuat dan mencoba kembali.

---

## Task 3: Create and configure communication between an Application Security Group and a Network Security Group

### Buat Application Security Group (ASG)

1. Di portal, cari `Application security groups`.

2. Klik **Create**, lalu isi informasi:

    | Setting       | Value        |
    |---------------|--------------|
    | Resource group| `rg4-p1`  |
    | Name          | `asg-web`    |
    | Region        | **Indonesia Central**  |

3. Klik **Review + create**, lalu **Create**.

>**Catatan**: ASG ini nantinya akan dikaitkan dengan VM yang akan diproteksi oleh NSG.

### Buat dan kaitkan Network Security Group (NSG)

1. Cari `Network security groups`, klik **Create**, isi:

    | Setting       | Value          |
    |---------------|----------------|
    | Resource group| `rg4-p1`    |
    | Name          | `myNSGSecure`  |
    | Region        | **Indonesia Central**    |

2. Klik **Review + create**, lalu **Create**.

3. Setelah selesai, klik **Go to resource**, lalu pilih **Subnets** → **Associate**:

    | Setting        | Value               |
    |----------------|---------------------|
    | Virtual network| `CoreServicesVnet`  |
    | Subnet         | `SharedServicesSubnet` |

### Atur inbound rule dari ASG

1. Di NSG, buka **Inbound security rules**.

2. Klik **+ Add**, lalu isi:

    | Setting                            | Value         |
    |------------------------------------|---------------|
    | Source                             | Application security group |
    | Source application security groups | `asg-web`     |
    | Destination port ranges            | `80,443`      |
    | Protocol                           | TCP           |
    | Action                             | Allow         |
    | Priority                           | 100           |
    | Name                               | `AllowASG`    |

### Atur outbound rule untuk blokir internet

1. Buka **Outbound security rules**, klik **+ Add**, isi:

    | Setting                   | Value        |
    |---------------------------|--------------|
    | Destination service tag   | Internet     |
    | Destination port ranges   | 8080         |
    | Protocol                  | Any          |
    | Action                    | Deny         |
    | Priority                  | 4096         |
    | Name                      | DenyAnyCustom8080Outbound |

---

## Task 4: Configure public and private Azure DNS zones

### Public DNS Zone

1. Cari `DNS zones`, klik **+ Create**, isi:

    | Property      | Value         |
    |---------------|---------------|
    | Resource group| `rg4-p1`   |
    | Name          | `contoso.com` |

2. Klik **Review + create**, lalu **Create**.

3. Catat salah satu name server di **Overview**.

4. Tambahkan **A record** di Recordsets:

    | Name | Type | TTL | IP address   |
    |------|------|-----|--------------|
    | www  | A    | 1   | 10.1.1.4     |

5. Uji menggunakan `nslookup`:

    ```sh
    nslookup www.contoso.com <name server name>
    ```

### Private DNS Zone

1. Cari `Private DNS zones`, klik **+ Create**, isi:

    | Property      | Value                 |
    |---------------|-----------------------|
    | Resource group| `rg4-p1`           |
    | Name          | `private.contoso.com` |

2. Setelah berhasil, tambahkan **Virtual network link**:

    | Property     | Value              |
    |--------------|--------------------|
    | Link name    | `manufacturing-link` |
    | Virtual net  | `ManufacturingVnet`  |

3. Tambahkan **Recordset**:

    | Name     | Type | TTL | IP address |
    |----------|------|-----|------------|
    | sensorvm | A    | 1   | 10.1.1.4   |

---

## Cleanup your resources

Jika Anda menggunakan **langganan sendiri**, hapus resource group untuk menghindari biaya:

- Azure portal: pilih resource group → **Delete resource group**
- PowerShell:  
  ```powershell
  Remove-AzResourceGroup -Name rg4-p1
