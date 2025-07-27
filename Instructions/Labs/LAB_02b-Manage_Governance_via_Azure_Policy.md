---
lab:
    title: 'Lab 02b - Manage Governance via Azure Policy'
    module: 'Administer Governance and Compliance'
---

# Lab 02b - Manage Governance via Azure Policy

## Lab introduction

Dalam lab ini, Anda akan mempelajari cara mengimplementasikan rencana tata kelola organisasi. Anda akan belajar bagaimana Azure Policy dapat memastikan keputusan operasional ditegakkan di seluruh organisasi. Anda juga akan belajar cara menggunakan tag sumber daya untuk meningkatkan pelaporan.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur. Anda dapat mengganti region, namun langkah-langkah ditulis menggunakan **East US**.

## Estimated timing: 30 minutes

## Lab scenario

Jejak cloud organisasi Anda telah berkembang pesat tahun ini. Dalam audit terakhir, Anda menemukan banyak sumber daya tidak memiliki pemilik, proyek, atau pusat biaya yang jelas. Untuk meningkatkan pengelolaan sumber daya Azure, Anda memutuskan untuk menerapkan:

- Penambahan tag pada sumber daya sebagai metadata penting
- Penegakan penggunaan tag menggunakan Azure Policy
- Pembaruan tag pada sumber daya yang sudah ada
- Penggunaan resource lock untuk melindungi konfigurasi

## Architecture diagram

![Diagram arsitektur tugas.](../media/az104-lab02b-architecture.png)

## Job skills

+ Tugas 1: Membuat dan menetapkan tag melalui portal Azure
+ Tugas 2: Menegakkan penggunaan tag melalui Azure Policy
+ Tugas 3: Menerapkan tag melalui Azure Policy
+ Tugas 4: Mengonfigurasi dan menguji resource locks

## Task 1: Assign tags via the Azure portal

1. Masuk ke **Azure Portal** - `https://portal.azure.com`
2. Cari dan pilih `Resource groups`
3. Klik **+ Create**

    | Pengaturan | Nilai |
    |------------|-------|
    | Subscription | langganan Anda |
    | Resource group name | `rg2-p1` |
    | Location | **East US** |

4. Klik **Next**, lalu masuk ke tab **Tags**:

    | Pengaturan | Nilai |
    |------------|-------|
    | Name | Cost Center |
    | Value | 000 |

5. Klik **Review + Create** lalu **Create**

## Task 2: Enforce tagging via an Azure Policy

1. Di portal, cari `Policy`
2. Pilih **Definitions** lalu cari `Require a tag and its value on resources`
3. Pilih kebijakan dan klik **Assign policy**
4. Tetapkan **Scope** ke:

    | Pengaturan | Nilai |
    |------------|-------|
    | Subscription | langganan Anda |
    | Resource Group | `rg2-p1` |

5. Di tab **Basics**:

    | Pengaturan | Nilai |
    |------------|-------|
    | Assignment name | `Require Cost Center tag and its value on resources` |
    | Description | `Menetapkan tag Cost Center pada semua resource dalam resource group` |
    | Enforcement | Enabled |

6. Di tab **Parameters**:

    | Pengaturan | Nilai |
    |------------|-------|
    | Tag Name | `Cost Center` |
    | Tag Value | `000` |

7. Klik **Review + Create** dan klik **Create**

8. Cobalah membuat storage account di RG tersebut **tanpa menambahkan tag**, dan perhatikan akan ada error **Validation failed**

## Task 3: Apply tagging via an Azure policy

1. Kembali ke `Policy > Assignments` dan hapus kebijakan sebelumnya
2. Klik **Assign policy** dan pilih kebijakan `Inherit a tag from the resource group if missing`
3. Scope:

    | Pengaturan | Nilai |
    |------------|-------|
    | Subscription | langganan Anda |
    | Resource Group | `rg2-p1` |

4. Di tab **Basics**:

    | Pengaturan | Nilai |
    |------------|-------|
    | Assignment name | `Inherit the Cost Center tag and its value 000` |
    | Description | sama |
    | Enforcement | Enabled |

5. Di **Parameters**:

    | Pengaturan | Nilai |
    |------------|-------|
    | Tag Name | `Cost Center` |

6. Di tab **Remediation**:

    | Pengaturan | Nilai |
    |------------|-------|
    | Create remediation task | Enabled |

7. Klik **Review + Create** dan klik **Create**
8. Buat lagi storage account baru tanpa tag, dan verifikasi tag otomatis ditambahkan

## Task 4: Configure and test resource locks

1. Buka resource group `rg2-p1`
2. Pilih **Locks > Add**

    | Pengaturan | Nilai |
    |------------|-------|
    | Lock Name | `rg-lock` |
    | Lock Type | **Delete** |

3. Coba hapus resource group. Akan muncul **error** karena terkunci
4. Hapus lock jika ingin menghapus resource group

## Cleanup your resources

Jika Anda menggunakan langganan sendiri, hapus resource group untuk menghindari biaya:

+ Di portal: buka RG > **Delete resource group**
+ PowerShell: `Remove-AzResourceGroup -Name rg2-p1`
+ CLI: `az group delete --name rg2-p1`

## Extend your learning with Copilot

Beberapa pertanyaan yang bisa Anda coba di Copilot:

- Perintah CLI & PowerShell untuk mengelola resource locks
- Perbedaan Azure Policy dan RBAC
- Cara remediasi resource non-compliant
- Cara membuat laporan berdasarkan tag

## Learn more with self-paced training

+ [Desain strategi tata kelola enterprise](https://learn.microsoft.com/training/modules/enterprise-governance/)

## Key takeaways

+ Azure Tags = metadata key-value
+ Azure Policy = menetapkan aturan kepatuhan sebelum deployment
+ Remediation = memperbaiki resource yang sudah ada
+ Locks = mencegah modifikasi/penghapusan
+ Azure Policy = pre-deployment, sedangkan RBAC dan locks = post-deployment
