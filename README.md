# 🖥️ HomeLab VMware ESXi

Documentación completa de mi laboratorio de virtualización doméstico con VMware ESXi en servidor HP ProLiant ML110 G6.

![ESXi Version](https://img.shields.io/badge/ESXi-6.5.0-blue)
![Status](https://img.shields.io/badge/Status-Active-green)
![VMs](https://img.shields.io/badge/VMs-2%20Running-success)

---

## 📋 Tabla de Contenidos

- [Visión General](#-visión-general)
- [Especificaciones del Hardware](#-especificaciones-del-hardware)
- [Configuración de ESXi](#️-configuración-de-esxi)
- [Máquinas Virtuales](#️-máquinas-virtuales)
- [Uso de Recursos](#-uso-de-recursos)
- [Documentación](#-documentación)
- [Roadmap](#️-roadmap)

---

## 🎯 Visión General

Este HomeLab está diseñado para prácticas de virtualización, administración de servidores Linux, despliegue de contenedores Docker y sistemas de monitoreo. El objetivo principal es desarrollar habilidades prácticas en administración de infraestructura IT y ciberseguridad.

### Propósitos del Lab:
- ✅ Virtualización con VMware ESXi
- ✅ Gestión de contenedores con Docker
- ✅ Servicios de red (SMB/CIFS)
- ✅ Monitoreo de infraestructura
- ✅ Prácticas de hardening y seguridad

---

## 🔧 Especificaciones del Hardware

### Servidor Principal
| Componente | Especificación |
|------------|----------------|
| **Fabricante** | HP |
| **Modelo** | ProLiant ML110 G6 |
| **Procesador** | Intel Xeon X3430 @ 2.40GHz (4 cores) |
| **Memoria RAM** | 8 GB DDR3 |
| **Almacenamiento** | 2.72 TB (RAID configurado) |
| **Red** | Gigabit Ethernet |

### Capacidades
- **CPU Total**: 9.6 GHz (4 cores físicos)
- **RAM Total**: 7.99 GB utilizable
- **Storage Total**: 2.72 TB
- **Hypervisor**: VMware ESXi 6.5.0 (Build 5310538)

---

## ⚙️ Configuración de ESXi

### Información del Sistema
- **Versión**: ESXi 6.5.0 (Build 5310538)
- **Fecha de Instalación**: 16 de septiembre de 2025
- **Perfil de Imagen**: HPE-ESXi-6.5.0-iso-650.10.1.5.20 (Hewlett Packard Enterprise)
- **Estado de vSphere HA**: Sin configurar (HomeLab standalone)
- **vMotion**: Compatible

### Configuración de Red
- **Management Network**: 192.168.30.10
- **Hostname**: ESXI
- **DNS**: Configurado en red local

---

## 🖥️ Máquinas Virtuales

### VM 1: Docker Server
![Ubuntu](https://img.shields.io/badge/Ubuntu-64--bit-orange)
![Docker](https://img.shields.io/badge/Docker-Enabled-blue)

**Propósito**: Servidor principal para despliegue de contenedores Docker

| Especificación | Valor |
|----------------|-------|
| **Sistema Operativo** | Ubuntu Linux (64 bits) |
| **vCPU** | Asignado dinámicamente |
| **RAM** | 4.01 GB |
| **Storage** | 393.9 GB |
| **Estado** | ✅ Running |
| **Hostname** | ubuntu |

**Servicios/Contenedores Deployados**:
- Servicios de monitoreo (Zabbix/Grafana - en desarrollo)
- Aplicaciones containerizadas
- Entorno de desarrollo y testing

---

### VM 2: DietPi VM
![Debian](https://img.shields.io/badge/Debian-10-red)
![SMB](https://img.shields.io/badge/SMB-Server-green)

**Propósito**: Servidor de archivos SMB/CIFS ligero

| Especificación | Valor |
|----------------|-------|
| **Sistema Operativo** | Debian GNU/Linux 10 (Buster) |
| **vCPU** | Asignado dinámicamente |
| **RAM** | 540 MB |
| **Storage** | 48.74 GB |
| **Estado** | ⚪ Stopped (On-demand) |

**Servicios**:
- Servidor SMB/CIFS para compartir archivos en red local
- Configuración optimizada para bajo consumo de recursos

---

## 📊 Uso de Recursos

### Estado Actual del Host

| Recurso | Usado | Disponible | Utilización |
|---------|-------|------------|-------------|
| **CPU** | 241 MHz | 9.6 GHz | ~2.5% |
| **Memoria** | 6.08 GB | 7.99 GB | 76% |
| **Storage** | 597.12 GB | 2.72 TB | 21% |

### Distribución de Recursos por VM
```
Docker Server:
├── CPU: 199 MHz
├── RAM: 4.01 GB (66% del total usado)
└── Storage: 393.9 GB

DietPi VM:
├── CPU: 3 MHz (cuando activa)
├── RAM: 540 MB (cuando activa)
└── Storage: 48.74 GB
```

---

## 📚 Documentación

- 📖 [Instalación de ESXi](docs/instalacion-esxi.md) *(Próximamente)*
- 🌐 [Configuración de Red](docs/configuracion-red.md) *(Próximamente)*
- 🐳 [Setup de Docker Server](docs/docker-server-setup.md) *(Próximamente)*
- 📁 [Configuración de DietPi SMB](docs/dietpi-smb.md) *(Próximamente)*

---

## 🗺️ Roadmap

### Completado ✅
- [x] Instalación de ESXi en HP ProLiant ML110 G6
- [x] Configuración de red management
- [x] Deployment de VM Ubuntu con Docker
- [x] Deployment de VM DietPi para SMB
- [x] Documentación inicial del HomeLab

### En Progreso 🚧
- [ ] Implementación de sistema de monitoreo (Zabbix + Grafana)
- [ ] Documentación detallada de configuraciones
- [ ] Optimización de uso de recursos

### Planificado 📅
- [ ] Implementación de backups automatizados con Veeam
- [ ] Despliegue de servicios adicionales en contenedores
- [ ] Configuración de VLAN para segmentación de red
- [ ] Implementación de sistema de logs centralizado
- [ ] Prácticas de hardening y seguridad

---

## 🛠️ Tecnologías Utilizadas

![VMware](https://img.shields.io/badge/VMware-ESXi%206.5-blue?logo=vmware)
![Linux](https://img.shields.io/badge/Linux-Ubuntu%20|%20Debian-orange?logo=linux)
![Docker](https://img.shields.io/badge/Docker-Containers-blue?logo=docker)
![Zabbix](https://img.shields.io/badge/Zabbix-Monitoring-red?logo=zabbix)
![Grafana](https://img.shields.io/badge/Grafana-Visualization-orange?logo=grafana)

---

## 📧 Contacto

**Felipe Loaiza Rodriguez**
- LinkedIn: [linkedin.com/in/felipeloaizar](https://linkedin.com/in/felipeloaizar)
- Email: feloaiza@outlook.com
- Portfolio: [curriculum-felipe-loaiza.vercel.app](https://curriculum-felipe-loaiza.vercel.app)

---

## 📝 Licencia

Este proyecto está documentado con fines educativos y de portfolio profesional.

---

*Última actualización: Enero 2026*
