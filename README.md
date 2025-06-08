# 🏠 Saját homelabom Proxmox alapon

Azért hoztam létre ezt a homelab környezetet, hogy a hálózati és vállalati szintű rendszergazdai ismereteimet mélyíthessem gyakorlati problémák útján. Bíztam benne, hogy a fejlesztés alatt előforduló problémák sorozata majd bővíti a tudásomat. Minden felmerülő problémát magamtól oldottam meg, aktív segítségét senkitől nem kértem, tehát nem írkáltam fórumokra és nem kérdeztem senkitől, kizárólag passzívan olvastam, és próbáltam rájönni mi lehet a probléma. Szerencsére minden esetben rövidebb vagy hosszabb avagy még hosszabb idő alatt, de megfejtettem, hogy valami miért nem működik vagy nem úgy működik, ahogyan azt elvárom. Végig azon voltam, hogy a alapjaiban megérthessem a hiba forrását, megelőzve így, hogy legközelebb is valamilyen formájában belefussak.

---

## 🎯 Célok

- Valós rendszergazdai/hálózati tudás megszerzése (junior szint felett)
- Automatizálás gyakorlása Ansible + LDAP segítségével
- VPN, DNS, biztonságos beléptetés és mentés konfigurálása
- Disaster recovery és magas rendelkezésre állás megvalósítása
- Egységes, központosított felhasználó- és jogosultságkezelés

---

## 🧱 Főbb komponensek

### 🖥️ Virtualizáció

- **Proxmox VE** mint hipervizor (több gépen)
- VM és LXC konténer vegyes használata
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


