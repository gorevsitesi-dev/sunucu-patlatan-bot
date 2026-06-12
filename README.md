# Discord Server Nuke Bot

Discord sunucularını sıfırlamak (nuke) için yazılmış bir bottur. **Kötü amaçlı kullanım sorumluluğu size aittir.**

## 📁 Dosya Yapısı

```
├── index.js               # Ana bot dosyası - giriş noktası
├── botConfig.json          # Bot ayarları (token, prefix, vb.)
├── botConfig.json.example  # Ayarlar için örnek dosya
├── package.json            # Bağımlılıklar
├── .gitignore              # Git'ten gizlenecek dosyalar
└── commands/
    ├── allban.js           # .ban - Tüm üyeleri banlar
    ├── kick.js             # .kick - Tüm üyeleri kickler
    ├── mute.js             # .timeout - Tüm üyeleri susturur
    └── nuke.js             # .nuke - Sunucuyu tamamen sıfırlar
```

## 🔧 Kurulum

```bash
# 1. Bağımlılıkları yükle
npm install

# 2. Bot ayarlarını yapılandır
# botConfig.json dosyasını açıp kendi değerlerini gir
```

## ⚙️ botConfig.json Ayarları

| Anahtar | Açıklama | Örnek |
|---------|----------|-------|
| `token` | Discord Bot Tokeni | `BURAYA_BOT_TOKENİ` |
| `prefix` | Komut öneki | `.` |
| `color` | Embed rengi (hex) | `FFFFFF` |
| `reason` | Ban/kick/timeout sebebi + spam mesajı | `BURAYA_SUNUCU_DAVET_LINKI` |

## 🤖 Komutlar

| Komut | Prefix | Dosya | Ne yapar? |
|-------|--------|-------|-----------|
| `.ban` | `.` | `commands/allban.js` | Sunucudaki **tüm üyeleri** banlar. Sebep: `botConfig.reason` |
| `.kick` | `.` | `commands/kick.js` | Sunucudaki **tüm üyeleri** kickler. Sebep: `botConfig.reason` |
| `.timeout` | `.` | `commands/mute.js` | Sunucudaki **tüm üyeleri** 4 saniyeliğine susturur. Sebep: `botConfig.reason` |
| `.nuke` | `.` | `commands/nuke.js` | Sunucuyu komple sıfırlar (aşağıya bak) |

## ☢️ .nuke Komutu Detaylı İşlem Sırası

Sırayla şunları yapar:

1. **Sunucu adını değiştirir** → `"Nuked Server"`
2. **Sunucu açıklamasını değiştirir** → `"Bu sunucu nuke botu tarafından sıfırlandı."`
3. **Sunucu ikonunu değiştirir** → `BURAYA_RESIM_URL` (şu an örnek: `https://example.com/nuke-icon.jpg`)
4. **Tüm kanalları siler**
5. **@everyone hariç tüm rolleri siler**
6. **240 tane "nuked-role" adında rol oluşturur** ve her role tüm üyeleri ekler
7. **50 tane "nuked-channel" adında kanal oluşturur** (herkese mesaj izni kapalı)
8. **Her kanala 1000'er mesaj atar** → `@everyone @here <davet_linkin>`

## 🔒 Açık Kaynak Düzenlemeleri

Projeyi paylaşıma hazır hale getirmek için aşağıdaki değişiklikler yapıldı:

| Dosya | Değişiklik | Eski (sansürsüz) | Yeni (paylaşıma hazır) |
|-------|-----------|-------------------|------------------------|
| `botConfig.json` | Token gizlendi | `MTUwOTY3...` | `BURAYA_BOT_TOKENİ` |
| `botConfig.json` | Davet linki gizlendi | `https://discord.gg/CV9abpmURC` | `BURAYA_SUNUCU_DAVET_LINKI` |
| `index.js:23` | Bot adı değiştirildi | `InfinityClient` | `Bot is ready!` |
| `commands/nuke.js:2` | Sunucu adı | `"Sunucu Sikildi"` | `"Nuked Server"` |
| `commands/nuke.js:4` | Sunucu açıklaması | `"AbuseBOT tarafından sunucu sikildi..."` | `"Bu sunucu nuke botu tarafından sıfırlandı."` |
| `commands/nuke.js:8` | Resim URL'si | Gerçek Pinterest URL | `https://example.com/nuke-icon.jpg` |
| `commands/nuke.js:19` | Rol adı | `"babanız-sikti-attı"` | `"nuked-role"` |
| `commands/nuke.js:24` | Kanal adı | `"babanız-sikti-attı"` | `"nuked-channel"` |
| `commands/nuke.js:34` | Spam mesajı linki | `https://discord.gg/CV9abpmURC` | `BURAYA_SUNUCU_DAVET_LINKI` |

## 📝 Kullanıcının Doldurması Gereken Yerler

| Nerede? | Ne? |
|---------|-----|
| `botConfig.json` → `token` | Discord Developer Portal'dan aldığın bot tokeni |
| `botConfig.json` → `reason` | Ban/kick sebebi veya kendi sunucu davet linkin |
| `commands/nuke.js:8` | Sunucuya atanacak icon resminin URL'si |
| `commands/nuke.js:34` | Spam mesajlarında gönderilecek davet linki |
