📖 Instalación de ESXi en HP ProLiant ML110 G6
Guía completa de instalación de VMware ESXi 6.5.0 en servidor HP ProLiant ML110 G6 utilizando imagen customizada de HPE.

📋 Tabla de Contenidos

Requisitos Previos
Descarga de ESXi
Preparación del Medio de Instalación
Configuración de BIOS
Proceso de Instalación
Configuración Inicial
Post-Instalación
Verificación
Lecciones Aprendidas


🔧 Requisitos Previos
Hardware Mínimo

✅ Procesador compatible con virtualización (Intel VT-x o AMD-V)
✅ Mínimo 4GB RAM (Recomendado: 8GB+)
✅ Adaptador de red Gigabit Ethernet
✅ Disco duro con mínimo 8GB de espacio

Hardware Utilizado
ComponenteEspecificaciónServidorHP ProLiant ML110 G6CPUIntel Xeon X3430 @ 2.40GHz (4 cores)RAM8 GB DDR3Storage2.72 TBRedGigabit Ethernet
Software Necesario

USB booteable (mínimo 8GB)
Rufus (para crear USB booteable)
Imagen ISO de ESXi


💿 Descarga de ESXi
Versión Instalada

Producto: VMware ESXi
Versión: 6.5.0 Update 3 (Build 5310538)
Imagen: HPE Customized Image ESXi 6.5.0
Profile: HPE-ESXi-6.5.0-iso-650.10.1.5.20

Fuente de Descarga
La imagen customizada de HPE se descarga desde:

Portal de HPE Support
VMware Customer Connect (con cuenta)

Ventajas de la imagen HPE:

✅ Drivers optimizados para hardware HP
✅ Mayor compatibilidad con controladores
✅ Soporte oficial de HPE


🔨 Preparación del Medio de Instalación
Crear USB Booteable con Rufus

Descargar Rufus

Sitio oficial: https://rufus.ie
Versión recomendada: 3.x o superior


Configuración en Rufus

   Dispositivo: [Tu USB de 8GB+]
   Tipo de arranque: Imagen ISO
   Seleccionar: [ESXi ISO descargado]
   Esquema de partición: MBR
   Sistema destino: BIOS o UEFI
   Sistema de archivos: FAT32

Crear USB

Clic en "EMPEZAR"
Esperar a que finalice (5-10 minutos)
Verificar que se creó correctamente




⚙️ Configuración de BIOS
Acceso a BIOS

Encender servidor HP ProLiant ML110 G6
Presionar F9 durante el POST
Navegar con las flechas del teclado

Configuraciones Realizadas
1. Boot Order (Orden de Arranque)
Antes:
1. Hard Drive
2. USB Device
3. Network Boot
Después:
1. USB Device    ← Modificado
2. Hard Drive
3. Network Boot
2. Virtualización
Verificar que esté habilitada:

Intel Virtualization Technology (VT-x): [Enabled]
Intel VT-d: [Enabled] (si está disponible)

3. Otras Configuraciones

SATA Mode: AHCI (recomendado)
RAID: No configurado (no fue necesario)
USB Support: Enabled

Guardar y Reiniciar

Presionar F10 para guardar
Confirmar cambios
Servidor reinicia automáticamente


🚀 Proceso de Instalación
Paso 1: Boot desde USB

Insertar USB booteable
Servidor inicia desde USB automáticamente
Aparece pantalla de carga de ESXi

Loading ESXi installer...
Paso 2: Pantalla de Bienvenida

Aparece: "Welcome to the VMware ESXi 6.5.0 Installation"
Presionar Enter para continuar

Paso 3: Aceptar EULA

Leer End User License Agreement
Presionar F11 para aceptar

Paso 4: Selección de Disco

ESXi detecta discos disponibles
Seleccionar disco para instalación (2.72 TB en este caso)
Presionar Enter

⚠️ Advertencia: Todos los datos del disco seleccionado serán eliminados
Paso 5: Layout de Teclado

Seleccionar: Spanish o US Standard
Presionar Enter

Paso 6: Contraseña de Root

Ingresar contraseña de administrador (mínimo 7 caracteres)
Confirmar contraseña
Presionar Enter

🔐 Nota de Seguridad: Por razones de seguridad, la contraseña no se documenta aquí
Paso 7: Confirmación

Revisar resumen de instalación
Presionar F11 para iniciar instalación

Paso 8: Instalación
Installing ESXi...
[████████████████████] 100%
Tiempo aproximado: 5-10 minutos
Paso 9: Finalización

Mensaje: "Installation Complete"
Remover USB
Presionar Enter para reiniciar


🔧 Configuración Inicial
DCUI (Direct Console User Interface)
Después del reinicio, aparece la consola de ESXi:
VMware ESXi 6.5.0 (VMware, Inc.)

192.168.30.10
Configuración de Red

Presionar F2 para personalizar sistema
Login: root
Password: [tu contraseña]

Configure Management Network
Parámetros configurados:
ConfiguraciónValorVLAN ID30IPv4192.168.30.10Subnet Mask255.255.255.0Gateway192.168.30.1DNS[DNS de tu red]HostnameESXI
Pasos:

Configure Management Network → IPv4 Configuration
Set static IPv4 address
Ingresar IP: 192.168.30.10
Ingresar Subnet Mask
Ingresar Default Gateway
VLAN (optional) → VLAN ID: 30
DNS Configuration → Hostname: ESXI
Esc para salir
Confirmar cambios → Y

Reiniciar Management Network
Restart Management Network? [Y/N]: Y
La configuración de red se aplica y el servidor queda accesible en: https://192.168.30.10

🌐 Post-Instalación
Acceso Web (VMware Host Client)

Abrir navegador web

URL: https://192.168.30.10
Aceptar certificado autofirmado


Login

Usuario: root
Password: [tu contraseña]


Interface Web

✅ No requiere instalación de vSphere Client
✅ Acceso completo desde navegador
✅ Gestión de VMs, storage, networking



Configuración de Licencia
Licencia instalada:

Producto: VMware vSphere with Operations Management 6 Enterprise Plus
Características:

Unlimited number of VMs
vMotion, Storage vMotion
High Availability (HA)
Distributed Resource Scheduler (DRS)



Activar licencia:

Host → Manage → Licensing
Assign license → Ingresar clave
Save

Verificar Servicios
Servicios habilitados por defecto:

✅ SSH (si se necesita acceso por terminal)
✅ ESXi Shell
✅ HTTPS (Host Client)
✅ NTP Client (opcional pero recomendado)

Configuraciones Adicionales Recomendadas
Habilitar SSH (Opcional)

Host → Actions → Services → Enable Secure Shell (SSH)
⚠️ Deshabilitar cuando no se use

Configurar NTP

Host → Manage → System → Time & date
Edit NTP settings
Agregar servidores NTP

Crear Datastore

Storage → New datastore
Seleccionar disco disponible
Formatear como VMFS6


✅ Verificación
Checklist Post-Instalación

 ESXi accesible vía web en https://192.168.30.10
 Licencia Enterprise Plus activada
 Red configurada en VLAN 30
 Hostname configurado: ESXI
 Datastore creado y disponible
 Hardware reconocido correctamente

 CPU: 4 cores @ 9.6 GHz
 RAM: 7.99 GB
 Storage: 2.72 TB
 NIC: Gigabit Ethernet



Comandos de Verificación (SSH)
bash# Ver versión de ESXi
vmware -vl

# Ver hardware
esxcli hardware platform get

# Ver adaptadores de red
esxcli network nic list

# Ver datastores
esxcli storage filesystem list

💡 Lecciones Aprendidas
Ventajas de Usar Imagen HPE Customizada

✅ Compatibilidad perfecta con hardware HP
✅ Drivers incluidos - no requirió instalación adicional
✅ Proceso sin errores - reconoció todo el hardware
✅ Soporte oficial de HPE para esta combinación

Recomendaciones

Usar siempre imagen customizada del fabricante

Evita problemas de compatibilidad
Drivers optimizados para el hardware


Verificar virtualización en BIOS

Intel VT-x debe estar habilitado
Sin esto, ESXi no funcionará correctamente


Documentar configuraciones

IP, VLAN, hostname
Facilita troubleshooting futuro


Backup de configuración

Exportar configuración desde Host Client
Guardar clave de licencia


RAID no siempre es necesario

Para homelabs pequeños, un solo disco es suficiente
RAID se puede configurar después si se necesita



Troubleshooting Común
Problema: ESXi no detecta adaptador de red

Solución: Usar imagen customizada de HPE

Problema: No se puede acceder vía web

Solución: Verificar VLAN, firewall de red local

Problema: Error "CPU not supported"

Solución: Habilitar VT-x en BIOS


📚 Referencias

VMware ESXi Documentation
HPE ESXi Custom Images
Rufus - Create bootable USB


📧 Notas

Esta instalación fue realizada en un entorno de laboratorio doméstico (HomeLab)
El objetivo es aprendizaje y desarrollo de habilidades en virtualización
Configuración standalone (sin vCenter Server)


Fecha de Instalación: 16 de septiembre de 2025
Documentado por: Felipe Loaiza Rodriguez
Última actualización: Enero 2026
