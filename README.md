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

# Daftar & Ketentuan
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
• Hostname
• IP Address
• DHCP Client
• NAT
• Web Proxy
• Firewall
• Routing OSPF




