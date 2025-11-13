# 🧩 Tugas Akhir Praktikum Jaringan Komputer – Percobaan 3  
**Nama:** Puan Akeyla Maharani Munaji  
**NPM:** 2315061070  
**Kelas:** Jaringan Komputer D  
**Judul:** Configure VLANs and Trunking (Physical Mode)  
**Link YouTube:** [https://youtu.be/CteWXPhbx1U](https://youtu.be/CteWXPhbx1U)

---

## 🎯 Tujuan Percobaan
- Membangun topologi jaringan menggunakan **Cisco Packet Tracer (Physical Mode)**.  
- Mengonfigurasi **VLAN (Virtual Local Area Network)** pada dua switch.  
- Mengatur **trunking 802.1Q** antar switch agar VLAN yang sama dapat berkomunikasi.  
- Melakukan **verifikasi konektivitas** antar host dalam VLAN yang sama.  
- Menambahkan **PC-C dan PC-D** tanpa mengganggu konfigurasi awal.

---

## ⚙️ Topologi Jaringan

**Perangkat yang digunakan:**
- 2 Switch Cisco 2960 (S1 dan S2)
- 4 PC: PC-A, PC-B, PC-C, dan PC-D

**Koneksi Fisik:**
PC-A → S1 port Fa0/6

PC-B → S2 port Fa0/18

PC-C → S1 port Fa0/11

PC-D → S2 port Fa0/19

S1 ↔ S2 → Trunk via port Fa0/1


---

## 🧱 Konfigurasi VLAN dan IP Address

| Perangkat | Interface | VLAN | IP Address | Subnet Mask | Default Gateway |
|------------|------------|------|-------------|--------------|-----------------|
| S1 | VLAN 99 | Management | 192.168.1.11 | 255.255.255.0 | - |
| S2 | VLAN 99 | Management | 192.168.1.12 | 255.255.255.0 | - |
| PC-A | NIC | VLAN 10 | 192.168.10.3 | 255.255.255.0 | 192.168.10.1 |
| PC-B | NIC | VLAN 10 | 192.168.10.4 | 255.255.255.0 | 192.168.10.1 |
| PC-C | NIC | VLAN 10 | 192.168.10.5 | 255.255.255.0 | 192.168.10.1 |
| PC-D | NIC | VLAN 10 | 192.168.10.6 | 255.255.255.0 | 192.168.10.1 |

---

## 🧩 Langkah-Langkah Konfigurasi

### Pembuatan VLAN
S1(config)# vlan 10

S1(config-vlan)# name Operations

S1(config)# vlan 99

S1(config-vlan)# name Management

S1(config)# vlan 1000

S1(config-vlan)# name Native

S2(config)# vlan 10

S2(config-vlan)# name Operations

S2(config)# vlan 99

S2(config-vlan)# name Management

### Penugasan Port ke VLAN
# PC-A di S1
S1(config)# interface f0/6

S1(config-if)# switchport mode access

S1(config-if)# switchport access vlan 10

# PC-B di S2
S2(config)# interface f0/18

S2(config-if)# switchport mode access

S2(config-if)# switchport access vlan 10

# Tambahan PC-C di S1
S1(config)# interface f0/11

S1(config-if)# switchport mode access

S1(config-if)# switchport access vlan 10

# Tambahan PC-D di S2
S2(config)# interface f0/19

S2(config-if)# switchport mode access

S2(config-if)# switchport access vlan 10

S2(config)# vlan 1000

S2(config-vlan)# name Native

### Konfigurasi Trunk Antar Switch
S1(config)# interface f0/11

S1(config-if)# no shutdown

S2(config)# interface f0/19

S2(config-if)# no shutdown

### Verifikasi Konfigurasi
show vlan brief

show interfaces trunk

show ip interface brief

Hasil Verifikasi VLAN:

VLAN 10  Operations  active  Fa0/6, Fa0/11, Fa0/18, Fa0/19

VLAN 99  Management  active

VLAN 1000 Native      active

### Pengujian Konektivitas
Uji ping antar PC di VLAN 10:

PC-A ↔ PC-B ✅

PC-C ↔ PC-D ✅

PC-A ↔ PC-C ✅

PC-B ↔ PC-D ✅

Semua host dalam VLAN 10 berhasil saling terhubung menandakan trunk dan VLAN berfungsi dengan baik.
