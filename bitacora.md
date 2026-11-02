# BITÁCORA

## Actividad 1: La lista de als siete carencias

## Punto de partida

1. Se conecta con el usuario "postgres" y tiene contraseña, aunque puede configurarse para no necesitarla. En una situación
real, esto puede dejar al servidor vulnerable a ciberataques al estar completamente abierto.

2. Es necesario arrancar Apache y MySQL en XAMPP. En una situación real, es muy inconveniente tener que arrancar el servidor manualmente cada vezz que lo reiniciemos.

3. Los errores se registran en el "error.log" de Apache, no en la consola. Aunque esto no afecte a los usuarios directamente,
no es práctico cuando tienes que trabajar con varias personas.

4. Ambos tenemos PHP 8.2.12 y Node v24.15.0 (aunque podría ser una versión distinta). En una situación real, el programa podría funcional mar o ser inutilizable para un usuario con versiones diferentes.

## Actividad 2: Las dos máquinas del grupo

## 1.- Preparación de la máquina
- Se desplegó una máquina virtual mínima en Proxmox.
- Se ejecutó la actualización completa del sistema operativo y se instalaron herramientas las esenciales.
- Se otorgaron permisos de administración al usuario principal. La contraseña de "root" se dejó vacía.
- Tras ciertas configuraciones específicas y de red, la máquina virtual se convirtió en una plantilla.

## 2.- Clonación de la plantilla
- Se generaron dos clones completos de la plantilla: **dev.grupo2** como entorno de desarrollo y **pro.grupo2** como entorno de producción.

## 3.- Configuración de red
- Se configuró la interfaz de red principal (ens18) de ambas máquinas virtuales editando el archivo "/etc/network/interfaces" para asignar direccionamiento estático dentro de la subred del aula.

**Comando ejecutado:**
  ```bash
  sudo nano /etc/network/interfaces
  ```

**dev.grupo2:**
```plaintext
allow-hotplug ens18
iface ens18 inet static
    address 192.168.1.49/24
    gateway 192.168.1.1
```

**pro.grupo2:**
```plaintext
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
- Se realizaron pruebas mediante el comando `ping` entre ambas máquinas y se verificó una comunicación exitosa.

## 5. Copia de seguridad
- Se generó una snapshot inicial en ambas máquinas desde la interfaz web de Proxmox, nombrada "recien-clonada".
