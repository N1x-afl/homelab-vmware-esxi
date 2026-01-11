# 📖 Instalación de ESXi en HP ProLiant ML110 G6

Guía completa de instalación de VMware ESXi 6.5.0 en servidor HP ProLiant ML110 G6 utilizando imagen customizada de HPE.

---

## 📋 Tabla de Contenidos

- [Requisitos Previos](#requisitos-previos)
- [Descarga de ESXi](#descarga-de-esxi)
- [Preparación del Medio de Instalación](#preparación-del-medio-de-instalación)
- [Configuración de BIOS](#configuración-de-bios)
- [Proceso de Instalación](#proceso-de-instalación)
- [Configuración Inicial](#configuración-inicial)
- [Post-Instalación](#post-instalación)
- [Verificación](#verificación)
- [Lecciones Aprendidas](#lecciones-aprendidas)

---

## 🔧 Requisitos Previos

### Hardware Mínimo
- ✅ Procesador compatible con virtualización (Intel VT-x o AMD-V)
- ✅ Mínimo 4GB RAM (Recomendado: 8GB+)
- ✅ Adaptador de red Gigabit Ethernet
- ✅ Disco duro con mínimo 8GB de espacio

### Hardware Utilizado
| Componente | Especificación |
|------------|----------------|
| **Servidor** | HP ProLiant ML110 G6 |
| **CPU** | Intel Xeon X3430 @ 2.40GHz (4 cores) |
| **RAM** | 8 GB DDR3 |
| **Storage** | 2.72 TB |
| **Red** | Gigabit Ethernet |

### Software Necesario
- USB booteable (mínimo 8GB)
- Rufus (para crear USB booteable)
- Imagen ISO de ESXi

---

## 💿 Descarga de ESXi

### Versión Instalada
- **Producto**: VMware ESXi
- **Versión**: 6.5.0 Update 3 (Build 5310538)
- **Imagen**: HPE Customized Image ESXi 6.5.0
- **Profile**: HPE-ESXi-6.5.0-iso-650.10.1.5.20

### Fuente de Descarga
La imagen customizada de HPE se descarga desde:
- Portal de HPE Support
- VMware Customer Connect (con cuenta)

**Ventajas de la imagen HPE:**
- ✅ Drivers optimizados para hardware HP
- ✅ Mayor compatibilidad con controladores
- ✅ Soporte oficial de HPE

---

## 🔨 Preparación del Medio de Instalación

### Crear USB Booteable con Rufus

1. **Descargar Rufus**
   - Sitio oficial: https://rufus.ie
   - Versión recomendada: 3.x o superior

2. **Configuración en Rufus**
```
   Dispositivo: [Tu USB de 8GB+]
   Tipo de arranque: Imagen ISO
   Seleccionar: [ESXi ISO descargado]
   Esquema de partición: MBR
   Sistema destino: BIOS o UEFI
   Sistema de archivos: FAT32
```

3. **Crear USB**
   - Clic en "EMPEZAR"
   - Esperar a que finalice (5-10 minutos)
   - Verificar que se creó correctamente

---

## ⚙️ Configuración de BIOS

### Acceso a BIOS
- Encender servidor HP ProLiant ML110 G6
- Presionar **F9** durante el POST
- Navegar con las flechas del teclado

### Configuraciones Realizadas

#### 1. Boot Order (Orden de Arranque)
**Antes:**
```
1. Hard Drive
2. USB Device
3. Network Boot
```

**Después:**
```
1. USB Device    ← Modificado
2. Hard Drive
3. Network Boot
```

#### 2. Virtualización
Verificar que esté habilitada:
- **Intel Virtualization Technology (VT-x)**: `[Enabled]`
- **Intel VT-d**: `[Enabled]` (si está disponible)

#### 3. Otras Configuraciones
- **SATA Mode**: AHCI (recomendado)
- **RAID**: No configurado (no fue necesario)
- **USB Support**: Enabled

### Guardar y Reiniciar
- Presionar **F10** para guardar
- Confirmar cambios
- Servidor reinicia automáticamente

---

## 🚀 Proceso de Instalación

### Paso 1: Boot desde USB

1. Insertar USB booteable
2. Servidor inicia desde USB automáticamente
3. Aparece pantalla de carga de ESXi
```
Loading ESXi installer...
```

### Paso 2: Pantalla de Bienvenida

- Aparece: "Welcome to the VMware ESXi 6.5.0 Installation"
- Presionar **Enter** para continuar

### Paso 3: Aceptar EULA

- Leer End User License Agreement
- Presionar **F11** para aceptar

### Paso 4: Selección de Disco

- ESXi detecta discos disponibles
- Seleccionar disco para instalación (2.72 TB en este caso)
- Presionar **Enter**

⚠️ **Advertencia**: Todos los datos del disco seleccionado serán eliminados

### Paso 5: Layout de Teclado

- Seleccionar: **Spanish** o **US Standard**
- Presionar **Enter**

### Paso 6: Contraseña de Root

- Ingresar contraseña de administrador (mínimo 7 caracteres)
- Confirmar contraseña
- Presionar **Enter**

🔐 **Nota de Seguridad**: Por razones de seguridad, la contraseña no se documenta aquí

### Paso 7: Confirmación

- Revisar resumen de instalación
- Presionar **F11** para iniciar instalación

### Paso 8: Instalación
```
Installing ESXi...
[████████████████████] 100%
```

Tiempo aproximado: **5-10 minutos**

### Paso 9: Finalización

- Mensaje: "Installation Complete"
- **Remover USB**
- Presionar **Enter** para reiniciar

---

## 🔧 Configuración Inicial

### DCUI (Direct Console User Interface)

Después del reinicio, aparece la consola de ESXi:
```
VMware ESXi 6.5.0 (VMware, Inc.)

192.168.30.10
```

### Configuración de Red

1. Presionar **F2** para personalizar sistema
2. Login: `root`
3. Password: [tu contraseña]

#### Configure Management Network

**Parámetros configurados:**

| Configuración | Valor |
|---------------|-------|
| **VLAN ID** | 30 |
| **IPv4** | 192.168.30.10 |
| **Subnet Mask** | 255.255.255.0 |
| **Gateway** | 192.168.30.1 |
| **DNS** | [DNS de tu red] |
| **Hostname** | ESXI |

**Pasos:**
1. Configure Management Network → IPv4 Configuration
2. Set static IPv4 address
3. Ingresar IP: `192.168.30.10`
4. Ingresar Subnet Mask
5. Ingresar Default Gateway
6. VLAN (optional) → VLAN ID: `30`
7. DNS Configuration → Hostname: `ESXI`
8. **Esc** para salir
9. Confirmar cambios → **Y**

### Reiniciar Management Network
```
Restart Management Network? [Y/N]: Y
```

La configuración de red se aplica y el servidor queda accesible en: `https://192.168.30.10`

---

## 🌐 Post-Instalación

### Acceso Web (VMware Host Client)

1. **Abrir navegador web**
   - URL: `https://192.168.30.10`
   - Aceptar certificado autofirmado

2. **Login**
   - Usuario: `root`
   - Password: [tu contraseña]

3. **Interface Web**
   - ✅ No requiere instalación de vSphere Client
   - ✅ Acceso completo desde navegador
   - ✅ Gestión de VMs, storage, networking

### Configuración de Licencia

**Licencia instalada:**
- **Producto**: VMware vSphere with Operations Management 6 Enterprise Plus
- **Características**:
  - Unlimited number of VMs
  - vMotion, Storage vMotion
  - High Availability (HA)
  - Distributed Resource Scheduler (DRS)

**Activar licencia:**
1. Host → Manage → Licensing
2. Assign license → Ingresar clave
3. Save

### Verificar Servicios

Servicios habilitados por defecto:
- ✅ SSH (si se necesita acceso por terminal)
- ✅ ESXi Shell
- ✅ HTTPS (Host Client)
- ✅ NTP Client (opcional pero recomendado)

### Configuraciones Adicionales Recomendadas

#### Habilitar SSH (Opcional)
1. Host → Actions → Services → Enable Secure Shell (SSH)
2. ⚠️ Deshabilitar cuando no se use

#### Configurar NTP
1. Host → Manage → System → Time & date
2. Edit NTP settings
3. Agregar servidores NTP

#### Crear Datastore
1. Storage → New datastore
2. Seleccionar disco disponible
3. Formatear como VMFS6

---

## ✅ Verificación

### Checklist Post-Instalación

- [x] ESXi accesible vía web en `https://192.168.30.10`
- [x] Licencia Enterprise Plus activada
- [x] Red configurada en VLAN 30
- [x] Hostname configurado: `ESXI`
- [x] Datastore creado y disponible
- [x] Hardware reconocido correctamente
  - [x] CPU: 4 cores @ 9.6 GHz
  - [x] RAM: 7.99 GB
  - [x] Storage: 2.72 TB
  - [x] NIC: Gigabit Ethernet

### Comandos de Verificación (SSH)
```bash
# Ver versión de ESXi
vmware -vl

# Ver hardware
esxcli hardware platform get

# Ver adaptadores de red
esxcli network nic list

# Ver datastores
esxcli storage filesystem list
```

---

## 💡 Lecciones Aprendidas

### Ventajas de Usar Imagen HPE Customizada
- ✅ **Compatibilidad perfecta** con hardware HP
- ✅ **Drivers incluidos** - no requirió instalación adicional
- ✅ **Proceso sin errores** - reconoció todo el hardware
- ✅ **Soporte oficial** de HPE para esta combinación

### Recomendaciones

1. **Usar siempre imagen customizada del fabricante**
   - Evita problemas de compatibilidad
   - Drivers optimizados para el hardware

2. **Verificar virtualización en BIOS**
   - Intel VT-x debe estar habilitado
   - Sin esto, ESXi no funcionará correctamente

3. **Documentar configuraciones**
   - IP, VLAN, hostname
   - Facilita troubleshooting futuro

4. **Backup de configuración**
   - Exportar configuración desde Host Client
   - Guardar clave de licencia

5. **RAID no siempre es necesario**
   - Para homelabs pequeños, un solo disco es suficiente
   - RAID se puede configurar después si se necesita

### Troubleshooting Común

**Problema**: ESXi no detecta adaptador de red
- **Solución**: Usar imagen customizada de HPE

**Problema**: No se puede acceder vía web
- **Solución**: Verificar VLAN, firewall de red local

**Problema**: Error "CPU not supported"
- **Solución**: Habilitar VT-x en BIOS

---

## 📚 Referencias

- [VMware ESXi Documentation](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.install.doc/GUID-B2F01BF5-078A-4C7E-B505-5DFFED0B8C38.html)
- [HPE ESXi Custom Images](https://vibsdepot.hpe.com/)
- [Rufus - Create bootable USB](https://rufus.ie/)

---

## 📧 Notas

- Esta instalación fue realizada en un entorno de laboratorio doméstico (HomeLab)
- El objetivo es aprendizaje y desarrollo de habilidades en virtualización
- Configuración standalone (sin vCenter Server)

---

**Fecha de Instalación**: 16 de septiembre de 2025  
**Documentado por**: Felipe Loaiza Rodriguez  
**Última actualización**: Enero 2026
