# Tunnel Link — GitHub reader app

Yo euta sano web page (GitHub Pages ma rakhne) ho, jasle **Tunnel Setup Manager** app le
pathayeko Cloudflare tunnel URL lai **passphrase** halera kholcha.

## Kasari kaam garcha (surakshit)

1. Tapaiko PC ko **Tunnel Setup Manager** le tunnel chalaune bela, URL lai **passphrase le
   encrypt** (AES-256-GCM) garera tapaiko GitHub repo ko euta file (`link.json`) ma lekhcha.
2. Tyo file ma **encrypt bhayeko text matra** huncha — repo public bhaye pani, jassँga
   passphrase chhaina usle asli URL dekhdaina.
3. Yo page (`index.html`) le tyo file tanera, tapaile haaleko passphrase le **browser mai**
   khulcha. Passphrase kahilyai internet ma jadaina.

**Dui taha ko surakshya:**
- **Inject rokne:** repo ma lekhna GitHub token chahincha, jun tapaiko PC ko `tunnel_link.ini`
  (wa `TUNNEL_LINK_TOKEN` env var) mai matra huncha. Baira kasaisँga hundaina → inject garna sakdaina.
- **Chori rokne:** URL encrypted huncha. Passphrase bina koi le kholna sakdaina.

---

## Setup — ek choti matra

### 1) GitHub repo banau (link rakhne thau)
- Naya repo banau, jastai `tunnel-link`. **Public** rakhe hunchha (link encrypted chha).
- Yo repo ma `link.json` app le aafai banauchha — tapaile kehi garnu pardaina.

### 2) Fine-grained token banau (PC ko lagi)
- GitHub → Settings → Developer settings → **Fine-grained tokens** → Generate new token.
- **Repository access:** _Only select repositories_ → tei `tunnel-link` matra.
- **Permissions:** Repository permissions → **Contents: Read and write**. Aru kehi hoina.
- Token copy gara (ek choti matra dekhincha).

### 3) PC ko app config (`tunnel_link.ini`)
Tunnel Setup Manager pahilo choti chalda tyo app ko folder ma `tunnel_link.ini` aafai banchha.
Tyeslai yesto bhar:
```
[tunnel-link]
enabled = true
repo = TAPAIKO_USERNAME/tunnel-link
path = link.json
branch = main
token =                 ; yeslai khali rakhera, TUNNEL_LINK_TOKEN env var ma rakhnu surakshit
passphrase = euta-gopya-passphrase
```
> **Surakshit tarika:** `token` ra `passphrase` lai file ma narakhera environment variable
> `TUNNEL_LINK_TOKEN` ra `TUNNEL_LINK_PASSPHRASE` ma rakhnu. Env var file bhanda ramro.

### 4) Yo reader page set gara ani GitHub Pages ma halide
- `index.html` kholera, mathi `CONFIG` ma `owner`, `repo`, `path` tapaiko anusar milau
  (`branch` sadhai `main`).
- `index.html` lai GitHub repo ma push gara (yei `tunnel-link` repo ma, wa arko repo ma).
- Repo → Settings → **Pages** → Source: `main` branch → Save.
- Kehi ber pachhi tapaiko page `https://TAPAIKO_USERNAME.github.io/tunnel-link/` ma khulcha.

### 5) Chalau
- PC ma Tunnel Setup Manager chalau → tunnel start huncha → URL encrypt bhayera GitHub ma jancha.
- Reader page kholera **passphrase** haal → **Unlock link** → asli URL dekhincha, Open/Copy.

---

## Mahatwapurna
- `passphrase` dubai thau (app ra reader) ma **ekdamai eutai** hunuparcha.
- Token **kahilyai** `index.html` ma narakhnu — yo page public huncha, token leak huncha.
  Token matra PC ko config/env ma.
- Token lai jahile pani GitHub bata revoke garna sakincha.
- Yo Cloudflare ko official product haina.
