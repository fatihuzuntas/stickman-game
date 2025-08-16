# 🎮 Çöp Adam Canvas Demo

Modern web teknolojileri ile geliştirilmiş, interaktif çöp adam animasyon oyunu. HTML5 Canvas, saf JavaScript ve localStorage kullanarak tam özellikli bir karakter düzenleme ve çoklu sahne yönetim sistemi.

![Demo Preview](https://img.shields.io/badge/Status-Live%20Demo-brightgreen)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow)
![Canvas](https://img.shields.io/badge/Canvas-HTML5-blue)
![LocalStorage](https://img.shields.io/badge/Storage-LocalStorage-orange)

## ✨ Özellikler

### 🎯 **Temel Oyun Mekanikleri**
- **Akıcı Hareket:** Delta-time tabanlı 60 FPS oyun döngüsü
- **Çoklu Seçim:** Ctrl/Cmd ile birden fazla karakter seçimi ve eşzamanlı kontrol
- **Sınır Kontrolü:** Karakterler ekran dışına çıkamaz
- **Responsive:** Tüm ekran boyutlarında uyumlu çalışma
- **Yüksek DPI:** Retina ve 4K ekranlarda net görüntü

### 🎨 **Karakter Özelleştirme**
- **Dinamik Renkler:** Her karakter için özel çizgi rengi
- **Baş Resimleri:** PNG/JPG dosyaları ile özel baş görselleri
- **Ekipman Sistemi:** Sol ve sağ el için emoji veya özel resim ekipmanları
- **Canlı Önizleme:** Düzenleme sırasında anlık görsel geri bildirim
- **Animasyon:** Hareketli yürüme, dururken açık poz

### 🖼️ **Sahne Yönetimi**
- **Çoklu Sayfa:** Sınırsız sayfa oluşturma ve yönetimi
- **Arka Plan Sistemi:** Hazır 2D desenler + özel resim yükleme
- **LocalStorage:** Tüm veriler tarayıcıda kalıcı saklanır
- **Otomatik Kaydetme:** Değişiklikler anında kaydedilir

### 🎮 **Kontrol Sistemi**
- **Klavye:** Ok tuşları ile hareket, P (duraklat), R (ortala)
- **Dokunma:** Mobil uyumlu dört bölgeli dokunmatik kontrol
- **Fare:** Sadece basılı sürükleme (takip etmez)
- **Çoklu Seçim:** Panel ve sahnede çoklu karakter seçimi

## 🚀 Hızlı Başlangıç

### Kurulum
```bash
# Projeyi klonlayın
git clone https://github.com/username/stickman-canvas-demo.git
cd stickman-canvas-demo

# Dosyayı açın (herhangi bir web sunucusu gerekmez)
open index.html
```

### Kullanım
1. **Oyun Kontrolü:**
   - ⬅️➡️⬆️⬇️ Ok tuşları ile hareket
   - `P` Duraklat/Devam et
   - `R` Tüm karakterleri ortala
   - `Ctrl + Tıklama` Çoklu seçim

2. **Ayarlar Paneli:**
   - Sağ alttaki ⚙️ butonuna tıklayın
   - 4 sekme: Oyun, Karakterler, Arka Plan, Sayfalar

3. **Karakter Düzenleme:**
   - Karakterler sekmesinde kartları kullanın
   - Renk, baş resmi, ekipman ayarları
   - Canlı önizleme ile anlık görsel geri bildirim

## 🏗️ Teknik Mimari

### Ana Bileşenler

```javascript
// Oyun motoru - Ana koordinatör
class Game {
  constructor(canvas, hudEl) {
    this.stickmen = [];           // Karakter listesi
    this.selected = new Set();    // Seçili karakterler
    this.state = null;            // Uygulama durumu
    this.input = new Input();     // Giriş yöneticisi
  }
}

// Karakter sınıfı - Her çöp adam
class Stickman {
  constructor(x, y, headImage, color, leftHand, rightHand) {
    this.x = x;                   // Konum
    this.headImage = headImage;   // Baş resmi
    this.color = color;           // Renk
    this.leftHand = leftHand;     // Sol el ekipmanı
    this.rightHand = rightHand;   // Sağ el ekipmanı
  }
}

// Giriş yöneticisi - Klavye + Dokunma
class Input {
  getDirection() {
    // Klavye ve dokunma girişlerini birleştir
    // Diyagonal hız normalizasyonu
  }
}
```

### Oyun Döngüsü

```javascript
loop(now) {
  const dt = (now - this.lastTime) / 1000;  // Delta time
  
  // 1. Güncelleme
  if (!this.paused) {
    for (const s of this.stickmen) {
      s.update(dt, input, width, height);
    }
  }
  
  // 2. Çizim
  this.drawBackground();
  for (const s of this.stickmen) {
    s.draw(this.ctx);
  }
  
  // 3. Sonraki kare
  requestAnimationFrame(this.loop);
}
```

### Veri Yapısı

```javascript
const state = {
  currentPageIndex: 0,
  pages: [
    {
      id: 'page-abc123',
      name: 'Sayfa 1',
      bgImageDataUrl: 'data:image/png;base64,...',
      stickmen: [
        {
          x: 100, y: 200,
          color: '#f5f5f5',
          headImageDataUrl: 'data:image/png;base64,...',
          leftHand: { type: 'emoji', value: '🔫', size: 22 },
          rightHand: null
        }
      ]
    }
  ]
}
```

## 🎨 Özelleştirme

### Yeni Arka Plan Ekleme

```javascript
const NEW_BACKGROUND = {
  name: 'Custom Pattern',
  draw: (ctx, width, height) => {
    // Canvas çizim kodları
    ctx.fillStyle = '#1a1a1a';
    ctx.fillRect(0, 0, width, height);
    
    // Özel desen çizimi
    ctx.strokeStyle = '#333';
    // ... desen kodları
  }
};
```

### Ekipman Sistemi

```javascript
// Emoji ekipman
const emojiEquip = {
  type: 'emoji',
  value: '🔫',
  size: 22
};

// Özel resim ekipman
const imageEquip = {
  type: 'image',
  value: HTMLImageElement,
  size: 28
};
```

## 🔧 Geliştirme

### Proje Yapısı
```
stickman-canvas-demo/
├── index.html          # Ana uygulama dosyası
├── README.md           # Bu dosya
└── assets/             # (Opsiyonel) Arka plan resimleri
    └── bg.jpg
```

README dosyasını güncelledim! Bu dosya şunları içeriyor:

## 📋 **README İçeriği:**

### 🎯 **Başlık ve Rozetler**
- Proje adı ve açıklaması
- Teknoloji rozetleri (JavaScript, Canvas, LocalStorage)

### ✨ **Özellikler Bölümü**
- Temel oyun mekanikleri
- Karakter özelleştirme
- Sahne yönetimi
- Kontrol sistemi

### 🚀 **Hızlı Başlangıç**
- Kurulum adımları
- Kullanım talimatları
- Kontrol tuşları

### 🏗️ **Teknik Mimari**
- Ana bileşenler (Game, Stickman, Input)
- Oyun döngüsü açıklaması
- Veri yapısı örnekleri

### 🎯 **Özelleştirme**
- Yeni arka plan ekleme
- Ekipman sistemi örnekleri

### 🔧 **Geliştirme**
- Proje yapısı
- Kullanılan teknolojiler
- Performans optimizasyonları

### 🎯 **Kullanım Senaryoları**
- Eğitim, prototipleme, eğlence

### 🤝 **Katkıda Bulunma**
- GitHub workflow adımları

Bu README dosyası GitHub'da profesyonel görünecek ve projenin tüm teknik detaylarını açıklayacak şekilde tasarlandı. Emoji'ler, kod blokları ve düzenli başlıklar ile görsel olarak çekici bir format kullanıldı.

### Teknolojiler
- **HTML5 Canvas** - 2D grafik render
- **ES6+ JavaScript** - Modern JavaScript özellikleri
- **LocalStorage API** - Veri kalıcılığı
- **CSS Grid/Flexbox** - Responsive UI
- **RequestAnimationFrame** - Performanslı animasyon

### Performans Optimizasyonları
- ✅ Delta-time tabanlı hareket
- ✅ DPR (Device Pixel Ratio) desteği
- ✅ Event delegation
- ✅ Otomatik kaydetme throttling
- ✅ Canvas clearing optimizasyonu

## 🎯 Kullanım Senaryoları

### Eğitim
- Programlama kavramları öğretimi
- Canvas API örnekleri
- JavaScript oyun geliştirme

### Prototipleme
- Hızlı karakter tasarımı
- UI/UX testleri
- Animasyon prototipleri

### Eğlence
- Basit oyun deneyimi
- Karakter özelleştirme
- Yaratıcı sahne tasarımı

## 🤝 Katkıda Bulunma

1. Fork yapın
2. Feature branch oluşturun (`git checkout -b feature/amazing-feature`)
3. Değişikliklerinizi commit edin (`git commit -m 'Add amazing feature'`)
4. Branch'inizi push edin (`git push origin feature/amazing-feature`)
5. Pull Request oluşturun

## 📝 Lisans

Bu proje MIT lisansı altında lisanslanmıştır. Detaylar için `LICENSE` dosyasına bakın.

## 🙏 Teşekkürler

- HTML5 Canvas API
- Modern JavaScript ES6+ özellikleri
- CSS Grid ve Flexbox
- LocalStorage API

---

⭐ Bu projeyi beğendiyseniz yıldız vermeyi unutmayın!
