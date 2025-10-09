# 📧 Konuşarak Öğren - Email Automation

Bu proje, **AI Growth Stajı Ön Çalışması** kapsamında geliştirilen, **Konuşarak Öğren** için tasarlanmış otomatik e-posta takip sistemidir. Sistem tamamen **n8n**, **Google Sheets**, **Gmail** ve **OpenAI** entegrasyonları ile oluşturulmuştur.

---

## 🚀 Genel Amaç

Potansiyel müşterilere yönelik takip e-postalarını manuel hazırlama sürecini otomatikleştirmek, e-posta performansını izlemek ve raporlamak.  
Amaç, **email hazırlama süresini %80 azaltmak**, **takip sürecinde tutarlılık sağlamak** ve **dönüşüm oranlarını %15-20 artırmak**.

---

## ⚙️ Akış Özeti

### **1️⃣ Günlük Tetikleme**
- `Daily Trigger 10:00`: Her sabah saat 10:00’da çalışır.

### **2️⃣ Veri Alma**
- `Fetch Leads`: Google Sheets’ten `Leads!A:D` aralığındaki verileri çeker.  
  > 🟨 **Not:** `your_google_sheet_id` kısmını kendi Google Sheet ID’nizle değiştirin.

### **3️⃣ Veri Hazırlama**
- `Normalize Leads`: Satır verilerini Name, Email, Status, LastContact formatına dönüştürür.
- `Segment by Status`: Kullanıcıları statülerine göre ayırır:
  - `TrialNotPurchased`
  - `FormNoTrial`
  - `Other`

### **4️⃣ Email Tipi Atama**
- `Set Type: TrialNotPurchased`
- `Set Type: FormNoTrial`
- `Set Type: Default`

### **5️⃣ Takip Linki Oluşturma**
- `Build Tracking URLs`: Her alıcı için benzersiz takip ID’si (`trackingId`), açılma pikseli (`openPixel`) ve tıklama linki (`clickUrl`) oluşturur.  
  > 🟨 **Not:** `https://your-n8n-host` kısmını kendi n8n host adresinizle değiştirin.

### **6️⃣ AI ile İçerik Üretimi**
- `AI: Generate Subject & Body`: OpenAI API ile Türkçe, 80–120 kelimelik HTML formatında e-posta ve konu başlığı üretir.
- `Merge AI + Tracking`: Takip linklerini HTML içeriğine ekler.
- `Send Email - Day 0`: Gmail üzerinden e-posta gönderir.
- `Log Email Day 0`: Gönderim bilgilerini `EmailLogs!A:D` sayfasına yazar.

---

## ⏳ Zaman Bazlı Devam Gönderimleri

### **Day 2 (2 Gün Sonra)**
- `Wait 2 Days` → `Set Type: Day 2` → `AI: Day 2 Content` → `Merge Day 2` → `Send Email - Day 2` → `Log Email Day 2`

### **Day 5 (5 Gün Sonra)**
- `Wait 3 More Days` → `Set Type: Day 5` → `AI: Day 5 Content` → `Merge Day 5` → `Send Email - Day 5` → `Log Email Day 5`

---

## 📊 Takip & Analiz

### **Açılma ve Tıklama Takibi**
- `Webhook - Open` → `Log Open` → `OpenLogs!A:D`
- `Webhook - Click` → `Log Click` → `ClickLogs!A:E`

Her e-postaya 1x1 piksel görünmez bir GIF eklenerek açılma kaydı alınır.  
Tıklama linkleri yönlendirilmeden önce webhook üzerinden loglanır.

---

## 📈 Raporlama

- `Get Email Logs` ve `Get Click Logs` nodları, logları okur.
- `Calculate Stats (by messageType)`:
  - Gönderilen e-posta sayısı  
  - Tıklama sayısı  
  - Tıklanma oranı (%)  
  - En iyi performans gösteren mesaj tipi (`best_message`)
- `Append Report`: Sonuçlar `Report!A:D` sayfasına eklenir.

### 📋 Örnek Google Sheet Tablosu
🔗 [Örnek Tablo (Google Sheets)](https://docs.google.com/spreadsheets/d/1Xu4a1L4Ot2eGeNiebp10nfzxAlR6aJMS6ufNzoxtHMM/edit?usp=sharing)

---

## 📒 Sticky Notlar (README için n8n içinde görünür açıklamalar)

| Node | Açıklama |
|------|-----------|
| **Fetch Leads** | `your_google_sheet_id` değerini değiştirmeyi unutmayın. |
| **Build Tracking URLs** | `your-n8n-host` alanına kendi n8n URL’nizi ekleyin. |
| **AI: Generate Subject & Body** | OpenAI kimliğini güncelleyin (`YOUR_OPENAI_CRED_ID`). |
| **Send Email - Day 0/2/5** | Gmail OAuth2 kimliğini güncelleyin (`YOUR_GMAIL_CRED_ID`). |
| **Google Sheets nodları** | Hepsi aynı kimliği kullanır (`YOUR_GOOGLE_SHEETS_CRED_ID`). |
| **Webhook - Click / Open** | Bunlar public endpoint’lerdir; erişim için SSL kullanmanız önerilir. |

---

## 🧠 Kullanılan Yapay Zeka Bileşenleri

- **Yapay Zeka Aracı:** ChatGPT  
  Bu projede ChatGPT’yi özellikle **planlama aşamasında**, **öğrenme sürecinde** ve benzer JSON dosyalarını analiz etmede aktif olarak kullandım.  
  Ayrıca bu README dosyasının hazırlanma sürecinde de destek aldım.

- **OpenAI GPT (n8n-nodes-base.openAi):**
  - Segment bazlı e-posta içeriği oluşturma  
  - Kısa ve CTA odaklı HTML üretimi

- **AI destekli analiz (function node):**
  - En iyi performans gösteren e-posta tipini belirleme

---

## 🎥 Öğrenme Kaynakları

Bu projeyi hazırlamadan önce n8n platformunu daha iyi anlamak için şu videoları izledim:

1. [📺 n8n ile Yapay Zeka Ajanları Kur ve Sat (5 Saatlik Eğitim – Sıfır Kodlama)](https://www.youtube.com/watch?v=PiOuEEBvY6A)  
2. [📺 N8N ÜCRETSİZ HOST İLE SINIRSIZ KULLANMA](https://www.youtube.com/watch?v=WbH3GDBw13A)

---

## ✅ Kurulum Adımları

1. `n8n-konusarak-ogren-mail-case.json` dosyasını n8n'e **Import Workflow** ile yükleyin.  
2. `Credentials` sekmesinden:
   - Google Sheets (OAuth2)
   - Gmail (OAuth2)
   - OpenAI API anahtarlarını ekleyin.
3. Sticky notlarda belirtilen alanları (`your_google_sheet_id`, `your-n8n-host`) güncelleyin.
4. Workflow’u **Active** hale getirin.
5. Google Sheets üzerinde:
   - **Leads**, **EmailLogs**, **ClickLogs**, **OpenLogs**, **Report** sayfalarını oluşturun.
6. İlk tetikleme sonrasında sistem otomatik çalışmaya başlayacaktır.

---

## 🧩 Beklenen Etki

| Metrik | Önce | Sonra | Değişim |
|--------|------|--------|----------|
| Email hazırlama süresi | 10 dk | 2 dk | ⬇️ %80 |
| Takip tutarlılığı | Düşük | Yüksek | ✅ |
| Dönüşüm oranı | %5 | %6–7 | ⬆️ +%15–20 |

---

## 👤 Hazırlayan

**Ömer Faruk Özer**  
AI Growth Intern Case Study - Konuşarak Öğren  
📧 ozeromerfaruk@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/omerozerf/) | [GitHub](https://github.com/omerozerf)
