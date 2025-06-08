# 🏠 Homelabom Proxmox alapon

Azért hoztam létre ezt a homelabot, hogy a hálózati és vállalati szintű rendszergazdai ismereteimet mélyíthessem gyakorlati alkalmazás útján. Arra gondoltam, hogy a fejlesztés alatt előforduló problémák remek lehetőséget adnak, a tudásom bővítésére. Minden felmerülő problémát magamtól oldottam meg, angol nyelvű fórumok, cikkek, youtube/udemy videók böngészésével, és próbáltam rájönni mi lehet a megoldás. Szerencsére minden esetben rövidebb vagy hosszabb idő alatt, de megfejtettem, hogy valami miért nem működik vagy nem úgy működik, ahogyan azt elvárom. Végig azon voltam, hogy a alapjaiban megérthessem a hiba forrását, megelőzve azt, hogy legközelebb is valamilyen formájában belefussak.

---

## 🎯 Célok
### ✅ Megvalósított célok

- A fő cél természetesen, a gyakorlati rendszergazdai/hálózati tudás folyamatos mélyítése.
- Számomra új technológiák megismerése, autodidakta módon.
- Különféle operációs rendszerek használata (CentOS 9 Stream, Ubuntu 22.04 desktop, Ubuntu 22.04 server, Windows 11, Windows Server 2019).
- Virtualizáció: Korábban rendszeresen használtam a type 2 hypervisor-t (VMware Workstation Pro, VirtualBox). Gyakorlati időm alatt segítettem a munkáltatómnak, aki váltottak az ESXi-ről, a számukra és számomra is újdonság erejével ható XCP-ng hypervisorra. A műveletet dokumentáltam nekik, hogy az alapján később majd támpontot adva, az alapján tudják majd beállítani a többi szervert. Ekkor kaptam kedvet, és otthon is ki szerettem volna próbálni a type 1 hypervisor-t. Az én választásom a Proxmoxra esett. 
- Több gép központi vezérlése (Ansible + Semaphore).
- Távoli elérés biztosítása (VPN, RDP, SSH).
- Saját privát domain oldása privát, lokális DNS szerverrel, illetve publikus domain DNS oldása DNS override útján (BIND9, Namecheap, Cloudflare, Pi-hole).
- Disaster recovery (Nextcloud, Clonezilla, Proxmox Backup Server, FreeFileSync).
- Egységes, központosított felhasználó- és jogosultságkezelés (Freeipa, Freeradius).
- Proxy (Nginx Proxy Manager, Apt-cacher-ng).
- Monitorozás (Zabbix).
- Router (Pfsense, rajta futó szolgáltatások DDNS, DHCP, NTP, Wireguard, OpenVPN).
- Egyéb kényelmi megoldások (iVentoy PXE boot-hoz, Proxmoxon template + Cloud-Init).
- Password management (Vaultwarden).

### 🕐 Megvalósításra váró célok
- High availability  megvalósítása: Venni szeretnék egy harmadik tiny PC-t (Lenovo M920Q tiny), és így a 3 Proxmox gépet cluster rendezném. Vásárolnék még további 3 SSD-t, és CEPH-et használnék 3-as replikációval.

---

## 🔭 Főbb komponensek

### 🖥️ Virtualizáció

- **Proxmox VE** mint hypervisor fut két gépen
- VM és LXC konténer használata, ahol az adott szolgáltatás installálása nem ment máshogyan, ott docker-t használok
- **iVentoy PXE boot** szerver Clonezilla mentésekhez

---

### 🔐 Biztonság és hozzáférés

- **SSH**: kulcsalapú, jelszavas és root belépés tiltva
- **FreeIPA**: központi LDAP hitelesítés, sudo jogosultságokkal
- **FreeRADIUS**: pfSense + OpenVPN hitelesítés fallback támogatással (ha nem elérhető, lokális userrel belépés)
- **Pi-hole**: DNS szintű reklámszűrés a hálózaton
- **Tűzfal/NAT**: pfSense használata

---

## 📦 Telepített szolgáltatások

| Szolgáltatás     | Leírás                                                      |
|------------------|-------------------------------------------------------------|
| **Bind9**        | Belső DNS (`*.trkrolf.com`) feloldás                        |
| **FreeIPA**      | LDAP-alapú hitelesítés és sudo szabálykezelés               |
| **FreeRADIUS**   | pfSense és OpenVPN beléptetés támogatás                     |
| **Pi-hole**      | Reklámszűrés az egész hálózaton                             |
| **Zabbix**       | Rendszermonitorozás és riasztás                             |
| **PBS**          | Proxmox Backup Server VM-ekhez és LXC-khez                  |
| **Nextcloud**    | Fájlmegosztás és szinkronizálás                             |
| **Nginx**        | Reverz proxy (Docker-ben), Cloudflare + Let's Encrypt SSL  |
| **Ansible**      | Automatizálás + Semaphore webes felület                     |
| **apt-cacher-ng**| Frissítések cache-elése, sávszélesség csökkentése érdekében|
| **Chrony/NTP**   | Időszinkronizálás, pfSense mint NTP szerver                |

---

## 🌐 Hálózat

- **pfSense** VM:
  - NAT: 192.168.2.0/24 → 192.168.1.0/24
  - DHCP: statikus IP MAC-hez kötve
  - NTP szerver az LXC-k és VM-ek számára
  - VPN megoldások:
    - **WireGuard** (split/full tunnel, mobil elérés)
    - **Tailscale** (zero-config, távoli elérés)
    - **OpenVPN** (full tunnel + Pi-hole szűrés)

- **DDNS + Cloudflare**
  - Saját domain (`*.trkrolf.com`)
  - Cloudflare + Let’s Encrypt DNS-01 hitelesítés
  - DDNS, hogy változó publikus IP mellett is működjön VPN
![kép](https://github.com/user-attachments/assets/d518d7cf-809b-4dc6-9af2-eab210de6ee1)

---

## 🧠 Automatizálás és user kezelés

- **FreeIPA**-ban hozom létre a `ansibleuser` nevű felhasználót
- Minden gépen ez fut Ansible-playbookokhoz (nem kell 10 gépen külön user)
- Ansible + Semaphore kezel:
  - Frissítések telepítése
  - SSH kulcsok
  - Felhasználók, jogosultságok, fájlkezelés

---

## 🧪 Disaster Recovery

- PXE boot → Clonezilla → mentés külső SSD-re SSH-n keresztül
- Disaster szimulációk és helyreállítási tesztek rendszeresen
- Cloud-init VM sablon gyors VM indításhoz

---

## 🛠️ Használt technológiák

- **Operációs rendszerek:** Ubuntu 22.04, CentOS 9, Windows 11
- **Infrastruktúra:** Proxmox VE, LXC, Docker, PXE boot
- **Biztonság:** FreeIPA, RADIUS, OpenVPN, WireGuard, Tűzfalak
- **Automatizálás:** Ansible, Semaphore
- **Monitorozás:** Zabbix
- **Mentés:** Proxmox Backup Server
- **Távoli elérés:** Tailscale, WireGuard, OpenVPN

---

## 💼 Miért hasznos ez a homelab?

Ez a rendszeres gyakorlatokkal fenntartott környezet segít abban, hogy:

- Magabiztosan kezeljek egy valós hálózati és rendszerinfrastruktúrát
- Automatizáljak rutinfeladatokat több rendszerre egyszerre
- Ismerjem a hitelesítési, VPN, DNS, és mentési megoldásokat
- Felkészült legyek junior IT / rendszergazda / hálózatos pozíciókra

---

## 📸 Képernyőképek *(opcionális)*

_Szükség esetén tehetek ide Proxmox, Zabbix, Semaphore vagy Nextcloud képeket._

---

## 📬 Megvalósítandó
Majd ideírni, hogy miket akarok még megvalósítani


