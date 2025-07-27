---
demo:
    title: 'Demonstration 03: Administer Azure Resources'
    module: 'Administer Administer Azure Resources'
---
# 03 - Administer Azure Resources

## Demonstration -- Azure Portal


Dalam demonstrasi ini, kita akan menjelajahi portal Azure.

**Reference**: [Manage Azure portal settings and preferences](https://docs.microsoft.com/azure/azure-portal/set-preferences)

**Reference**: [Create a dashboard in the Azure portal](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

**Reference**: [How to create an Azure support request](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

### Langkah-langkah:

1. Akses **Azure Portal**.
2. Klik ikon **Support & Troubleshooting** di bagian atas. Tinjau tautan **Support resources**.
3. Klik ikon **Settings** di bagian atas. Tinjau pengaturan **Appearance + startup views**.
4. Gunakan menu di sisi kiri dan pilih **Dashboard**. Klik **Edit**, lalu gunakan **Tile Gallery** untuk menyesuaikan tampilan dashboard.
5. Tunjukkan cara mencari dan menemukan sumber daya (*resources*).
6. Gunakan menu pojok kiri atas untuk membuka **All services**.
7. Jika waktu memungkinkan, tinjau fitur-fitur lainnya.
8. Ajak peserta untuk bertanya jika ada yang belum dipahami.

---

## ☁️ Demonstrasi — Cloud Shell

Dalam bagian ini, kita akan bereksperimen dengan **Azure Cloud Shell**.

📚 **Referensi**:  
[Quickstart: Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

### 🔧 Mengonfigurasi Cloud Shell

1. Masuk ke **Azure Portal**.
2. Klik ikon **Cloud Shell** di bagian atas.
3. Di halaman "Welcome to the Shell", pilih antara **Bash** atau **PowerShell**. Pilih **PowerShell**.
4. Bahas bahwa Cloud Shell membutuhkan **Azure File Share** untuk menyimpan file. Jika diperlukan, buat storage-nya terlebih dahulu.

### 🧪 Mencoba PowerShell dan Bash di Cloud Shell

1. Pastikan mode **PowerShell** aktif, lalu coba beberapa perintah seperti:
   - `Get-AzSubscription`
   - `Get-AzResourceGroup`
2. Tunjukkan fitur auto-complete, serta perintah seperti `cls` untuk membersihkan layar.
3. Beralih ke **Bash**, lalu coba perintah:
   - `az account list`
   - `az resource list`
4. Ajak peserta untuk bertanya jika ada kebingungan terkait perintah PowerShell atau Bash.

### 📝 Mencoba Cloud Shell Editor (opsional)

1. Klik ikon **kurung kurawal** (`{}`) untuk membuka editor.
2. Pilih file dari panel navigasi kiri, misalnya `.profile`.
3. Tinjau menu editor di bagian atas (ukuran teks, font, unggah/unduh).
4. Klik ikon titik tiga (...) di kanan atas untuk melihat opsi **Save**, **Close Editor**, dan **Open File**.
5. Bereksperimen sebentar, lalu **tutup editor** dan **tutup Cloud Shell**.

---

## ⚡ Demonstrasi — QuickStart Templates

Dalam demonstrasi ini, kita akan menjelajahi **QuickStart Templates**.

📚 **Referensi**:  
[Tutorial: Membuat dan menjalankan template ARM](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

### Langkah-langkah:

1. Kunjungi [galeri Azure Quickstart Templates](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager). Tinjau contoh **JSON** dan **Bicep**.
2. Tanyakan kepada peserta apakah ada template tertentu yang menarik. Jika tidak, pilih satu contoh, misalnya:  
   [Deploy a simple Windows VM with tags](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/)
3. Bahas fitur **Deploy to Azure**, yang memungkinkan Anda mengirim template langsung ke portal.
4. Lakukan **deploy** template JSON, bahas cara mengedit file template dan parameter. Jika ada waktu, tinjau sintaks JSON-nya.
5. Kembali ke galeri dan pilih contoh **template Bicep**, misalnya:  
   [Create a Standard Storage Account](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/)
6. Lakukan **deploy** template Bicep, dan ulangi pembahasan tentang file template serta parameternya. Jika ada waktu, bahas juga sintaks Bicep.
