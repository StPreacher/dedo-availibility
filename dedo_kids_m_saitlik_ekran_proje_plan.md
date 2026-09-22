# PROJE BİLGİSİ VE BAĞLAM
**Proje Adı:** Dedo Kids Atölye Müsaitlik Ekranı
**Amaç:** Instagram hikaye reklamlarından gelen (10k-40k görüntülenme) velilere, atölye seanslarının doluluk durumunu göstererek "Kaçırma Korkusu (FOMO)" yaratmak ve rezervasyon sürecindeki sürtünmeyi (DM atıp sorma zorunluluğunu) ortadan kaldırmak.
**Hedef Kitle:** Ebeveynler (Mobil cihazlardan, Instagram içindeki tarayıcıdan girecekler).
**Tasarım Yaklaşımı:** %100 Mobile-First, temiz, güven veren ve hızlı açılan bir arayüz.

---

# TEKNİK MİMARİ VE GEREKSİNİMLER
Bu proje sıfır maliyetli ve yüksek trafik kaldırabilen "Serverless" bir yapıda kurgulanacaktır.

*   **Veritabanı:** Google Sheets (Eş zamanlı ve mobil uygulama üzerinden kolay güncelleme için).
*   **Veri Çekme Yöntemi:** Google Sheets API KULLANILMAYACAKTIR (Rate-limit sorunları yaşamamak için). Bunun yerine Google Sheets "Web'de Yayınla -> CSV" özelliği kullanılacak ve veri önyüzde `PapaParse` kütüphanesi ile ayrıştırılacaktır.
*   **Önyüz (Frontend):** Tek sayfa (Single Page). Sadece HTML, Vanilla JavaScript ve stil için Tailwind CSS (CDN üzerinden eklenebilir) kullanılacaktır. React vs. gibi ağır framework'lere gerek yoktur.
*   **Hosting:** Vercel veya Netlify (Statik site olarak).

---

# AI (ANTIGRAVITY) İÇİN GELİŞTİRME TALİMATLARI
Lütfen aşağıdaki yönergelere uyarak `index.html` (içinde JS ve Tailwind CSS dahil) dosyasını oluştur.

## 1. Veri Yapısı Beklentisi
JavaScript kodu, aşağıdaki sütun başlıklarına sahip bir CSV dosyasını çekecek şekilde yazılmalıdır:
*   `Tarih` (Örn: 24 Eylül Cumartesi)
*   `Saat` (Örn: 11:00 - 12:00)
*   `YasGrubu` (Örn: 3-5 Yaş)
*   `Kapasite` (Örn: 10)
*   `Kalan` (Örn: 2)

## 2. Mantık ve Kurallar (FOMO Tetikleyicileri)
Gelen verideki `Kalan` sayısına göre UI'da otomatik olarak aşağıdaki etiketleme (badge) sistemi çalışmalıdır:
*   **Eğer Kalan Kontenjan > 4 ise:** Sadece yeşil bir etiketle "Müsait" veya "Kayıtlar Açık" yazsın. (Sayıyı gösterme, negatif sosyal kanıt olmasın).
*   **Eğer Kalan Kontenjan 1 ile 4 arasında ise:** Turuncu/Kırmızı bir etiketle, dikkat çekici şekilde **"Son X Kişi"** yazsın (Örn: Son 2 Kişi). Puding/Titreşim efekti verilebilir.
*   **Eğer Kalan Kontenjan = 0 ise:** Gri bir etiketle "Doldu" yazsın ve satırın opaklığı (opacity) düşürülsün.

## 3. UI/UX Tasarımı
*   **Tema:** Çocuk atölyesine uygun ama ebeveynlere güven veren pastel tonlar (Örn: Pastel sarı, yumuşak turuncu ve temiz beyaz arka planlar).
*   **Header:** Dedo Kids logosu veya şık bir başlık. Altında ufak bir açıklama: "Atölye seanslarımızın güncel kontenjan durumunu aşağıdan takip edebilir, dolmadan yerinizi ayırtabilirsiniz."
*   **Liste Görünümü:** Tablo kullanma. Mobil ekranlar için her bir seans "Kart (Card)" tasarımı şeklinde alt alta listelensin.
*   **Aksiyon:** Sitenin en altında (veya her kartın içinde) "Hemen Kayıt Ol (WhatsApp)" butonu olsun.

---

# SENİN İÇİN ADIM ADIM UYGULAMA REHBERİ
*(Yapay zeka kodları sana verdikten sonra yapman gerekenler)*

### Adım 1: Google Sheets Hazırlığı (Şu an yapabilirsin)
1. Yeni bir Google E-Tablo oluştur.
2. İlk satıra başlıkları yaz: `Tarih`, `Saat`, `YasGrubu`, `Kapasite`, `Kalan`.
3. Altına test amaçlı 2-3 satır veri gir.
4. Sol üstten **Dosya -> Paylaş -> Web'de Yayınla** (File -> Share -> Publish to web) seçeneğine tıkla.
5. "Tüm Belge" (Entire Document) ve "Web Sayfası" yazan yeri **"Virgülle ayrılmış değerler (.csv)"** olarak değiştir.
6. "Yayınla" butonuna bas ve sana verdiği **Link'i kopyala**.

### Adım 2: Kodun Birleştirilmesi
1. AI'ın (Antigravity) sana verdiği `index.html` kodunun içine gir.
2. Kodun içinde `const SHEET_CSV_URL = "BURAYA_LINK_GELECEK";` yazan bir yer olacak. Oraya 1. Adımda kopyaladığın linki yapıştır.

### Adım 3: Yayına Alma (Vercel)
1. Kendine bir klasör aç, bu `index.html` dosyasını içine koy.
2. Klasörü GitHub'a bir repository olarak yükle (Eğer GitHub masaüstü uygulamasını kullanıyorsan saniyeler sürer).
3. Vercel.com'a gir, GitHub ile giriş yap.
4. "Add New Project" de, oluşturduğun repoyu seç ve "Deploy" butonuna tıkla.
5. Vercel sana `dedokids-musaitlik.vercel.app` gibi bir link verecek. Site yayında!

### Adım 4: İşleyiş (Seyahat Sırasında Melike'nin Yapacakları)
1. Melike telefonuna **Google E-Tablolar (Google Sheets)** uygulamasını indirsin.
2. Senin oluşturduğun tabloya o da erişebilsin (Düzenleyici olarak mailini ekle).
3. Yeni bir kayıt geldiğinde (Örn: Cumartesi 11:00 seansına), Melike sadece telefondan o tablodaki "Kalan" sütunundaki sayıyı 1 düşürecek ve çıkacak.
4. Web sitesine giren herkes anında "Son 2 Kişi" vb. güncel durumu görecek. Ekstra hiçbir teknik işlem yok!