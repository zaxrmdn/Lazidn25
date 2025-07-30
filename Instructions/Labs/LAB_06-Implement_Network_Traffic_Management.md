---
lab:
    title: 'Lab 06: Implement Network Traffic Management'
    module: 'Administer Network Traffic Management'
---

# Lab 06 - Implement Network Traffic Management

## Lab introduction

Pada lab ini, Anda akan mempelajari cara mengonfigurasi dan menguji Load Balancer publik serta Application Gateway.

Lab ini membutuhkan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur dalam lab ini. Anda boleh mengubah wilayah, namun langkah-langkah ditulis menggunakan wilayah **Indonesia Central**.

## Estimated timing: 50 minutes

## Lab scenario

Organisasi Anda memiliki situs web publik. Anda perlu mendistribusikan permintaan publik masuk ke beberapa virtual machine. Anda juga perlu menyajikan gambar dan video dari VM yang berbeda. Anda berencana untuk mengimplementasikan Azure Load Balancer dan Azure Application Gateway. Semua sumber daya berada di wilayah yang sama.

## Interactive lab simulations

>**Catatan**: Simulasi interaktif pada lab sebelumnya telah dihentikan.

## Job skills

+ Task 1: Gunakan template untuk menyediakan infrastruktur.
+ Task 2: Konfigurasikan Azure Load Balancer.
+ Task 3: Konfigurasikan Azure Application Gateway.

## Task 1: Use a template to provision an infrastructure

Dalam tugas ini, Anda akan menggunakan template untuk menyebarkan satu virtual network, satu network security group, dan tiga virtual machine.

1. Unduh file lab **\\Allfiles\\Lab06** (template dan parameter).
2. Masuk ke **Azure portal** - `https://portal.azure.com`.
3. Cari dan pilih `Deploy a custom template`.
4. Pada halaman penyebaran, pilih **Build your own template in the editor**.
5. Pada halaman pengeditan template, pilih **Load file**.
6. Pilih file **\\Allfiles\\Lab06\\06-vms-template.json** dan klik **Open**.
7. Klik **Save**.
8. Pilih **Edit parameters** dan muat file **\\Allfiles\\Lab06\\06-vms-parameters.json**.
9. Klik **Save**.
10. Isi bidang seperti berikut (biarkan nilai lain default):

    | Setting       | Value                   |
    |---------------|-------------------------|
    | Subscription  | langganan Azure Anda    |
    | Resource group| `rg6-p1` (buat baru jika perlu) |
    | Password      | Masukkan password aman  |

    >**Catatan**: Jika Anda mendapat error bahwa ukuran VM tidak tersedia, pilih SKU lain yang memiliki minimal 2 core.

11. Klik **Review + Create**, lalu **Create**.

    >**Catatan**: Tunggu hingga penyebaran selesai (sekitar 5 menit).

    >**Catatan**: Tinjau sumber daya yang disebarkan: satu virtual network dengan tiga subnet, dan tiap subnet memiliki satu VM.

## Task 2: Configure an Azure Load Balancer

Pada tugas ini, Anda mengimplementasikan Azure Load Balancer di depan dua virtual machine. Load Balancer menyediakan konektivitas layer 4. Konfigurasinya meliputi IP publik (frontend), backend pool, dan aturan distribusi lalu lintas.

## Architecture diagram - Load Balancer

>**Catatan**: Perhatikan Load Balancer mendistribusikan ke dua VM dalam virtual network yang sama.

![Diagram of the lab tasks.](../media/az104-lab06-lb-architecture.png)

1. Di portal Azure, cari dan pilih `Load balancers`, lalu klik **+ Create**.
2. Gunakan pengaturan berikut (sisanya biarkan default):

    | Setting       | Value             |
    |---------------|-------------------|
    | Subscription  | langganan Azure Anda |
    | Resource group| `rg6-p1`        |
    | Name          | `lb`         |
    | Region        | wilayah sama seperti VM |
    | SKU           | **Standard**       |
    | Type          | **Public**         |
    | Tier          | **Regional**       |

3. Pada tab **Frontend IP configuration**, klik **Add a frontend IP configuration** dan gunakan:

    | Setting             | Value         |
    |---------------------|---------------|
    | Name                | `fe`    |
    | IP type             | IP address    |
    | Gateway Load Balancer | None       |
    | Public IP address   | Buat baru     |

4. Di popup **Add a public IP address**, isikan:

    | Setting             | Value         |
    |---------------------|---------------|
    | Name                | `lbpip` |
    | SKU                 | Standard      |
    | Tier                | Regional      |
    | Assignment          | Static        |
    | Routing Preference  | Microsoft network |

    >**Catatan:** IP statis dari SKU Standard akan tetap aktif hingga resource dihapus.

5. Pada tab **Backend pools**, klik **Add a backend pool**, isikan:

    | Setting              | Value            |
    |----------------------|------------------|
    | Name                 | `be`       |
    | Virtual network      | `06-vnet1` |
    | Backend Pool Config. | NIC              |
    | Tambahkan VM         | centang `06-vm0` dan `06-vm1` |

6. Klik **Review + create**, pastikan tidak ada error, lalu klik **Create**.

7. Setelah Load Balancer selesai dibuat, buka dan klik **Go to resource**.

### Tambahkan aturan untuk distribusi trafik

1. Di bagian **Settings**, pilih **Load balancing rules**.
2. Klik **+ Add**, isi pengaturan:

    | Setting             | Value         |
    |---------------------|---------------|
    | Name                | `lbrule`|
    | IP Version          | IPv4          |
    | Frontend IP Address | `fe`    |
    | Backend pool        | `be`    |
    | Protocol            | TCP           |
    | Port                | 80            |
    | Backend port        | 80            |
    | Health probe        | Buat baru     |
    | Probe Name          | `hp`    |
    | Protocol            | TCP           |
    | Port                | 80            |
    | Interval            | 5             |
    | Session persistence | None          |
    | Idle timeout        | 4             |
    | TCP reset           | Disabled      |
    | Floating IP         | Disabled      |
    | SNAT                | Recommended   |

3. Setelah selesai, buka tab **Frontend IP configuration**, salin alamat IP publik.

4. Buka browser dan akses IP tersebut. Anda akan melihat pesan dari `vm0` atau `vm1`.

5. Muat ulang halaman untuk melihat Load Balancer mendistribusikan trafik.

    > **Catatan**: Coba muat ulang beberapa kali atau gunakan jendela InPrivate.

## Task 3: Configure an Azure Application Gateway

Pada tugas ini, Anda mengimplementasikan Application Gateway untuk dua VM. Application Gateway menyediakan load balancing layer 7, WAF, SSL termination, dan enkripsi ujung ke ujung. Gateway akan merutekan `/image/*` ke satu VM dan `/video/*` ke VM lainnya.

## Architecture diagram - Application Gateway

>**Catatan**: Application Gateway menggunakan virtual network yang sama dengan Load Balancer. Ini tidak umum di lingkungan produksi.

![Diagram of the lab tasks.](../media/az104-lab06-gw-architecture.png)

1. Di portal Azure, buka **Virtual networks** > `06-vnet1` > **Subnets** > **+ Subnet**.
2. Tambahkan subnet:

    | Setting           | Value             |
    |-------------------|-------------------|
    | Name              | `subnet-appgw`    |
    | Starting address  | `10.60.3.224`     |
    | Size              | `/27`             |

    > **Catatan**: Application Gateway memerlukan subnet khusus dengan ukuran /27 atau lebih besar.

3. Kembali ke portal, cari **Application gateways** > **+ Create**.
4. Di tab **Basics**, isikan:

    | Setting               | Value               |
    |-----------------------|---------------------|
    | Resource group        | `rg6-p1`         |
    | Name                  | `appgw`       |
    | Tier                  | Standard V2         |
    | Autoscaling           | No                  |
    | Instance count        | 2                   |
    | HTTP2                 | Disabled            |
    | Virtual network       | `06-vnet1`    |
    | Subnet                | `subnet-appgw`      |

5. Lanjut ke tab **Frontends**, pilih:

    | Setting           | Value         |
    |-------------------|---------------|
    | Type              | Public        |
    | Public IP address | Buat baru     |
    | Name              | `gwpip` |
    | Availability zone | 1             |

6. Lanjut ke tab **Backends**, dan tambahkan tiga backend pool:

    - `appgwbe` untuk kedua VM
    - `imagebe` untuk `vm1`
    - `video` untuk `vm2`

7. Masuk ke tab **Configuration**, tambahkan routing rule:

    | Rule name     | gwrule |
    | Priority      | 10           |
    | Listener name | listener |
    | Frontend IP   | Public IPv4  |
    | Protocol      | HTTP         |
    | Port          | 80           |
    | Listener type | Basic        |

8. Buat **path-based routing**:

    **Rule ke images**

    | Path     | /image/*         |
    | Target   | images           |
    | Backend  | `imagebe`  |

    **Rule ke videos**

    | Path     | /video/*         |
    | Target   | videos           |
    | Backend  | `video`  |

9. Klik **Review + create** dan tunggu 5-10 menit.

10. Setelah selesai, buka `appgw` > **Backend health**, pastikan semua **Healthy**.

11. Salin IP publik di tab **Overview**.

12. Akses URL `http://<ip>/image/` dan `http://<ip>/video/` untuk menguji.

## Cleanup your resources

Jika menggunakan langganan pribadi, hapus resource untuk menghindari biaya:

- Di portal: buka resource group, klik **Delete resource group**.
- PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`
- CLI: `az group delete --name resourceGroupName`

## Extend your learning with Copilot

Cobalah prompt berikut di *copilot.microsoft.com*:

+ Bandingkan Azure Load Balancer dan Application Gateway.
+ Alat untuk troubleshooting koneksi Load Balancer.
+ Langkah-langkah konfigurasi Application Gateway.
+ Tabel perbandingan solusi load balancing di Azure.

## Learn more with self-paced training

+ [Improve application scalability and resiliency by using Azure Load Balancer](https://learn.microsoft.com/training/modules/improve-app-scalability-resiliency-with-load-balancer/)
+ [Load balance your web service traffic with Application Gateway](https://learn.microsoft.com/training/modules/load-balance-web-traffic-with-application-gateway/)

## Key takeaways

+ Load Balancer cocok untuk distribusi trafik di level transport (layer 4).
+ Load Balancer publik menangani trafik dari internet, sedangkan internal untuk private IP.
+ Load Balancer Standard cocok untuk performa tinggi dan latensi rendah.
+ Application Gateway cocok untuk trafik web (layer 7), dengan WAF dan SSL offloading.
+ Dapat merutekan berdasarkan URI path atau host headers.
