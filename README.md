# Rancang-Bangun-Keamanan-Jaringan-Dynamic-Routing-MikroTik-

---

## Daftar Isi

1. [Topologi](#-topologi)
2. [Alat dan Bahan](#-alat-dan-bahan)
3. [Persiapan](#-persiapan)
4. [Ketentuan Soal](#-ketentuan-soal)
5. [Konfigurasi Router A](#-konfigurasi-router-a)
6. [Konfigurasi Router B](#-konfigurasi-router-b)
7. [Pengujian](#-pengujian)
8. [Catatan Penting](#-catatan-penting)

---

## Topologi

<img width="402" height="350" alt="Topologi jaringan" src="https://github.com/user-attachments/assets/3d453efa-6fb9-4b5a-9ddd-fecb7257b7aa" />

---

## Alat dan Bahan

| No | Alat / Bahan | Spesifikasi | Jumlah |
|----|--------------|-------------|--------|
| 1 | Koneksi Internet | Minimal 1 Mbps | - |
| 2 | Router | Minimal 4 port, mendukung routing statis & dinamis | 1 |
| 3 | Wi-Fi Routerboard | Minimal 4 port | 1 |
| 4 | Switch unmanaged | Minimal 4 port | 1 |
| 5 | PC Client / Laptop | - | 3 |
| 6 | Smartphone | Android / iOS | 1 |
| 7 | Kabel UTP | Cat 5 | 12 meter |
| 8 | Konektor RJ-45 | Cat 5 | 10 buah |
| 9 | Crimping Tool & Cable Tester | Untuk RJ-45 | 1 set |

---

## Persiapan

1. Siapkan seluruh alat dan bahan sesuai daftar di atas.
2. Siapkan berkas perencanaan instalasi dan konfigurasi jaringan.
3. Pasang konektor pada kabel jaringan, lalu uji kabel dengan cable tester.
4. Pasang kabel ke switch, router, proxy server, dan client.
5. Lakukan pengalamatan IP pada seluruh perangkat jaringan.

---

## Ketentuan Soal

### Jaringan Lokal Kabel (LAN)
| Parameter | Nilai |
|-----------|-------|
| IP LAN client | `192.(200+absen).10.2/24` |

### Jaringan Internet (WAN)
| Parameter | Nilai |
|-----------|-------|
| IP Internet | Sesuai network dari ISP |
| IP Router A | Sesuai network dari ISP |
| IP Router B | Sesuai network dari ISP |
| Gateway | Sesuai network dari ISP |

### Jaringan Lokal Nirkabel (WLAN)
| Parameter | Nilai |
|-----------|-------|
| IP | `192.(100+absen).20.2/24` |
| Lease Time | 8 jam |
| DNS | Primary: `8.8.8.8` |

### Konfigurasi Router
| Jalur | Metode |
|-------|--------|
| Router A ↔ Router B | Dynamic Routing (OSPF) |
| Router B → Internet | Static Routing (default route) |

### Sistem Keamanan Jaringan (Web Proxy)
| Parameter | Nilai di Soal |
|-----------|---------------|
| Port proxy | `3128` |
| Cache Manager | `NoAbsen+NamaPeserta@sekolah.sch.id` |
| Visible host | `www.sekolah.sch.id` |
| Transparent | Yes |
| Situs yang diblokir | `https://www.linux.org` |

---

## Konfigurasi Router A

**Urutan konfigurasi:** Hostname → IP Address → DHCP Client → NAT → Web Proxy → Firewall → Routing OSPF

### 1. Hostname
Atur identitas router dengan format **`Router A – nama peserta`**.
**Menu:** `System → Identity`

<img width="282" height="141" alt="Identity Router A" src="https://github.com/user-attachments/assets/0e2e1a4e-7e8d-4c7e-8a08-176674abf64d" />

### 2. DHCP Client (ke modem ISP)
Aktifkan DHCP Client pada `ether1` yang terhubung ke modem ISP.
**Menu:** `IP → DHCP Client → Add`

| Parameter | Nilai |
|-----------|-------|
| Interface | ether1 |
| Use Peer DNS | ✅ Dicentang |
| Use Peer NTP | ✅ Dicentang |
| Add Default Route | yes |

<img width="448" height="197" alt="DHCP Client Router A" src="https://github.com/user-attachments/assets/c106336b-411a-4a8f-a328-dae10ecfadaa" />

### 3. IP Address (sisi client)
Beri alamat IP pada `ether2` yang mengarah ke client, memakai network `192.(200+absen).10.1/24`.
**Menu:** `IP → Address → Add`

| Parameter | Nilai |
|-----------|-------|
| Address | 192.212.10.1/24 |
| Network | 192.212.10.0 |
| Interface | ether2 |

<img width="352" height="133" alt="IP Address Router A" src="https://github.com/user-attachments/assets/b9f0434b-1ba2-47fb-973f-2aac9ea96be0" />

### 4. NAT (Masquerade)
Agar client dapat mengakses internet, buat rule NAT dengan action masquerade.
**Menu:** `IP → Firewall → NAT → Add`

| Parameter | Nilai |
|-----------|-------|
| Chain | srcnat |
| Out. Interface | ether1 |
| Action | masquerade |

<img width="697" height="270" alt="NAT Router A" src="https://github.com/user-attachments/assets/d57366b4-832b-4f7c-8b8f-c388782c5637" />

### 5. Routing OSPF
Agar client di balik Router A dan Router B bisa saling berkomunikasi, aktifkan OSPF.
**Menu:** `Routing → OSPF → Networks → Add`

- Advertise semua network yang terhubung
- Gunakan area **backbone**

<img width="450" height="192" alt="OSPF Router A" src="https://github.com/user-attachments/assets/c0947e6e-fe4c-46f8-974c-9c11ba94e273" />

### 6. Web Proxy
Setelah OSPF selesai, lanjutkan ke Web Proxy dengan ketentuan berikut.

| Parameter | Nilai yang Dipakai |
|-----------|--------------------|
| Port proxy | 8080 |
| Cache Manager | NoAbsen+NamaPeserta@sekolah.sch.id |
| Visible host | www.sekolah.sch.id |
| Transparent | Yes |
| Situs diblokir | `https://www.linux.org` & `http://httpforever.com/` |

**Menu:** `IP → Web Proxy`

- Centang **Enabled**
- **Port:** `8080`
- Centang **Anonymous**
- **Cache Administrator:** `12Sabiq@sekolah.sch.id`

<img width="497" height="298" alt="Web Proxy Settings" src="https://github.com/user-attachments/assets/c8176b46-e623-4970-a443-f97cfee66ca4" />

> Jangan langsung klik **OK**. Klik tombol **Access** terlebih dahulu untuk menentukan situs yang akan diblokir, lalu klik **Add**.

| Parameter | Nilai |
|-----------|-------|
| Dst. Host | `*linux.org*` dan `*httpforever.com*` (satu rule untuk masing-masing) |
| Action | deny |

<img width="565" height="418" alt="Web Proxy Access Rule" src="https://github.com/user-attachments/assets/6d73e7b5-445f-4b21-8a68-a6cfb0e8dfc0" />

### 7. Firewall NAT (Redirect HTTP ke Proxy)
Supaya traffic HTTP client otomatis dibelokkan ke web proxy (mode transparent), tambahkan rule berikut.
**Menu:** `IP → Firewall → NAT → Add`

| Parameter | Nilai |
|-----------|-------|
| Chain | dstnat |
| Protocol | 6 (tcp) |
| Dst. Port | 80 |
| In. Interface | ether2 |
| Action | redirect |
| To Ports | 8080 |

<img width="737" height="270" alt="NAT Redirect Proxy" src="https://github.com/user-attachments/assets/0c6d778f-ca5b-4c50-a837-861a38840ef2" />

Konfigurasi **Router A** selesai. Lanjut ke Router B, langkahnya mirip dengan beberapa perbedaan.

---

## Konfigurasi Router B

**Urutan konfigurasi:** Hostname → Wireless → DHCP Client → IP Address → DHCP Server → Default Route → NAT → Routing OSPF

### 1. Hostname
Gunakan format **`Router B – nama peserta`**.
**Menu:** `System → Identity`

<img width="282" height="142" alt="Identity Router B" src="https://github.com/user-attachments/assets/8af7235f-0a9f-4102-b685-40ad160a2b4c" />

### 2. Wireless (Access Point)
Jadikan router sebagai Access Point agar client bisa terhubung lewat Wi-Fi.
**Menu:** `Wireless → Interface wlan1 → Tab Wireless`

| Parameter | Nilai |
|-----------|-------|
| Mode | ap bridge |
| Band | 2GHz-B/G/N |
| SSID | UKK-12-Sabiq |

<img width="598" height="226" alt="Wireless wlan1" src="https://github.com/user-attachments/assets/f2bb0c1e-4a58-4a73-9aec-3394efe329cd" />

### 3. Security Profile Wireless
Agar hanya client yang memiliki password yang bisa terhubung, buat security profile.
**Menu:** `Wireless → Security Profiles → Add`

| Parameter | Nilai |
|-----------|-------|
| Name | Key123 |
| Mode | dynamic keys |
| Authentication Types | WPA PSK & WPA2 PSK |
| WPA Pre-Shared Key | P@ssw0rd |
| WPA2 Pre-Shared Key | P@ssw0rd |

<img width="487" height="292" alt="Security Profile" src="https://github.com/user-attachments/assets/e44f6731-e5d8-48d2-bd99-38ce4ea06529" />

Terapkan profile tersebut pada interface wlan1.
**Menu:** `Wireless → wlan1` → **Security Profile:** `Key123`

<img width="587" height="227" alt="Terapkan Security Profile" src="https://github.com/user-attachments/assets/88c9c2a4-02d8-4df0-8e6e-d23dadd55807" />

### 4. DHCP Client (ke modem ISP)
Pada Router B, DHCP Client di `ether1` **tidak boleh** menambahkan default route.
**Menu:** `IP → DHCP Client → Add`

| Parameter | Nilai |
|-----------|-------|
| Interface | ether1 |
| Use Peer DNS | ✅ Dicentang |
| Use Peer NTP | ✅ Dicentang |
| Add Default Route | **no** |

<img width="396" height="177" alt="DHCP Client Router B" src="https://github.com/user-attachments/assets/69e90d0f-1e8a-4e6d-84ea-137ffb2e4b5c" />

### 5. IP Address (sisi WLAN)
Beri alamat IP pada `wlan1` dengan network `192.(100+absen).20.1/24`.
**Menu:** `IP → Address → Add`

| Parameter | Nilai |
|-----------|-------|
| Address | 192.112.20.1/24 |
| Network | 192.112.20.0 |
| Interface | wlan1 |

<img width="357" height="132" alt="IP Address wlan1" src="https://github.com/user-attachments/assets/0aa7012f-43af-4bec-884c-bdcd739e58ce" />

### 6. DHCP Server
Supaya client wireless tidak perlu mengatur IP manual, buat DHCP Server pada `wlan1` menggunakan wizard.
**Menu:** `IP → DHCP Server → DHCP Setup`

| Parameter | Nilai |
|-----------|-------|
| DHCP Server Interface | wlan1 |
| DHCP Address Space | 192.112.20.0/24 |
| Gateway for DHCP Network | 192.112.20.1 |
| Addresses to Give Out | 192.112.20.2 – 192.112.20.254 |
| DNS Servers | 8.8.8.8 & 192.168.100.1 |
| Lease Time | 08:00:00 (8 jam) |

<img width="737" height="262" alt="DHCP Setup" src="https://github.com/user-attachments/assets/f28a2321-c849-4a45-8b86-e58ff8e6ab4a" />

### 7. Default Route (Static)
Karena DHCP Client tidak menambahkan default route, buat secara manual.
**Menu:** `IP → Routes → Add`

| Parameter | Nilai |
|-----------|-------|
| Dst. Address | 0.0.0.0/0 |
| Gateway | 172.18.20.1 |

<img width="650" height="106" alt="Default Route" src="https://github.com/user-attachments/assets/20abe72d-ebbd-439c-9316-642ca770b44c" />

### 8. NAT (Masquerade)
Setelah default route dibuat, router sudah bisa online, tetapi client belum. Tambahkan NAT agar client ikut terhubung ke internet.
**Menu:** `IP → Firewall → NAT → Add`

| Parameter | Nilai |
|-----------|-------|
| Chain | srcnat |
| Out. Interface | ether1 |
| Action | masquerade |

<img width="713" height="272" alt="NAT Router B" src="https://github.com/user-attachments/assets/ee2bf4eb-2b71-432f-ab9d-c6bcbdcf167c" />

### 9. Routing OSPF
**Menu:** `Routing → OSPF → Networks → Add`

- Advertise semua network yang terhubung
- Gunakan area **backbone**

<img width="427" height="182" alt="OSPF Router B" src="https://github.com/user-attachments/assets/d6ff7622-6076-464f-9e9f-7142a49f5703" />

✅ Konfigurasi **Router B** selesai. Selanjutnya lakukan pengujian dari sisi router maupun client.

---

## Pengujian

### Pengujian Router A

**1. Tes konektivitas (ping dari CMD client)**
- Ping ke gateway
- Ping antar client
- Ping ke internet (`google.com`)

<img width="615" height="322" alt="Ping Router A" src="https://github.com/user-attachments/assets/39c0fb0f-a170-41fa-aea0-8b9ac2ab5d06" />

**2. Tes pemblokiran situs**
Buka salah satu situs yang diblokir (`linux.org` atau `httpforever.com`) lewat browser. Jika berhasil, akan muncul pesan *Forbidden / Access Denied*.

<img width="537" height="252" alt="Tes blokir situs" src="https://github.com/user-attachments/assets/ec6680e2-a7b8-4703-b6c8-a323de7f0dbc" />

**3. Cek OSPF Neighbor**
Pastikan status neighbor antara Router A dan Router B sudah *up*.
**Menu:** `Routing → OSPF → Neighbors`

<img width="585" height="161" alt="OSPF Neighbor Router A" src="https://github.com/user-attachments/assets/cd339a1c-99d3-4850-b1ec-ee148a030894" />

**4. Cek routing table**
Router A sudah memiliki route menuju network client milik Router B, ditandai flag **`DAo`** (*Dynamic, Active, OSPF*).
**Menu:** `IP → Routes`

<img width="646" height="172" alt="Routing table Router A" src="https://github.com/user-attachments/assets/98c0d44c-c4b8-4868-87f9-75828869428c" />

### Pengujian Router B

**1. Cek DHCP Server**
Pastikan perangkat yang terhubung lewat Wi-Fi mendapatkan IP otomatis dari DHCP.

<img width="418" height="365" alt="Hasil DHCP client" src="https://github.com/user-attachments/assets/63aa386e-cf5b-4747-8cce-4cb475eb43ba" />

**2. Tes konektivitas (ping dari CMD client)**
- Ping ke gateway
- Ping antar client
- Ping ke internet (`google.com`)

<img width="612" height="331" alt="Ping Router B" src="https://github.com/user-attachments/assets/0501d4a4-ccd8-4ed7-a362-c0b643f608ae" />

**3. Cek OSPF Neighbor**
Pastikan informasi Router A muncul pada tab Neighbors.
**Menu:** `Routing → OSPF → Neighbors`

<img width="526" height="147" alt="OSPF Neighbor Router B" src="https://github.com/user-attachments/assets/e73d0bb1-00f4-484a-9413-9a25eabfe71f" />

**4. Cek routing table**
Router B sudah memiliki route menuju network client milik Router A.
**Menu:** `IP → Routes`

<img width="622" height="185" alt="Routing table Router B" src="https://github.com/user-attachments/assets/604922dc-7784-4cc9-82c6-a4cd479324f0" />

Dengan adanya route OSPF di kedua router, dapat disimpulkan bahwa **dynamic routing berjalan dengan baik**.

---

## Catatan Penting

Seluruh konfigurasi telah selesai dan hasil pengujian sesuai harapan. Namun, ada dua penyesuaian dari ketentuan soal pada bagian Web Proxy:

| Ketentuan Soal | Yang Diterapkan | Alasan |
|----------------|-----------------|--------|
| Port proxy `3128` | Port `8080` | Saat memakai port 3128, web proxy tidak berjalan |
| Blokir `linux.org` | Blokir `linux.org` **dan** `httpforever.com` | Ditambahkan situs `httpforever.com` sebagai pembanding/uji coba pemblokiran |

---

<p align="center"><b>SELAMAT & SUKSES 🎉</b></p>
