# 🏗️ Sistema IoT de Monitoreo y Detección Temprana de Fisuras

> **Proyecto Capstone APT 2026** — *Ingeniería en Conectividad y Redes*  
> **Institución:** Duoc UC Sede San Joaquín | **Docente:** Claudia Reinoso  
> **Integrantes:** Joao Jeldres | Néstor Navarro | Francianna Aro  

---

## 📌 Descripción del Proyecto
Sistema IoT integral de monitoreo estructural en tiempo real capaz de medir la apertura de fisuras en milímetros (mm) y transmitir telemetría de forma segura hacia un Centro de Operaciones de Red (NOC). Combina sensado con ESP32, protocolo ligero MQTT, procesamiento en Linux (Mosquitto + InfluxDB + Grafana), segmentación de red segura con VLANs/ACLs en Cisco Packet Tracer y alertas instantáneas en Telegram al superar el umbral crítico de **4.5 mm**.

---

## 🚀 Características Clave
- **Sensado IoT:** Medición de aperturas mecánicas (mm) mediante potenciómetro y microcontrolador ESP32 (Wokwi / Hardware).
- **Protocolo MQTT:** Envío continuo de telemetría estructurada en formato JSON sobre TCP/IP.
- **Ciberseguridad & Segmentación:** Aislamiento estricto de sensores en **VLAN 50** y servidores en **VLAN 100** mediante **ACLs extendidas** en Router0 (filtrado exclusivo de puertos TCP 1883/8883).
- **Procesamiento Time-Series:** Almacenamiento histórico de lecturas en **InfluxDB** sobre máquina virtual Kali Linux.
- **Plataforma NOC & Alertas:** Dashboards visuales en **Grafana** y notificaciones push automáticas en **Telegram Bot API** (< 1 segundo de latencia).

---

## 📐 Arquitectura del Sistema
```text
[ Sensor ESP32 / Wokwi ] ──(MQTT/JSON)──> [ VLAN 50 (IoT) ]
                                                │
                                    (ACL Filtering Router0)
                                                │
                                       [ VLAN 100 (Servers) ]
                                                │
                   ┌────────────────────────────┴────────────────────────────┐
                   ▼                                                         ▼
         [ Mosquitto Broker ]                                       [ Kali Linux VM ]
                   │                                                         │
                   └─────────────────────> [ InfluxDB ] <────────────────────┘
                                               │
                                        [ Grafana NOC ]
                                               │
                                  (Umbral >= 4.5 mm detectado)
                                               │
                                     [ Telegram Bot API ] ───> 📱 [ Alerta Push ]
