# FlowMind 🌊
### AI Destekli Kişisel Verimlilik Asistanı

> **"Bir cümle söyle, sistem halleder."**

FlowMind, doğal dilde yazdığınız görevleri yapay zeka ile analiz edip otomatik olarak Google Sheets'e kaydeden, Google Calendar'a işleyen ve her sabah Gmail üzerinden özet gönderen bir no-code verimlilik asistanıdır.

---

## 🎯 Problem

Bireyler günlük sorumluluklarını onlarca farklı platformda yönetmek zorunda kalıyor. E-posta bir yerde, takvim başka bir uygulamada, yapılacaklar listesi ayrı bir araçta. FlowMind bunu tek bir cümleyle çözüyor.

---

## ✨ Özellikler

- **Doğal Dil Girişi** — Form yok, kategori seçimi yok. Sadece ne yapman gerektiğini yaz.
- **AI Analizi** — Gemini 2.5 Flash, mesajı profil bazlı analiz eder; öncelik, kategori ve deadline tahmini üretir.
- **Otomatik Takvim** — Tarih/saat içeren görevler Google Calendar'a otomatik eklenir.
- **Sabah Brifing** — Her sabah 08:30'da profil bazlı Gmail özeti gönderilir.
- **Haftalık Özet** — Her Pazar 20:00'de tamamlanan/eksik görevler özetlenir.
- **4 Kullanıcı Profili** — Profesyonel, Öğrenci, Freelancer, Ekip Üyesi.

---

## 🖥️ Arayüz

### Glide — Görev Listesi & Form
![Glide Görevler](images/glide_gorevler.png)
![Glide Form](images/glide_form.png)

### Glide — Takvim Görünümü
![Glide Takvim](images/glide_takvim.png)

### Gmail — Sabah Brifing
![Gmail Brifing](images/gmail_brifing.png)

---

## ⚙️ Teknoloji Yığını

| Araç | Rol | Açıklama |
|------|-----|----------|
| **n8n** | Otomasyon | Tüm workflow'lar burada tanımlanır |
| **Gemini 2.5 Flash** | Yapay Zeka | Metin analizi ve profil bazlı cevap üretimi |
| **Google Sheets** | Veritabanı | Kullanıcı profilleri, görevler, takvim ve log |
| **Glide** | Arayüz | Web ve mobil uygulama |
| **Google Calendar** | Takvim | Otomatik etkinlik oluşturma |
| **Gmail** | Bildirim | Sabah brifing ve haftalık özet |
| **Google Drive** | Belge | Profil bazlı not yönetimi |
| **Google Forms** | Onboarding | İlk kullanımda profil seçimi |

---

## 🔄 Workflow Yapısı

### Workflow 1 — Ana Yönlendirici
Sheets'e yeni satır eklenince tetiklenir → Gemini analiz eder → Sheets güncellenir + Calendar'a etkinlik eklenir.

![Workflow 1](images/workflow1.png)

### Workflow 2 — Zamanlı İşler
Sabah 08:30'da brifing, Pazar 20:00'de haftalık özet gönderir.

![Workflow 2](images/workflow2.png)

### Workflow 3 — Profil Kurulum
Google Forms'tan gelen yeni yanıtı Sheets'teki Kullanıcılar sekmesine kaydeder.

![Workflow 3](images/workflow3.png)

---

## 🗂️ Veritabanı Yapısı (Google Sheets)

**FlowMind - Veritabani** dosyası 4 sekmeden oluşur:

| Sekme | İçerik |
|-------|--------|
| `Kullaniciler` | Ad, e-posta, profil bilgisi |
| `Gorevler` | Mesaj, intent, metin, öncelik, kategori, deadline, tarih, profil |
| `Takvim` | Etkinlik adı, başlangıç/bitiş, Calendar ID |
| `Log` | İşlem tarihi, kullanıcı, işlem tipi, detay, durum |

---

## 👤 Kullanıcı Profilleri

| Profil | Odak Alanı |
|--------|-----------|
| **Profesyonel** | Toplantı, müşteri, sunum, teklif |
| **Öğrenci** | Ödev, sınav, deadline hassasiyeti |
| **Freelancer** | Proje, fatura, müşteri takibi |
| **Ekip Üyesi** | Görev dağıtımı, ortak takvim yönetimi |

---

## 🚀 Kurulum

### Gereksinimler
- n8n (local veya cloud)
- Google Cloud hesabı (Sheets, Calendar, Gmail, Drive API'leri aktif)
- Gemini API key
- Glide hesabı

### Adımlar

1. **Google Sheets** — `FlowMind - Veritabani` adında bir Sheets dosyası oluşturun. 4 sekme ekleyin: `Kullaniciler`, `Gorevler`, `Takvim`, `Log`. Her sekmeye ilgili başlık satırlarını ekleyin.

2. **n8n Credentials** — Google Sheets, Google Calendar, Gmail ve Gemini API için n8n'de credential'ları oluşturun.

3. **Workflow'ları İçe Aktarın** — Bu repodaki 3 JSON dosyasını n8n'e import edin:
   - `FlowMind_Workflow1_Ana_Yonlendirici.json`
   - `FlowMind_Workflow2_Zamanli_Isler.json`
   - `FlowMind_Workflow3_Profil_Kurulum.json`

4. **Credential'ları Güncelleyin** — Her workflow'daki `YOUR_CREDENTIAL_ID` ve `YOUR_SPREADSHEET_ID` alanlarını kendi değerlerinizle değiştirin.

5. **Glide** — Glide'da yeni bir uygulama oluşturun, Google Sheets'i veri kaynağı olarak bağlayın.

6. **Workflow'ları Aktifleştirin** — n8n'de 3 workflow'u da "Active" yapın.

> ⚠️ n8n local çalıştırıyorsanız demo sırasında açık kalması gerekir.

---

## 📁 Repo Yapısı

```
flowmind/
├── README.md
├── workflows/
│   ├── FlowMind_Workflow1_Ana_Yonlendirici.json
│   ├── FlowMind_Workflow2_Zamanli_Isler.json
│   └── FlowMind_Workflow3_Profil_Kurulum.json
└── images/
    ├── glide_gorevler.png
    ├── glide_form.png
    ├── glide_takvim.png
    ├── gmail_brifing.png
    ├── workflow1.png
    ├── workflow2.png
    └── workflow3.png
```

---

## 👥 Takım

**Takım 4 — YZTA Hackathon 2026 | No-Code / Low-Code**

| No | İsim | Rol |
|----|------|-----|
| 1608 | Enise Cömet | Teknik Öncü — n8n workflow mimarisi, Gemini entegrasyonu, Google Sheets & Calendar bağlantıları |
| 1570 | Ahmet Kağan Ertürk | Arayüz Sorumlusu — Glide dashboard tasarımı, veri bağlantıları |
| 1617 | Yasemin Akgül | Sunum & Teslim — Demo video, pitch deck, teslim formu |

---

## 🏆 Hackathon

**Yapay Zeka ve Akademi Hackathon 2026**
No-Code / Low-Code Kategorisi

---

*Bu proje YZTA Hackathon 2026 kapsamında geliştirilmiştir. Tüm araçlar ücretsiz katmanlarda kullanılmıştır.*
