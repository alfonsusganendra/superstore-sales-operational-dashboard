# Sales & Operational Performance Dashboard

## Latar Belakang
*Project* ini menganalisis data transaksi penjualan dan operasional logistik dari dataset Superstore Retail. 

Tujuan utama:
- Mengukur efektivitas pengiriman (*On-Time Delivery Rate*) per *Ship Mode*.
- Menganalisis kontribusi *Sales* dan *Profit* per *Region*.
- Mengidentifikasi *bottleneck* operasional melalui *Average Lead Time*.
- Memberikan *insight* kepada tim Sales Planning untuk optimasi distribusi.

## Tools & Technology
- **Microsoft Excel:** Untuk *data cleaning*, *feature engineering* (kolom 'Lead Time' dan 'Delivery Status') dan pembuatan PivotTable sebagai *tools* validasi KPI.
- **Microsoft Power BI**: Untuk pembuatan *dashboard* interaktif dengan *conditional formatting*, *slicer* dan *cross-filtering* dinamis.

## Sumber Data
*Dataset* yang digunakan adalah **Superstore Retail Dataset** (dataset publik). *Dataset* ini berisi 9.994 baris dengan rentang waktu 2014 - 2017, terdiri dari antara lain:
- Metrik ("Sales", "Profit", "Quantity", "Discount").
- Dimensi ("Ship Mode", "Region", "Category", "Sub-Category")
- Waktu ("Order Date", "Ship Date").

Catatan:
*Dataset* ini merupakan *synthetic data* untuk keperluan latihan BI. Beberapa anomali seperti *Profit* > *Sales* di beberapa *region* tidak merepresentasikan logika bisnis yang nyata. 

## Proses Analisis
1. **Data Preparation:** Memperbaiki *mismatch* pada beberapa kolom menggunakan *Find & Replace* dan *Text to Columns* di Microsoft Excel.
2. **Feature Engineering:** Membuat kolom baru "Lead Time (Days)" (selisih *Ship Date* - *Order Date*) dan "Delivery Status" (On Time/Standard/Delayed) menggunakan formula di Microsoft Excel.
3. **Exploratory Data Analysis:** Membangun 3 Pivot Table di Excel sebagai *benchmark* KPI:
   a. *On-Time Delivery Rate* per *Ship Mode*,
   b. *Sales* & *Profit* per *Region*.
   c. *Average Lead Time* per *Region*.
4. **Visualisasi di Power BI:**
   a. Mengimpor *Fact Table* (`tSuperstore`) dan *Dimension Table* (`tRegion`) ke Power BI.
   b. Membangun 4 KPI Cards, 3 visualisasi utama dengan *conditional formatting*, dan 3 *slicer* interaktif.

## Contoh Formula Excel (Feature Engineering)
```excel
// Kolom "Lead Time (Days)" di Excel
=DAYS([@[Ship Date]], [@[Order Date]])

// Kolom "Delivery Status" di Excel
=IF([@[Lead Time (Days)]] <= 2, "On Time", IF([@[Lead Time (Days)]] <= 5, "Standard", "Delayed"))
```

## Key Insights & Rekomendasi
1. **Standard Class memiliki tingkat keterlambatan 31%** 
- *Ship Mode* ini menjadi *bottleneck* utama dengan 31% transaksi berstatus *Delayed*.
- **Rekomendasi:** Lakukan audit Service Level Agreement (SLA) dengan vendor logistik untuk *Standard Class*.

2. **Average Lead Time sangat konsisten antar Region** 
- Selisih rata-rata *Lead Time* antar *region* hanya 0.2 hari (Contoh: Central: 4.1 hari vs East/West: 3.9 hari), artinya sistem pengiriman sudah terstandarisasi.
- **Rekomendasi:** Fokus perbaikan bukan pada *gap* antar *region*, melainkan pada **pengurangan Lead Time secara keseluruhan**.

3. **Anomali Profitabilitas di 3 dari 4 Region** 
- *West*, *East*, dan *South* menunjukkan *Profit* > *Sales* (contoh *West*: *Profit* $0.71bn vs *Sales* $0.33bn), yang secara logika bisnis tidak masuk akal.
- **Rekomendasi:** Sebelum *dashboard* digunakan untuk keputusan bisnis, perlu dilakukan analisa mendalam dan diskusi terlebih dahulu ke tim Data Engineering untuk validasi *pipeline* data.

## Tantangan dan Pembelajaran
1. **Angka dengan format Float di Conditional Formatting:** Saat membuat *conditional formatting* untuk *Average Lead Time*, saya menemukan *region* dengan nilai rata-rata 3.9 (East & West) muncul berwarna kuning, padahal seharusnya hijau. Setelah dianalisa, ternyata Power BI menyimpan nilai tersebut sebagai 3.9000000000000004 (floating point), sehingga rule `<= 3.9` tidak terpenuhi. Saya memperbaikinya dengan mengubah range.
- Pembelajaran: gunakan *range* yang lebih toleran dengan data yang ada.

2. **Validasi Data menggunakan PivotTable Excel:** Saya hanya mengimpor *Fact Table* dan *Dimension Table* ke Power BI, karena Pivot Table Excel saya gunakan sebagai *tools* untuk memvalidasi angka di Power BI. (Contoh: ketika Card Total Sales di Power BI muncul `1.01bn`, saya langsung cek ke Pivot Table Excel (`1.006.076.552`) untuk memastikan akurasin datanya). 
- Pembelajaran: *dashboard* yang valid harus punya *benchmark* yang jelas.

## Tampilan Dashboard
<img width="1151" height="649" alt="dashboard_screenshot" src="https://github.com/user-attachments/assets/739a5c39-e7e3-4118-9de9-6c8f7058af30" />

## Struktur File Repository
- 'README.md': Dokumentasi lengkap *project* ini.
- 'dashboard_screenshot.png': *Screenshot* tampilan *dashboard* Power BI.
- 'Superstore_Sales_Operational_Dashboard.pbix': File Power BI yang dapat dibuka di Power BI Desktop.
- 'Superstore_Analysis_Raw.xlsx': File Excel yang berisi data mentah, *feature engineering* dan Pivot Table.
- 'dashboard_export.pdf': *Export* PDF *dashboard* untuk keperluan portofolio.

--------
Dibuat oleh saya, **Alfonsus Ganendra W. G. K.** sebagai bagian dari portfolio untuk *role* Data Analyst dan sejenisnya.
