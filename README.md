# 🛡️ RuleOC - Custom Geosite, GeoIP & Filter Rulesets

Sistem otomatis berbasis GitHub Actions untuk mengumpulkan, membersihkan, dan mengompilasi daftar domain serta IP ke dalam format biner (`geosite.dat` & `geoip.dat`) dan teks plain (`.txt`). 

Dioptimalkan khusus untuk **OpenClash**, **Clash Premium**, **v2rayN**, **Xray**, dan **Sing-Box**.

---

## 🚀 Fitur Utama

- 🔄 **Pembaruan Otomatis**: Diperbarui secara berkala setiap hari pukul 00:00 UTC.
- 🧹 **Sanitasi Ketat**: Pembersihan domain dari karakter ilegal (`*`, `_`, prefix bermasalah) agar kompatibel dengan v2fly compiler.
- ⚡ **Multi-Format**: Mendukung format biner Protobuf (`.dat`) dan format daftar domain teks (`.txt`).
- 🛑 **Proteksi Lengkap**: Kombinasi AdBlock, Phishing/Malware, NSFW, Meta (FB/IG/WA), dan TikTok.

---

## 📁 Daftar Berkas & Link Unduhan

### 1. Berkas Biner (Releases / Raw)

| Nama Berkas | Tag Terdaftar | Deskripsi | Link Unduhan Direct |
| :--- | :--- | :--- | :--- |
| `geosite.dat` | `adblock`, `malware`, `nsfw`, `meta`, `tiktok` | Kompilasi domain biner lengkap | [Download `geosite.dat`](https://github.com/jadahmambu/ruleoc/releases/download/release/geosite.dat) |
| `geoip.dat` | `private`, `id`, `facebook`, `google`, `telegram`, dll. | Kompilasi IP CIDR biner resmi v2fly | [Download `geoip.dat`](https://github.com/jadahmambu/ruleoc/releases/download/release/geoip.dat) |

### 2. Berkas Teks Plain (Domain List)

| Nama Berkas | Sumber Utama | Link Raw |
| :--- | :--- | :--- |
| `adblock_rules.txt` | StevenBlack Hosts, AdGuard SDNS | [adblock_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/adblock_rules.txt) |
| `malware_rules.txt` | URLhaus, Phishing Filter, OISD | [malware_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/malware_rules.txt) |
| `nsfw_rules.txt` | StevenBlack Porn, Sinfonietta | [nsfw_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/nsfw_rules.txt) |
| `meta_rules.txt` | v2fly Facebook, Instagram, WhatsApp | [meta_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/meta_rules.txt) |
| `tiktok_rules.txt` | v2fly TikTok, ByteDance CDN | [tiktok_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/tiktok_rules.txt) |

---

## 📊 Rincian Isi Tag Geosite & GeoIP

Gunakan referensi ini untuk memilih tag mana yang ingin Anda masukkan ke dalam aturan routing:

### A. Tag Geosite (`geosite.dat`)

| Tag Geosite | Cakupan Domain & Sumber | Rekomendasi Aksi / Routing |
| :--- | :--- | :--- |
| **`adblock`** | Domain iklan, tracker, analytics, & telemetry | **REJECT** (Blokir iklan) |
| **`malware`** | Domain phishing, malware, virus, scam, & judi online | **REJECT** (Keamanan) |
| **`nsfw`** | Situs dewasa / konten pornografi | **REJECT** (Filter negatif) |
| **`meta`** | Facebook, Instagram, WhatsApp, & Threads | **DIRECT** (Bypass proxy) |
| **`tiktok`** | TikTok, TikTok CDN, ByteDance, & Live Streaming | **DIRECT** atau **Proxy Khusus** |

### B. Tag GeoIP (`geoip.dat`)

| Tag GeoIP | Cakupan IP CIDR | Rekomendasi Aksi / Routing |
| :--- | :--- | :--- |
| **`private`** | Blok IP LAN/Lokal Network (`192.168.x.x`, `10.x.x.x`, `127.0.0.1`) | **DIRECT** (Wajib agar LAN tak terputus) |
| **`id`** | Seluruh IP publik lokal Indonesia | **DIRECT** (Bypass trafik domestik) |
| **`facebook`** / **`meta`** | Blok IP server Meta / Facebook / WhatsApp | **DIRECT** |
| **`google`** | Blok IP infrastruktur Google & YouTube | **DIRECT** atau **Proxy** |
| **`cloudflare`** | Blok IP CDN Cloudflare | **DIRECT** atau **Proxy** |
| **`telegram`** | Blok IP server pusat Telegram | **DIRECT** atau **Proxy** |
| **`netflix`** | Blok IP server CDN streaming Netflix | **DIRECT** atau **Proxy Khusus** |

---

## 📖 Cara Penggunaan & Format Penulisan Rule

### 1. OpenClash / Clash Premium (`config.yaml`)

#### A. Menggunakan Biner (`geosite.dat` & `geoip.dat`)
Unduh `geosite.dat` dan `geoip.dat`, letakkan di folder `/etc/openclash/` atau `/etc/openclash/RuleSet/`.

```yaml
rules:
  # Pemblokiran Keamanan (GEOSITE)
  - GEOSITE,adblock,REJECT
  - GEOSITE,malware,REJECT
  - GEOSITE,nsfw,REJECT

  # Routing Network & Sosmed (GEOIP & GEOSITE)
  - GEOIP,private,DIRECT
  - GEOIP,id,DIRECT
  - GEOSITE,meta,DIRECT
  - GEOIP,facebook,DIRECT
  - GEOSITE,tiktok,DIRECT

  # Match Sisanya ke Proxy
  - MATCH,Proxy-Group
```

#### B. Menggunakan Teks Plain (`rule-providers`)
```yaml
rule-providers:
  adblock-rules:
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/jadahmambu/ruleoc/main/adblock_rules.txt"
    path: ./ruleset/adblock_rules.yaml
    interval: 86400

  meta-rules:
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/jadahmambu/ruleoc/main/meta_rules.txt"
    path: ./ruleset/meta_rules.yaml
    interval: 86400

  tiktok-rules:
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/jadahmambu/ruleoc/main/tiktok_rules.txt"
    path: ./ruleset/tiktok_rules.yaml
    interval: 86400

rules:
  - RULE-SET,adblock-rules,REJECT
  - RULE-SET,meta-rules,DIRECT
  - RULE-SET,tiktok-rules,DIRECT
```

---

### 2. V2Ray / Xray (`config.json`)

```json
"routing": {
  "rules": [
    {
      "type": "field",
      "outboundTag": "blocked",
      "geosite": [
        "ext:geosite.dat:adblock",
        "ext:geosite.dat:malware",
        "ext:geosite.dat:nsfw"
      ]
    },
    {
      "type": "field",
      "outboundTag": "direct",
      "geosite": [
        "ext:geosite.dat:meta",
        "ext:geosite.dat:tiktok"
      ],
      "geoip": [
        "private",
        "id",
        "facebook"
      ]
    }
  ]
}
```

---

### 3. Sing-Box (`config.json`)

```json
"route": {
  "rules": [
    {
      "geosite": ["adblock", "malware", "nsfw"],
      "outbound": "block"
    },
    {
      "geosite": ["meta", "tiktok"],
      "geoip": ["private", "id", "facebook"],
      "outbound": "direct"
    }
  ]
}
```

---

## 🛠️ Lisensi & Sumber Data
- **StevenBlack Hosts**: [StevenBlack/hosts](https://github.com/StevenBlack/hosts)
- **AdGuard Filters**: [AdGuardDNS](https://github.com/AdGuardTeam/AdGuardSDNSFilter)
- **URLhaus Threat Intelligence**: [abuse.ch](https://urlhaus.abuse.ch/)
- **v2fly Community Data**: [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community) & [v2fly/geoip](https://github.com/v2fly/geoip)
