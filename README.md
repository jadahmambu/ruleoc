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
| `geoip.dat` | `private`, `id`, `facebook`, dll. | Kompilasi IP CIDR biner resmi v2fly | [Download `geoip.dat`](https://github.com/jadahmambu/ruleoc/releases/download/release/geoip.dat) |

### 2. Berkas Teks Plain (Domain List)

| Nama Berkas | Sumber Utama | Link Raw |
| :--- | :--- | :--- |
| `adblock_rules.txt` | StevenBlack Hosts, AdGuard SDNS | [adblock_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/adblock_rules.txt) |
| `malware_rules.txt` | URLhaus, Phishing Filter, OISD | [malware_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/malware_rules.txt) |
| `nsfw_rules.txt` | StevenBlack Porn, Sinfonietta | [nsfw_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/nsfw_rules.txt) |
| `meta_rules.txt` | v2fly Facebook, Instagram, WhatsApp | [meta_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/meta_rules.txt) |
| `tiktok_rules.txt` | v2fly TikTok, ByteDance CDN | [tiktok_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/tiktok_rules.txt) |

---

## 📖 Cara Penggunaan & Integrasi

### A. Penggunaan di OpenClash (Metode `Rule-Provider` Teks)

Tambahkan ke dalam berkas konfigurasi `config.yaml` OpenClash Anda:

```yaml
rule-providers:
  adblock-rules:
    type: http
    behavior: domain
    url: "[https://raw.githubusercontent.com/jadahmambu/ruleoc/main/adblock_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/adblock_rules.txt)"
    path: ./ruleset/adblock_rules.yaml
    interval: 86400

  malware-rules:
    type: http
    behavior: domain
    url: "[https://raw.githubusercontent.com/jadahmambu/ruleoc/main/malware_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/malware_rules.txt)"
    path: ./ruleset/malware_rules.yaml
    interval: 86400

  nsfw-rules:
    type: http
    behavior: domain
    url: "[https://raw.githubusercontent.com/jadahmambu/ruleoc/main/nsfw_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/nsfw_rules.txt)"
    path: ./ruleset/nsfw_rules.yaml
    interval: 86400

  meta-rules:
    type: http
    behavior: domain
    url: "[https://raw.githubusercontent.com/jadahmambu/ruleoc/main/meta_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/meta_rules.txt)"
    path: ./ruleset/meta_rules.yaml
    interval: 86400

  tiktok-rules:
    type: http
    behavior: domain
    url: "[https://raw.githubusercontent.com/jadahmambu/ruleoc/main/tiktok_rules.txt](https://raw.githubusercontent.com/jadahmambu/ruleoc/main/tiktok_rules.txt)"
    path: ./ruleset/tiktok_rules.yaml
    interval: 86400

rules:
  # Pemblokiran Keamanan & Iklan
  - RULE-SET,adblock-rules,REJECT
  - RULE-SET,malware-rules,REJECT
  - RULE-SET,nsfw-rules,REJECT

  # Routing Aplikasi & Sosmed
  - RULE-SET,meta-rules,DIRECT
  - RULE-SET,tiktok-rules,DIRECT
