# BITÁCORA

## Actividad 1: La lista de las siete carencias

## Punto de partida

1. **Usuario y contraseña de la base de datos**: Se conecta con el usuario "postgres" y tiene contraseña, aunque puede configurarse para no necesitarla.  
   *Consecuencia con usuarios reales*: En una situación real, esto puede dejar al servidor vulnerable a ciberataques al estar completamente abierto.

2. **Arranque de servicios**: Es necesario arrancar Apache y MySQL en XAMPP.  
   *Consecuencia con usuarios reales*: En una situación real, es muy inconveniente tener que arrancar el servidor manualmente cada vez que lo reiniciemos, ya que no arranca solo.

3. **Registro de errores**: Los errores se registran en el "error.log" de Apache, no en la consola interactiva.  
   *Consecuencia con usuarios reales*: Aunque esto no afecte a los usuarios directamente, no es práctico para trabajar en equipo y diagnosticar fallos persistentes de producción.

4. **Versiones del entorno**: Ambos tenemos PHP 8.2.12 y Node v24.15.0 (aunque podría ser una versión distinta).  
   *Consecuencia con usuarios reales*: En una situación real, el programa podría fallar o ser inutilizable para un usuario con versiones diferentes en su entorno ("en mi equipo funcionaba").

## Actividad 2: Las dos máquinas del grupo

## 1. Preparación de la máquina
- Se desplegó una máquina virtual mínima en Proxmox.
- Se ejecutó la actualización completa del sistema operativo y se instalaron las herramientas esenciales.
- Se otorgaron permisos de administración al usuario principal y se instaló `sudo`. La contraseña de "root" se dejó vacía.
- Tras las configuraciones iniciales, la máquina virtual se convirtió en una plantilla.

## 2. Clonación de la plantilla
- Se generaron dos clones completos de la plantilla: **dev.grupo2** como entorno de desarrollo y **pro.grupo2** como entorno de producción.

## 3. Configuración de red
- Se configuró la interfaz de red principal (`ens18`) de ambas máquinas virtuales editando el archivo `/etc/network/interfaces` para asignar direccionamiento estático dentro de la subred del aula.

**Comando ejecutado:**
```bash
sudo nano /etc/network/interfaces
```

**dev.grupo2:**
```text
allow-hotplug ens18
iface ens18 inet static
    address 192.168.1.49/24
    gateway 192.168.1.1
```

**pro.grupo2:**
```text
allow-hotplug ens18
iface ens18 inet static
    address 192.168.1.50/24
    gateway 192.168.1.1
```

## 4. Cambios y verificación
- Se aplicaron los cambios en la configuración de red ejecutando el comando:
```bash
sudo systemctl restart networking
```
- Se realizaron pruebas mediante el comando `ping` entre ambas máquinas y se verificó una comunicación exitosa (0% de pérdida).

## 5. Copia de seguridad
- Se generó una snapshot inicial en ambas máquinas desde la interfaz web de Proxmox, nombrada `recien-clonada`.
