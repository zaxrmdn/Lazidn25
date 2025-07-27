---
lab:
    title: 'Lab 02a - Manage Subscriptions and RBAC'
    module: 'Administer Governance and Compliance'
---

# Lab 02a - Lab 02a - Manage Subscriptions and RBAC

## Pendahuluan Lab

Dalam lab ini, Anda akan menjelajahi kontrol akses berbasis peran (Role-Based Access Control/RBAC) di Azure. Anda akan mempelajari bagaimana Azure menggunakan izin dan cakupan (scope) untuk menentukan tindakan yang dapat dan tidak dapat dilakukan oleh suatu identitas. Selain itu, Anda juga akan mempelajari bagaimana menyederhanakan manajemen langganan dengan menggunakan grup manajemen (management group).

> **Catatan:** Lab ini memerlukan langganan Azure aktif. Anda bisa mengubah wilayah tempat sumber daya dibuat. Instruksi lab menggunakan wilayah **East US**.

## Estimated timing: 30 minutes

## Lab scenario
Untuk menyederhanakan manajemen sumber daya Azure di organisasi Anda, Anda diminta untuk:

  - Membuat grup manajemen yang mencakup semua langganan Azure yang dimiliki organisasi.
  - Memberikan izin kepada tim helpdesk untuk:
  - Membuat dan mengelola mesin virtual.
  - Membuat permintaan dukungan Azure tanpa memberikan akses tambahan.

## Architecture diagram

![Diagram tugas lab](../media/az104-lab02a-architecture.png)

## Job skills

- Membuat dan mengonfigurasi grup manajemen.
- Meninjau dan menetapkan peran bawaan Azure.
- Membuat peran RBAC kustom.
- Memantau aktivitas terkait RBAC menggunakan log aktivitas.

---

## Task 1: Implement Management Groups

1. Masuk ke portal Azure: [https://portal.azure.com](https://portal.azure.com)
2. Buka layanan **Microsoft Entra ID**.
3. Buka bagian **Properties**, pastikan Anda memiliki akses ke seluruh langganan dan grup manajemen.
4. Cari dan buka layanan **Management groups**.
5. Klik **+ Create** dan masukkan:
   - **Management group ID**: `mg1-p1`
   - **Display name**: `mg1-p1`
6. Klik **Submit**.
7. Tunggu hingga `mg1-p1` muncul di daftar grup.

> **Catatan:** Grup manajemen root mencakup seluruh grup dan langganan. Ini berguna untuk menetapkan kebijakan global seperti RBAC dan Azure Policy.

---

## Task 2: Review and assign a built-in Azure role

1. Buka grup manajemen `mg1-p1`.
2. Arahkan ke menu **Access control (IAM)** dan buka tab **Roles**.
3. Telusuri daftar peran yang tersedia. Pelajari deskripsi, izin JSON, dan penetapan yang ada.
4. Klik **+ Add** > **Add role assignment**.
5. Pilih peran **Virtual Machine Contributor** > **Next**.
6. Pada bagian **Members**, cari dan pilih grup `helpdesk`. Jika belum ada, buat terlebih dahulu dari layanan Microsoft Entra ID.
7. Klik **Review + assign** dua kali.
8. Pastikan `helpdesk` sudah mendapatkan peran **Virtual Machine Contributor** di tab **Role assignments**.

> **Tips:** Disarankan menetapkan peran kepada grup, bukan ke pengguna individu.

---

## Task 3: Create a custom RBAC role

1. Di halaman grup manajemen `mg1-p1`, buka **Access control (IAM)**.
2. Klik **+ Add** > **Add custom role**.
3. Isi detail berikut:
   - **Name**: `Custom Support Request`
   - **Description**: `Peran kustom untuk membuat tiket dukungan Azure`
4. Pilih opsi **Start from JSON** atau **Clone a role**, dan pilih **Support Request Contributor**.
5. Di tab **Permissions**, klik **+ Exclude permissions**:
   - Cari `Microsoft.Support`
   - Hapus izin: `Microsoft.Support/*/register/action`
6. Pastikan scope penugasan mencakup grup `mg1-p1`.
7. Tinjau tab JSON, lalu klik **Create**.

---

## Task 4: Monitor role assignments with the Activity Log

1. Buka **mg1-p1** > **Activity log**.
2. Filter hasil log untuk menampilkan operasi terkait role assignment.
3. Tinjau hasilnya untuk memahami siapa yang menetapkan peran dan kapan.

---

## Cleanup your resources

Jika Anda menggunakan langganan pribadi dan ingin menghindari biaya tambahan:

- Hapus grup sumber daya melalui portal Azure.
- Atau gunakan PowerShell:

    ```powershell
    Remove-AzResourceGroup -Name <nama-resource-group>
    ```

- Atau gunakan Azure CLI:

    ```bash
    az group delete --name <nama-resource-group>
    ```

---

## Learn more with self-paced training

- [RBAC di Azure - Modul Microsoft Learn](https://learn.microsoft.com/training/modules/secure-azure-resources-with-rbac/)
- [Membuat Peran Kustom di Azure - Microsoft Learn](https://learn.microsoft.com/training/modules/create-custom-azure-roles-with-rbac/)

---

## Key takeaways

- Gunakan grup manajemen untuk menyederhanakan pengelolaan banyak langganan.
- Azure memiliki banyak peran bawaan untuk berbagai kebutuhan.
- Buat peran kustom jika peran bawaan tidak cukup.
- Role Assignment dapat diaudit menggunakan Activity Log.
