---

demo:
    title: 'Demonstration 02: Administer Governance and Compliance'
    module: 'Administer Governance and Compliance'
---

# 02 - Administer Governance and Compliance

## Configure Subscriptions

Bagian ini **tidak memiliki demonstrasi formal**. 

**Reference**: [Create an additional Azure subscription](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription)

## Configure Azure Policy

In this demonstration, we will work with Azure policies.

**Reference**: [Tutorial: Build policies to enforce compliance - Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Assign a policy**

1. Masuk ke **Azure Portal**.

2. Cari dan pilih menu **Policy**.

3. Buka tab **Assignments** dan klik **Assign Policy**.

4. Bahas bagian **Scope**, yang menentukan cakupan sumber daya yang dipengaruhi oleh kebijakan.

5. Klik ikon titik tiga pada **Policy definition** untuk melihat daftar definisi kebijakan yang tersedia.

6. Cari dan pilih kebijakan **Allowed locations** – kebijakan ini membatasi lokasi sumber daya yang bisa digunakan organisasi.

7. Buka tab **Parameters** dan pilih satu atau beberapa lokasi yang diizinkan.

8. Klik **Review + create**, lalu **Create** untuk menerapkan kebijakan.


**Create and assign an initiative definition**

1. Kembali ke halaman utama **Azure Policy** dan pilih **Definitions** di bawah bagian *Authoring*.

2. Klik **Initiative Definition** di bagian atas halaman.

3. Isi **Nama** dan **Deskripsi** untuk inisiatif.

4. Buat **Kategori baru** (Create new category).

5. Tambahkan kebijakan **Allowed locations** dari panel kanan.

6. Tambahkan satu kebijakan tambahan pilihan Anda.

7. Klik **Save**, lalu **Assign** inisiatif ke subscription Anda.

**Check for compliance**

1. Kembali ke layanan **Azure Policy**.

2. Pilih tab **Compliance**.

3. Tinjau status kebijakan dan inisiatif yang telah diterapkan.

**Check for remediation tasks**

1. Masuk kembali ke layanan **Azure Policy**.

2. Pilih tab **Remediation**.

3. Tinjau semua tugas remediasi yang muncul.

4. Bila sudah tidak diperlukan, hapus kebijakan dan inisiatif.

## Configure Role-Based Access Control

Dalam bagian ini kita akan belajar tentang penetapan peran (role assignments).

**Reference**: [Tutorial: Grant a user access to Azure resources using the Azure portal - Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

**Reference**: [Quickstart - Check access for a user to Azure resources - Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/check-access)

**Locate Access Control blade**


1. Masuk ke **Azure Portal** dan pilih salah satu **Resource Group**. Catat nama resource group yang digunakan.

2. Pilih menu **Access Control (IAM)**.

3. Panel ini tersedia di berbagai jenis sumber daya untuk mengatur izin akses.

**Review role permissions**

1. Klik tab **Roles** di bagian atas.

2. Tinjau daftar **role bawaan (built-in roles)** yang tersedia.

3. Klik dua kali salah satu role, lalu pilih tab **Permissions**.

4. Jelajahi detail role hingga terlihat aksi **Read**, **Write**, dan **Delete**.

5. Kembali ke tab **Access Control (IAM)**.

**Add a role assignment**

1. Buat pengguna baru atau pilih pengguna yang sudah ada.

2. Klik **Add role assignment**, lalu pilih role, misalnya *Owner*.

3. Gunakan fitur **Check access** untuk meninjau hak akses pengguna tersebut.

4. Catat bahwa Anda juga dapat membuat **Deny assignment** jika dibutuhkan.
