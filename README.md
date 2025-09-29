# Proyek-Segmentasi-Jaringan-VLAN

Proyek segmentasi jaringan dengan VLAN dan pencegahan loop menggunakan STP

![Cisco](https://img.shields.io/badge/Cisco-Switch-blue?style=for-the-badge&logo=cisco&logoColor=white)
![VLAN](https://img.shields.io/badge/VLAN-Segmentation-green?style=for-the-badge)
![STP](https://img.shields.io/badge/STP-Protocol-orange?style=for-the-badge)

---

## 📝 Deskripsi
Proyek ini mengimplementasikan **VLAN (Virtual LAN)** dan **Spanning Tree Protocol (STP)** untuk segmentasi jaringan dan pencegahan loop. Implementasi mencakup:
- Konfigurasi VLAN untuk segmentasi jaringan logikal
- Implementasi trunk port untuk komunikasi antar switch
- Konfigurasi STP untuk mencegah loop jaringan
- Verifikasi tabel spanning tree dan status port

---

## 🏗️ Topologi Jaringan
| Perangkat | VLAN | Trunk Ports | STP Priority | Status |
|-----------|------|-------------|--------------|--------|
| **SW1** | 10 (Sales), 20 (IT) | fa0/1-6 | VLAN 10: 8192 | Non-Root Bridge |
| **SW2** | 10 (Sales), 20 (IT) | fa0/1-6 | Default | Non-Root Bridge |
| **SW-BRIDGE** | 10 (Sales), 20 (IT) | fa0/1-6 | VLAN 1: 4096 | **Root Bridge** |

---

## 🖼️ Gambar Topologi
```
    [SW1]-------------------[SW-BRIDGE]-------------------[SW2]
      |                       |                           |
  Trunk (fa0/1-6)         Root Bridge                Trunk (fa0/1-6)
      |                       |                           |
  VLAN 10, 20            VLAN 10, 20                 VLAN 10, 20
```

<img width="827" height="487" alt="image" src="https://github.com/user-attachments/assets/7fe337b8-3039-439e-8460-4b7e09d8813f" />

---

## 📊 Output Verifikasi
### Spanning Tree SW1
```
SW1# show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0002.4AE0.DAB3
             Cost        19
             Port        1 (FastEthernet0/1)
  
  Interface        Role Sts Cost      Prio.Nbr Type
  ---------------- ---- --- --------- -------- --------------------------------
  Fa0/1            Root FWD 19        128.1    P2p
  Fa0/2            Altn BLK 19        128.2    P2p
  Fa0/5            Desg FWD 19        128.5    P2p

VLAN0010
  Spanning tree enabled protocol ieee
  Root ID    Priority    8192
             Address     0060.70DB.C5E3
             This bridge is the root
```

### Spanning Tree SW2
```
SW2# show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0002.4AE0.DAB3
             Cost        19
             Port        1 (FastEthernet0/1)
  
  Interface        Role Sts Cost      Prio.Nbr Type
  ---------------- ---- --- --------- -------- --------------------------------
  Fa0/1            Root FWD 19        128.1    P2p
  Fa0/3            Desg FWD 19        128.3    P2p

VLAN0010
  Spanning tree enabled protocol ieee
  Root ID    Priority    8202
             Address     0060.70DB.C5E3
             Cost        19
             Port        3 (FastEthernet0/3)
```

### Spanning Tree SW-BRIDGE
```
SW-BRIDGE# show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0002.4AE0.DAB3
             This bridge is the root
  
  Interface        Role Sts Cost      Prio.Nbr Type
  ---------------- ---- --- --------- -------- --------------------------------
  Fa0/1            Desg FWD 19        128.1    P2p
  Fa0/2            Desg FWD 19        128.2    P2p
```

### Verifikasi VLAN
```
SW1# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24
10   Sales                            active    
20   IT                               active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
```

---

**luqmanaru**  
