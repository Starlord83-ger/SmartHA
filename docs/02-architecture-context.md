# 02 Kontextdiagramm

Stand: 2026-11-25 · Status: Freigegeben (2026-11-25) · Paket 1 und 1b

Legende: Orange Linien sind bidirektionale Schnittstellen, türkise Knoten sind die Tablets.

```mermaid
flowchart LR
  subgraph Haus["Haus-LAN"]
    HA["Home Assistant<br/>Container in Portainer, Raspberry Pi 5 (16 GB)<br/>Master: Orchestrierung und Visualisierung<br/>interner Kalender"]
    LOX["Loxone Miniserver Gen1<br/>Gebäudeautomation<br/>Eigenbau-Schnittstelle"]
    SPC["Acre SPC<br/>Alarmanlage"]
    FRO["Fronius<br/>Energie, nur Anzeige"]
    NAS["Synology NAS<br/>Backup-Ziel"]
    TAB["Lenovo Tablets 12,1 Zoll<br/>Kiosk-Modus"]
  end
  HK["HomeKit<br/>Alternatives Frontend, tbd"]
  MOB["Mobile Geräte<br/>nur per WireGuard-VPN"]
  GH["GitHub privat<br/>Single Source of Truth"]

  HA <--> LOX
  HA <--> SPC
  HA <--> HK
  FRO --> HA
  TAB --> HA
  MOB --> HA
  HA --> NAS
  GH -.-> HA

  classDef tablet fill:#40E0D0,stroke:#0a7a6e,color:#000;
  class TAB tablet;
  linkStyle 0,1,2 stroke:#FF8C00,stroke-width:3px;
```

## Vertrauenszonen und Grundsätze

- Loxone und SPC bleiben führend für ihre Funktionen. HA ist Leitstand, nicht Ersatz.
- HA ist die einzige Instanz, die Frontends bedient. Tablets, Mobilgeräte und HomeKit greifen nie direkt auf SPC oder Loxone zu.
- Kein Zugang aus dem Internet, nur WireGuard.
- Die Schnittstellen im Detail stehen in `05-interfaces.md` (Paket 3, Entwurf).
