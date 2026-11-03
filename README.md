# DHCP-Shield — Protege tu red desde el DHCP

Laboratorio de seguridad en **GNS3** y herramienta de auditoría en **Python**
para detectar y mitigar ataques de **DHCP** (*Rogue DHCP* y *DHCP Starvation*)
en redes conmutadas.

---

## Características Principales

### 1. Laboratorio GNS3 (Cisco IOSv / IOSvL2)

- **Topología lista para importar**: 1 router DHCP, 2 switches, 3 PCs
  cliente y 1 equipo atacante (Kali Linux).
- **Escenarios incluidos**:
  - Red base con DHCP funcionando.
  - Red con servidor DHCP falso (*Rogue DHCP*).
  - Red protegida con las mitigaciones activadas.
- **Mitigaciones configuradas**: `DHCP Snooping`, `Port Security`,
  `Dynamic ARP Inspection (DAI)` y `IP Source Guard`.

### 2. Auditor de capturas (Python + Scapy)

- **Detección de Rogue DHCP**: analiza un archivo `.pcap` y alerta si hay
  más de un servidor respondiendo con `DHCPOFFER`.
- **Detección de Starvation**: cuenta solicitudes por MAC y marca ráfagas
  anormales.
- **Reporte** en consola y exportación a `JSON` / `Markdown`.

### 3. Verificador de configuración

- Revisa archivos `.cfg` de switches y comprueba si tienen activados
  snooping, puertos *trusted* correctos y límites de MAC por puerto.

---

## Arquitectura del Proyecto

DHCP-Shield/

├── lab/

│   ├──topologia.gnsproject        # Proyecto GNS3 exportado

│   └── configs/

│       ├── router-dhcp.cfg         # Servidor DHCP legítimo

│       ├── switch-base.cfg         # Sin protecciones

│       └── switch-seguro.cfg       # Snooping + Port Security + DAI

├── auditor/

│   ├── main.py                     # CLI principal

│   ├── pcap_analyzer.py            # Rogue DHCP y Starvation

│   └── config_checker.py           # Revisión de .cfg

├── capturas/

│   └── ejemplo_rogue.pcap          # Captura de prueba

├── docs/

│   ├── guia-laboratorio.md         # Paso a paso del lab

│   └── mitigaciones.md             # Explicación de cada defensa

├── tests/

│   └── test_auditor.py             # Pruebas con pytest

├── requirements.txt

└── README.md

---

## Ejecución Local y Pruebas

### 1. Laboratorio

1. Abre GNS3 e importa `lab/topologia.gnsproject`.
2. Carga `switch-base.cfg` y observa el ataque.
3. Carga `switch-seguro.cfg` y confirma que el ataque falla.

### 2. Auditor

pip install -r requirements.txt
python -m auditor.main --pcap capturas/ejemplo_rogue.pcap
python -m auditor.main --config lab/configs/switch-seguro.cfg

### 3. Pruebas

pytest -v

---

## Aviso

Uso exclusivo **educativo** y en entornos de laboratorio propios.
