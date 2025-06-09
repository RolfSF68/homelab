# HomelabGPT

Ez a repository a saját homelab infrastruktúrám dokumentációját tartalmazza, amelyet tanulás és önfejlesztés céljából építettem. A célom egy vállalati szintű környezet modellezése volt otthoni erőforrásokkal, modern technológiák (virtualizáció, konténerek, DNS, fordított proxy, monitoring, biztonság) segítségével.

---

## 🧠 Célok

- Gyakorlati tudás megszerzése hálózatüzemeltetés, Linux szerverek és DevOps eszközök terén
- Automatizálás, hibaelhárítás és monitoring kipróbálása valós helyzetekben
- Portfólió építése álláskereséshez

---

## ⚙️ Használt technológiák

| Terület | Eszköz |
|--------|--------|
| Virtualizáció | Proxmox |
| Konténerizáció | Docker, LXC |
| DNS | Bind9 |
| Reverse proxy + SSL | Nginx Proxy Manager + Cloudflare |
| Monitoring | Zabbix |
| VPN | Tailscale |
| Automatizálás (tervben) | Ansible |

---

## 🗂️ Főbb projektek és funkciók

- **Proxmox alapú virtualizációs környezet**
  - Több VM és LXC konténer futtatása
  - Erőforrásmenedzsment és snapshotok használata

- **Helyi DNS (Bind9)**
  - `*.trkrolf.com` aldomainjeim lokális feloldása
  - Lokális fejlesztés Cloudflare nélkül is

- **Reverse Proxy (Nginx Proxy Manager)**
  - Különböző szolgáltatások elérése saját domain alatt
  - Cloudflare integráció, automatikus SSL

- **Zabbix Monitoring**
  - VM-ek, konténerek és hálózati forgalom figyelése
  - E-mail értesítések hibákról

- **Tailscale**
  - Külső hálózatból biztonságos elérés a homelabra

---

## 📸 Képernyőképek / diagramok (javasolt ide betenni)

- Hálózati topológia (pl. Draw.io diagram)
- Nginx Proxy Manager dashboard
- Zabbix monitoring példa

---

## 💼 Mire használható ez az anyag?

Ez a dokumentáció betekintést nyújt:
- Rendszeres karbantartási és hibakeresési gyakorlatomba
- Valós IT infrastruktúra modellezésébe
- Proaktív tanulási hozzáállásomba

---

## 📎 Álláskeresési link

Ez a projekt a [szakmai önéletrajzomban](https://github.com/RolfSF68/homelabgpt) is szerepel, portfólióként.

