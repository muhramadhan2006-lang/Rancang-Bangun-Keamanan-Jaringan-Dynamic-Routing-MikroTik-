# Rancang-Bangun-Keamanan-Jaringan-Dynamic-Routing-MikroTik-

# Topologi
<img width="402" height="350" alt="image" src="https://github.com/user-attachments/assets/3d453efa-6fb9-4b5a-9ddd-fecb7257b7aa" />

# Alat dan Bahan
1. Internet (Koneksi internet minimal 1 Mbps)
2. Router (Minimal 4 port, mendukung routing statis & dinamis)
3. Wifi Routerboard (Minimal 4 port)
4. Switch unmanaged (Minimal 4 port)
5. PC Client / Laptop (3 buah)
6. Smartphone (1 buah)
7. Kabel UTP (Cat 5, panjang 12 meter)
8. Konektor RJ-45 (10 buah)
9. Crimping Tools & Cable Tester

# Setup
1. Siapkan perlatan dan bahan yang diperlukan sesuai dengan daftar peralatan.
2. Siapkan berkas perencanaan instalasi dan konfigurasi jaringan.
3. Lakukan pemasangan konektor pada kabel jaringan dan pengujian koneksi kabel.
4. Lakukan pemasangan kabel pada switch, router, proxy server dan client.
5. Lakukan pengalamatan jaringan pada seluruh perangkat jaringan.

# Daftar Ketentuan
## Jaringan lokal kabel (LAN)
1. IP LAN client : 192.200+absen.10.2/24
   
## Jaringan internet (WAN)
1. IP Internet : Sesuai dengan network yang diberikan ISP
2. IP Router A : Sesuai dengan network yang diberikan ISP
3. IP Router B : Sesuai dengan network yang diberikan ISP
4. Gateway : Sesuai dengan network yang diberikan ISP

## Jaringan lokal nirkabel (WLAN)
1. IP : 192.100+absen.20.2/24
2. Lease Time : 8 jam
3. DNS : Primary: 8.8.8.8

## Konfigurasi router
1. Dynamic Routing (Router A ke B)
2. Static Routing (Router B ke Internet)

## Sistem keamanan jaringan
1. Port proxy : 3128
2. Cache Manager: Noabsen+namapeserta@sekolah.sch.id
3. Visible host : www.sekolah.sch.id
4. Transparent : yes
5. Bloking Site : https://www.linux.org

# Langkah-Langkah Konfigurasi
## Konfigurasi Router A
1. Hostname
2. IP Address
3. DHCP Client
4. NAT
5. Web Proxy
6. Firewall
7. Routing OSPF

Mulai konfigurasi hostname terlebih dahulu dengan format “Router A – nama peserta”. System → Identity.

<img width="282" height="141" alt="image" src="https://github.com/user-attachments/assets/0e2e1a4e-7e8d-4c7e-8a08-176674abf64d" />

Setelah itu konfigurasi DHCP Client untuk interface di router yang mengarah ke modem isp yaitu ether1. IP → DHCP Client → Add.
* Checklist Use Peer DNS
* Checklist Use Peer NTP
* Add Default Route: Yes

<img width="448" height="197" alt="image" src="https://github.com/user-attachments/assets/c106336b-411a-4a8f-a328-dae10ecfadaa" />

Konfigurasi IP Address untuk interface yang mengarah ke client yakni di interface ether2 dengan menggunakan network “192.200+absen.10.1/24”. IP → Address → Add.
* Address: 192.212.10.1/24
* Network: 192.212.10.0
* Interface: ether2
<img width="352" height="133" alt="image" src="https://github.com/user-attachments/assets/b9f0434b-1ba2-47fb-973f-2aac9ea96be0" />

Agar client terhubung ke internet. maka perlu konfigurasi NAT pada router dengan menggunakan action “Masquerade”. IP → Firewall → NAT → Add.
* Chain: srcnat
* Out Interface: ether1
* Action: masquerade
<img width="697" height="270" alt="image" src="https://github.com/user-attachments/assets/d57366b4-832b-4f7c-8b8f-c388782c5637" />

Selanjutnya agar client antar router bisa saling berkomunikasi, kita perlu konfigurasi Routing OSPF pada router. Routing → OSPF → Network → Add.
* Advertise semua network
* Gunakan area bacbone
<img width="450" height="192" alt="image" src="https://github.com/user-attachments/assets/c0947e6e-fe4c-46f8-974c-9c11ba94e273" />

Setelah konfigurasi OSPF selesai maka bisa lanjut untuk konfigurasi Web Proxy dengan
ketentuan sebagai berikut:
* Port proxy : 8080
* Cache Manager: No.absen+namapeserta@sekolah.sch.id
* Visible host : www.sekolah.sch.id
* Transparent : yes
* Bloking Site : https://www.linux.org & http://httpforever.com/

Lanjut masuk ke menunya terlebih dahulu. IP → Web Proxy.
* Checklist Enabled
* Port: 8080
* Checklist Anonymous
* Cache Administrator: 12Sabiq@sekolah.sch.id
<img width="497" height="298" alt="image" src="https://github.com/user-attachments/assets/c8176b46-e623-4970-a443-f97cfee66ca4" />

Jangan langsung klik “OK” tapi klik “Access” terlebih dahulu karena kita akan mendefine website yang akan kita blok. Add.
* Dst. Host: *linux.org* & *httpforever.com*
* Action: deny
<img width="565" height="418" alt="image" src="https://github.com/user-attachments/assets/6d73e7b5-445f-4b21-8a68-a6cfb0e8dfc0" />

Setelah selesai setup Web Proxy, selanjutnya kita perlu menambahkan rule di menu Firewall
NAT, agar traffic http dibelokkan ke web proxy. IP → Firewall → NAT → Add.
* Chain: dstnat
* Protocol: 6 (tcp)
* Dst. Port: 80
* In. Interface: ether2
* Action: redirect
* To Ports: 8080
<img width="737" height="270" alt="image" src="https://github.com/user-attachments/assets/0c6d778f-ca5b-4c50-a837-861a38840ef2" />

Konfigurasi pada “Router A” sudah selesai, lanjut kita konfigurasi di Router B, konfigurasinya mirip-mirip tidak terlalu beda.
## Konfigurasi Router B
* Hostname
* IP Address
* Wireless
* DHCP Server
* DHCP Client
* NAT
* Web Proxy
* Default Route
* Routing OSPF
Mulai konfigurasi hostname terlebih dahulu dengan format “Router B – nama peserta”. System → Identity.
<img width="282" height="142" alt="image" src="https://github.com/user-attachments/assets/8af7235f-0a9f-4102-b685-40ad160a2b4c" />

Lanjut kita konfigurasi wireless dengan menjadikan router sebagai Access Point, sehingga
client dapat terhubung ke router menggunakan wireless. Wireless → Interface Wireless
(wlan1) → Tab Wireless.
* Mode: ap bridge
* Band: 2GHz-B/G/N
* SSID: UKK-12-Sabiq
<img width="598" height="226" alt="image" src="https://github.com/user-attachments/assets/f2bb0c1e-4a58-4a73-9aec-3394efe329cd" />

Agar hanya client yang memiliki credential (password) yang dapat terhubung ke jaringan
wireless, perlu dibuat security pada interface wlan1 dengan menetapkan password pada
SSID yang digunakan. Wireless → Security Profiles → Add.
* Name: Key123
* Mode: dynamic keys
* Authentication Types: WPA PSK & WPA2 PSK
* WPA Pre-Shared Key: P@ssw0rd
* WPA2 Pre-Shared Key: P@ssw0rd
<img width="487" height="292" alt="image" src="https://github.com/user-attachments/assets/e44f6731-e5d8-48d2-bd99-38ce4ea06529" />

Setelah security profile-nya terbuat, selanjutnya kita terapkan security profile tersebut di interface wlan1. Wireless → wlan1.
* Security Profile: Key123
<img width="587" height="227" alt="image" src="https://github.com/user-attachments/assets/88c9c2a4-02d8-4df0-8e6e-d23dadd55807" />

Saat ini router sudah terkoneksi dengan modem isp di interface ether1, konfigurasikan DHCP Client pada ether1 dan kita diminta untuk tidak menggunakan option “Add Default Route”. IP → DHCP Client → Add.
* Checklist Use Peer DNS
* Checklist Use Peer NTP
* Add Default Route: No
<img width="396" height="177" alt="image" src="https://github.com/user-attachments/assets/69e90d0f-1e8a-4e6d-84ea-137ffb2e4b5c" />

Lanjut konfigurasi IP Address untuk interface wlan1 dengan menggunakan network “192.100+absen.20.2/24”. IP → Address → Add.
* Address: 192.112.20.1/24
* Network: 192.112.20.0
* Interface: wlan1
<img width="357" height="132" alt="image" src="https://github.com/user-attachments/assets/0aa7012f-43af-4bec-884c-bdcd739e58ce" />

Agar client tidak perlu set ip secara manual, kita perlu konfigurasi DHCP Server untuk interface wlan1 dengan ketentuan berikut:
* IP : 192.100.(no.absen).20.1/24
* Lease Time : 8 jam
* DNS : Primary: 8.8.8.8

Kita bisa gunakan wizard “DHCP Setup” untuk membuatnya. IP → DHCP Server → DHCP Setup.
* DHCP Server Interface: wlan1
* DHCP Address Space: 192.112.20.0/24
* Gateway For DHCP Network: Primary: 192.112.20.1
* Addresses To Give Out: 192.112.20.2-192.112.20.254
* DNS Server: 8.8.8.8 & 192.168.100.1
* Lease Time: 08:00:00
<img width="737" height="262" alt="image" src="https://github.com/user-attachments/assets/f28a2321-c849-4a45-8b86-e58ff8e6ab4a" />

Karena kita tidak menggunakan option “Add Default Route” saat konfigurasi DHCP Client,maka kita perlu membuat default route secara manual. IP → Routes → Add.
* Dst. Address: 0.0.0.0/0
* Gateway: 172.18.20.1
<img width="650" height="106" alt="image" src="https://github.com/user-attachments/assets/20abe72d-ebbd-439c-9316-642ca770b44c" />

Setelah membuat default route, router sudah langsung terhubung ke internet namun client masih belum terhubung ke internet, maka dari itu kita perlu konfigurasi NAT terlebih dahulu.
* Chain: srcnat
* Out Interface: ether1
* Action: masquerade
<img width="713" height="272" alt="image" src="https://github.com/user-attachments/assets/ee2bf4eb-2b71-432f-ab9d-c6bcbdcf167c" />

Selanjutnya agar client antar router bisa saling berkomunikasi, kita perlu konfigurasi Routing OSPF pada router. Routing → OSPF → Networks → Add.
* Advertise semua network
* Gunakan area bacbone
<img width="427" height="182" alt="image" src="https://github.com/user-attachments/assets/d6ff7622-6076-464f-9e9f-7142a49f5703" />

Konfigurasi pada “Router B” sudah selesai, sekarang saatnya lakukan pengujian baik dari
sisi router ataupun client.

# Pengujian 
## Pengujian Router A
Untuk pengujian kita bisa mulai dari test connectivity dengan menggunakan perintah ping lewat “cmd” dari sisi client, untuk detailnya seperti dibawah ini.
* Ping Gateway
* Ping Antar Client
* Ping Ke Internet (google.com)
<img width="615" height="322" alt="image" src="https://github.com/user-attachments/assets/39c0fb0f-a170-41fa-aea0-8b9ac2ab5d06" />

Selanjutnya karena tadi kita sudah konfigurasi Web Proxy untuk website “linux.org” & “httpforever.com”, maka kita bisa test dengan mengakses salah satu dari kedua web tersebut lewat web browser.
<img width="537" height="252" alt="image" src="https://github.com/user-attachments/assets/ec6680e2-a7b8-4703-b6c8-a323de7f0dbc" />

Karena kita konfigurasi Routing OSPF juga, kita bisa verify dengan cara check status Neighborship Router A dengan Router B, apakah statusnya up atau tidak. Routing → OSPF → Neighbors.
<img width="585" height="161" alt="image" src="https://github.com/user-attachments/assets/cd339a1c-99d3-4850-b1ec-ee148a030894" />

OSPF Neighborship di Router A statusnya Up yang artinya Routing OSPF-nya berjalan dan jika di check pada routing table, Router A ini memiliki route network client milik Router B. IP → Routes
<img width="646" height="172" alt="image" src="https://github.com/user-attachments/assets/98c0d44c-c4b8-4868-87f9-75828869428c" />

Di tandai dengan flag “DAo= Dynamic Active OSPF”. Pengujian di Router A sudah cukup lanjut ke Router B.

## Pengujian Router B
Pada Router B kita bisa mulai pengujian dari konfigurasi DHCP Server, apakah perangkat yang connect lewat wireless mendapatkan ip secara DHCP atau tidak?.
<img width="418" height="365" alt="image" src="https://github.com/user-attachments/assets/63aa386e-cf5b-4747-8cce-4cb475eb43ba" />

Bisa dilihat client yang connect via wireless ke Router B mendapatkan ip secara DHCP, Selanjutnya kita bisa lakukan tes connectivity dengan menggunakan perintah ping lewat cmd dari sisi client. untuk detailnya seperti dibawah ini.
* Ping Gateway
* Ping Antar Client
* Ping Ke Internet (google.com)
<img width="612" height="331" alt="image" src="https://github.com/user-attachments/assets/0501d4a4-ccd8-4ed7-a362-c0b643f608ae" />

Lanjut kita cek OSPF Neighbors pada Router B, apakah ada informasi mengenai neighbor Router A atau tidak?. Routing → OSPF → Neighbors.
<img width="526" height="147" alt="image" src="https://github.com/user-attachments/assets/e73d0bb1-00f4-484a-9413-9a25eabfe71f" />

Bisa dilihat pada tab neighbor terdapat informasi Router A, lanjut kita check pada routing table apakah terdapat informasi route mengenai network client Router A. IP → Routes.
<img width="622" height="185" alt="image" src="https://github.com/user-attachments/assets/604922dc-7784-4cc9-82c6-a4cd479324f0" />

Pada routing table Router B terdapat informasi route network client milik Router A, yang menandakan routing OSPF di kedua router berjalan dengan baik dan normal. Sejauh ini semua tugas sudah selesai dan hasilnya sesuai, untuk Web Proxy ini saya menggunakan port yang tidak sesuai dengan soal yaitu 8080 dan saya menambahkan website lain yaitu “httpforever.com”, dikarenakan ketika menggunakan port 3128 web proxy-nya ngga jalan

















