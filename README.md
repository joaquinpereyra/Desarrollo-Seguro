![vb start](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/e85beb60-3a1d-44cc-b167-4f07f932a585)![hardware](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/96926121-ece8-4146-b0d6-3a261c91a004)
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
   ![virtualizar](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/a452a573-1ad8-4b14-851e-dc53cc502227)
   - Elegir "Linux" como sistema operativo.
   ![linux](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/6243754a-8379-42d8-9e5b-c1a021c1980a)
   - Seleccionar el archivo ISO de Kali Linux previamente descargado.
   - Configurar la cantidad de RAM y CPU deseada.
   ![hardware](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/c1138a21-0202-47b3-8535-21223b761723)
   - Configurar el almacenamiento deseado.
   ![almacenamiento](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/818c76e9-ab6e-4ac4-8773-38e27454442b)
   - Guardar cambios.
   ![Resumen](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/2b636d88-beb3-40c0-85a7-27ccc077bf91)
   - Añadir un emulador de dispositivo serie.
   ![Serial](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/176b0073-df2e-4c62-943c-371c730facc3)
   - Configurar redes de la VM agregando una nueva red en modo "solo host".
   ![Red](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/5d230748-01d0-43f2-9881-22944b63477d)


4. Realizar la instalación de Kali Linux dentro de la VM:
   - Utilizar la terminal para iniciar la instalación.
     ![kali Terminal](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/67add05e-6e38-415c-a989-4043c68a4587)
   - Seleccionar "Install".
   - Configurar idioma, ubicación y teclado.
   - Elegir una red configurada (p. ej., eth1: Ethernet).
   - Si aparece un error de configuración de red, seleccionar "Do not configure the network at this time".
   ![Error notwork](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/660eff6b-764f-4743-91cc-4a1fc2e88efe)
   ![not this time](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/5fd5c2c5-86ea-44de-80df-73595d9f37f2)
   - Configurar el usuario, hostname y contraseña.
   - Seleccionar "Guided - use entire disk" en la partición de disco.
   - Seleccionar el disco y opciones de particionamiento.
   - Seleccionar "Finish partitioning and write changes to disk".
   - Continuar con la instalación usando las opciones por defecto.
   - Apagar la VM después de completar la instalación y eliminar el serial emulado y el ISO de los discos.
   ![last image utm](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/3e0bb2a2-5f0e-4485-9469-af1de119fa49)


5. Encender la VM y acceder a Kali Linux usando el usuario y contraseña previamente configurados.

## Instalación con VirtualBox

1. Descargar e instalar VirtualBox desde [virtualbox.org](https://www.virtualbox.org/).
2. Descargar la imagen ISO de Kali Linux desde [este enlace](https://www.kali.org/get-kali/#kali-platforms).
3. Ejecutar VirtualBox y crear una nueva máquina virtual, seleccionando el archivo ISO de Kali Linux.
![vb start](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/32850afa-79d0-4eb6-8bf0-f585dfdb0822)
5. Iniciar la máquina virtual desde VirtualBox.
![vb last](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/fa868874-ca9b-4293-99b7-7f12719bced0)

## Instalación de Herramientas

### Instalación de ZAP (OWASP Zed Attack Proxy)

1. Instalar ZAP en Kali Linux:
   ```bash
   sudo apt install zaproxy
   ```

2. Verificar la instalación:
   ```bash
   owasp-zap -h
   ```
#### Errores
Si aparece el sigueinte error
![Error zap](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/c8743160-ea7f-4343-97d8-671148f52949)

Ejecutar los siguientes comandos:
```bash
   sudo apt update
   sudo apt upgrade
   sudo apt install zaproxy
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
![proxy](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/a7ea2938-8144-4ca5-8855-ffcfa999d8e8)

3. Navegar por internet y observar el tráfico en ZAP.
![trafic zap](https://github.com/joaquinpereyra/Desarrollo-Seguro/assets/42189479/2159fba5-a45e-4b19-a74f-7269b45b9318)


## Conclusiones

Este documento ha detallado el proceso de implementación y configuración de un ambiente destinado a la prueba de aplicaciones utilizando Kali Linux en una máquina virtual. Se han instalado herramientas como ZAP y Docker para llevar a cabo pruebas de seguridad y desarrollo de aplicaciones.
