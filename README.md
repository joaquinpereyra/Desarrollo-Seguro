
# Implementación y Configuración de un Ambiente de Prueba de Aplicaciones

En este documento se describe el proceso de implementación y configuración de un ambiente destinado a la prueba de aplicaciones utilizando la distribución Kali Linux en una máquina virtual, así como la instalación de herramientas como ZAP (OWASP Zed Attack Proxy) y Docker para realizar pruebas de seguridad y desarrollo.

## Instalación de una Máquina Virtual Kali Linux

**Sistemas:**
- MacBook Pro - macOS Monterey 12.5.1 - M2
- MacBook Pro - macOS Ventura 13.4.1 - Intel Core i7 de cuatro núcleos

### Instalación Kali Linux con UTM

1. Descargar la imagen ISO de Kali Linux desde [este enlace](https://www.kali.org/get-kali/#kali-platforms).
2. Instalar UTM, un emulador de máquinas virtuales, desde [mac.getutm.app](https://mac.getutm.app/).
3. Crear una nueva máquina virtual en UTM con los siguientes pasos:
   - Seleccionar "Virtualizar".
     ![virtualizar.png](..%2F..%2F..%2F..%2FDownloads%2Fvirtualizar.png)
   - Elegir "Linux" como sistema operativo.
   ![linux.png](..%2F..%2F..%2F..%2FDownloads%2Flinux.png)
   - Seleccionar el archivo ISO de Kali Linux previamente descargado.
   - Configurar la cantidad de RAM y CPU deseada.
   - Configurar el almacenamiento deseado.
   - Añadir un emulador de dispositivo serie.
   - Configurar redes de la VM agregando una nueva red en modo "solo host".

4. Realizar la instalación de Kali Linux dentro de la VM:
   - Utilizar la terminal para iniciar la instalación.
   - Seleccionar "Install".
   - Configurar idioma, ubicación y teclado.
   - Elegir una red configurada (p. ej., eth1: Ethernet).
   - Si aparece un error de configuración de red, seleccionar "Do not configure the network at this time".
   - Configurar el usuario, hostname y contraseña.
   - Seleccionar "Guided - use entire disk" en la partición de disco.
   - Seleccionar el disco y opciones de particionamiento.
   - Seleccionar "Finish partitioning and write changes to disk".
   - Continuar con la instalación usando las opciones por defecto.
   - Apagar la VM después de completar la instalación y eliminar el ISO de los discos.

5. Encender la VM y acceder a Kali Linux usando el usuario y contraseña previamente configurados.

## Instalación con VirtualBox

1. Descargar e instalar VirtualBox desde [virtualbox.org](https://www.virtualbox.org/).
2. Descargar la imagen ISO de Kali Linux desde [este enlace](https://www.kali.org/get-kali/#kali-platforms).
3. Ejecutar VirtualBox y crear una nueva máquina virtual, seleccionando el archivo ISO de Kali Linux.
4. Iniciar la máquina virtual desde VirtualBox.

## Instalación de Herramientas

### Instalación de ZAP (OWASP Zed Attack Proxy)

1. Instalar ZAP en Kali Linux:
   ```bash
   sudo apt update
   sudo apt install zaproxy
   ```

2. Verificar la instalación:
   ```bash
   owasp-zap -h
   ```

### Instalación de Docker

1. Instalar Docker en Kali Linux:
   ```bash
   sudo apt install -y docker.io
   sudo systemctl enable docker --now
   sudo usermod -aG docker $USER
   ```

2. Verificar la instalación:
   ```bash
   docker
   ```

3. Ejecución de un contenedor con OWASP Juice Shop:
   - Descargar imagen de Juice Shop 
      ```bash
      sudo docker pull bkimminich/juice-shop
      ```
   - Ejecutar Docker con la imagen de Juice Shop
      ```bash
      sudo docker run -d -p 3000:3000 bkimminich/juice-shop
      ```

## Prueba de Visualización del Tráfico en el Proxy

### Configuración de Certificado SSL en ZAP

1. En ZAP, ir a "Tools" -> "Options" -> "Dynamic SSL Certificates" y crear un nuevo certificado. Guardarlo en una carpeta.
2. Agregar el certificado al navegador Firefox:
   - Ir a "Options" -> "Privacy & Security" -> "Certificates" -> "View Certificates" -> "Import".
   - Agregar el certificado previamente guardado.

### Configuración del Proxy en ZAP y Firefox

1. En ZAP, ir a "Tools" -> "Options" -> "Local Proxies" -> "Go To new Screen":
   - Configurar dirección: localhost
   - Configurar puerto: 8081

2. Configurar el proxy en Firefox:
   - Ir a "Options" -> "Network Settings" -> "Settings".
   - Seleccionar "Manual proxy configuration".
   - Configurar HTTP proxy: localhost, Puerto: 8081.

3. Navegar por internet y observar el tráfico en ZAP.

## Conclusiones

Este documento ha detallado el proceso de implementación y configuración de un ambiente destinado a la prueba de aplicaciones utilizando Kali Linux en una máquina virtual. Se han instalado herramientas como ZAP y Docker para llevar a cabo pruebas de seguridad y desarrollo de aplicaciones.
