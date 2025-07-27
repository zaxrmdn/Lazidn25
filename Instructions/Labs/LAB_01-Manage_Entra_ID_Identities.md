---
lab:
    title: 'Lab 01: Manage Microsoft Entra ID Identities'
    module: 'Administer Identity'
---

# Lab 01 - Manage Microsoft Entra ID Identities

## Lab introduction

Ini adalah lab pertama dalam rangkaian lab untuk Azure Administrator. Dalam lab ini, Anda akan mempelajari tentang pengguna (users) dan grup (groups). Pengguna dan grup adalah elemen dasar dari solusi identitas.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur dalam lab ini.


## Estimated timing: 30 minutes

## Lab scenario

Organisasi Anda sedang membangun lingkungan lab baru untuk pengujian aplikasi dan layanan pra-produksi. Beberapa engineer akan dipekerjakan untuk mengelola lingkungan lab ini, termasuk virtual machine. Untuk memungkinkan para engineer melakukan autentikasi menggunakan Microsoft Entra ID, Anda ditugaskan untuk menyediakan pengguna dan grup. Untuk meminimalkan overhead administrasi, keanggotaan grup harus diperbarui secara otomatis berdasarkan jabatan pekerjaan (job title).

## Architecture diagram

![Diagram of the lab 01 architecture.](../media/az104-lab01-architecture.png)

## Job skills

+ Tugas 1: Membuat dan mengonfigurasi akun user.
+ Tugas 2: Membuat grup dan menambahkan member.

## Task 1: Create and configure user accounts

Pada tugas ini, Anda akan membuat dan mengonfigurasi akun user. Akun user menyimpan data seperti nama, departemen, lokasi, dan informasi kontak.

1. Masuk ke Azure portal **Azure portal** - `https://portal.azure.com`.

2. Untuk melanjutkan, pilih **Cancel** pada layar sambutan **Welcome to Azure**.

    >**Note:** The Azure portal is used in all the labs. If you are new to the Azure, search for and select `Quickstart Center`. Take a few minutes to watch the **Getting started in the Azure portal** video. Even if you have used the portal before, you will find a few tips and tricks on navigating and customizing the interface.
    
3. Cari dan pilih `Microsoft Entra ID`.

4. Pilih tab **Overview**, lalu tab **Manage tenants**.

    >**Tahukah kamu?** Tenant adalah instance spesifik dari Microsoft Entra ID yang berisi akun dan grup. Kamu dapat membuat tenant tambahan dan **Switch** antar tenant.

5. Kembali ke halaman **Entra ID** menggunakan tombol kembali atau breadcrumb.

6. Jelajahi opsi lainnya seperti **Licenses** dan **Password reset**.

### Membuat User Baru

1. Pilih **Users**, kemudian di drop-down **New user**, pilih **Create new user**.

2. Buat user baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | User principal name | `user1-p1` |
    | Display name | `user-p1` |
    | Auto-generate password | **dicentang** |
    | Account enabled | **dicentang** |
    | Job title | `IT Lab Administrator P1` |
    | Department | `IT P1` |
    | Usage location | **Indonesia** |

3. Pilih **Review + create**, lalu **Create**.

4. Refresh halaman dan pastikan user berhasil dibuat.

### Mengundang External User

1. Di menu **New user**, pilih **Invite an external user**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Email | alamat email kamu |
    | Display name | nama kamu |
    | Send invite message | **dicentang** |
    | Message | `Welcome to Azure and our group project` |

2. Di tab **Properties**, isi informasi dasar berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Job title | `IT Lab Administrator P1` |
    | Department | `IT P1` |
    | Usage location | **Indonesia** |

3. Pilih **Review + invite**, lalu **Invite**.

4. Refresh halaman dan pastikan external user berhasil diundang.

    >**Catatan:** Biasanya kamu tidak akan membuat user satu per satu. Apakah organisasi kamu punya metode otomatisasi untuk membuat akun user?

## Task 2: Membuat Group dan Menambahkan Member

Group dapat berisi user atau perangkat. Ada dua cara utama untuk menambahkan anggota: statis dan dinamis.

1. Di Azure portal, cari dan pilih `Microsoft Entra ID`, lalu pilih **Groups**.

2. Jelajahi beberapa pengaturan grup seperti:

    + **Expiration**: mengatur masa aktif grup.
    + **Naming policy**: mengatur kata terlarang dan format nama grup.

3. Di blade **All groups**, pilih **+ New group** dan isi:

    | Pengaturan | Nilai |
    | --- | --- |
    | Group type | **Security** |
    | Group name | `IT Lab Administrators P1` |
    | Group description | `Administrators that manage the IT lab` |
    | Membership type | **Assigned** |

    >**Catatan:** Untuk dynamic membership dibutuhkan lisensi Entra ID Premium P1/P2.

4. Tambahkan **owner** grup (pilih akun kamu).

5. Tambahkan **members**: `user1-p1` dan guest user.

6. Klik **Create**.

7. Refresh dan verifikasi grup telah dibuat.

8. Tinjau tab **Members** dan **Owners** dari grup.

    >**Catatan:** Apakah organisasi kamu memiliki kebijakan pengelolaan grup yang jelas?

## Perluas Pembelajaran dengan Copilot

Buka *copilot.microsoft.com* dan coba beberapa prompt:

+ Perintah PowerShell/CLI untuk membuat grup `IT Admins P1`.
+ Strategi langkah demi langkah untuk mengelola user dan grup.
+ Cara membuat user dan grup secara massal.
+ Perbandingan akun user internal dan eksternal di Entra ID.

## Pelatihan Mandiri

+ [Memahami Microsoft Entra ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/)
+ [Membuat user dan grup di Entra ID](https://learn.microsoft.com/training/modules/create-users-and-groups-in-azure-active-directory/)
+ [Self-service password reset](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/)

## Ringkasan

+ Tenant merepresentasikan organisasi kamu.
+ Entra ID memiliki user internal dan eksternal.
+ Grup dapat berisi user atau perangkat, jenisnya adalah Security atau Microsoft 365.
+ Keanggotaan grup bisa statis atau dinamis.