# Manual de Instalación de VirtualBox

## 1. ¿Qué es VirtualBox?

VirtualBox es una herramienta de virtualización que permite instalar y ejecutar sistemas operativos en máquinas virtuales (VMs). Con VirtualBox, puedes usar múltiples sistemas operativos en tu computadora sin tener que instalar cada uno de ellos físicamente en el disco duro.

## 2. Requisitos Previos

- Procesador compatible con virtualización (Intel VT-x o AMD-V).
- Al menos 8 GB de RAM.
- Espacio en disco para almacenar máquinas virtuales.

---

## 3. Instalación en Windows

### Paso 1: Descargar VirtualBox

1. Abre tu navegador y dirígete a la [página oficial de descargas de VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Haz clic en el enlace que dice "Windows hosts" para descargar el archivo de instalación para Windows.

### Paso 2: Ejecutar el Instalador

1. Localiza el archivo descargado (por lo general, estará en la carpeta de "Descargas") y haz doble clic para iniciar el instalador.
2. Aparecerá una ventana de bienvenida; haz clic en **"Next"** (Siguiente).

### Paso 3: Seleccionar Componentes

1. En la ventana de componentes, puedes elegir qué componentes instalar. Se recomienda dejar las opciones predeterminadas.
2. Haz clic en **"Next"**.

### Paso 4: Opciones de Instalación

1. Aquí puedes configurar accesos directos adicionales (como en el escritorio o en el menú de inicio).
2. Haz clic en **"Next"** y después en **"Yes"** si aparece una advertencia sobre conexión de red.

### Paso 5: Comenzar la Instalación

1. Haz clic en **"Install"** para iniciar el proceso de instalación.
2. Durante la instalación, Windows podría mostrar un mensaje de seguridad. Haz clic en **"Install"** para confirmar.

### Paso 6: Completar la Instalación

1. Una vez completada la instalación, marca la casilla **"Start Oracle VM VirtualBox"** si deseas abrir VirtualBox inmediatamente.
2. Haz clic en **"Finish"** (Finalizar) para salir del instalador.

---

## 4. Instalación en Linux (Debian, Ubuntu y derivados)

### Paso 1: Actualizar el Sistema

Antes de comenzar, asegúrate de que el sistema esté actualizado. Abre la terminal y ejecuta:

```bash
sudo apt update && sudo apt upgrade -y
```

### Paso 2: Añadir el Repositorio de VirtualBox

1. Añade la clave pública de Oracle para VirtualBox:

   ```bash
   wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
   ```

2. Añade el repositorio de VirtualBox a tu lista de fuentes. Si usas Ubuntu:

   ```bash
   echo "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee -a /etc/apt/sources.list.d/virtualbox.list
   ```

### Paso 3: Instalar VirtualBox

1. Actualiza de nuevo la lista de paquetes para incluir el nuevo repositorio:

   ```bash
   sudo apt update
   ```

2. Instala la última versión de VirtualBox:

   ```bash
   sudo apt install virtualbox -y
   ```

### Paso 4: Verificar la Instalación

Para comprobar que VirtualBox se instaló correctamente, ejecuta:

```bash
virtualbox
```

Esto debería abrir la interfaz gráfica de VirtualBox.

---

## 5. Instalación de Extensiones de VirtualBox (Opcional)

Oracle proporciona un paquete de extensiones que añade características adicionales a VirtualBox, como el soporte para USB 2.0/3.0 y el cifrado de disco.

1. Dirígete a la [página de descargas de VirtualBox](https://www.virtualbox.org/wiki/Downloads) y descarga el "Extension Pack" correspondiente a tu versión de VirtualBox.
2. Abre VirtualBox.
3. Ve a **"File"** > **"Preferences"** > **"Extensions"**.
4. Haz clic en el icono de añadir y selecciona el archivo descargado.

---

## 6. Configuración Básica de una Máquina Virtual

1. En la ventana principal de VirtualBox, haz clic en **"New"** para crear una nueva máquina virtual.
2. Asigna un nombre, selecciona el tipo de sistema operativo y la versión.
3. Configura la memoria RAM que deseas asignar.
4. Crea un disco duro virtual si es necesario y sigue los pasos de la configuración.

---

## 7. Resolución de Problemas Comunes

- **Error de virtualización**: Asegúrate de que la virtualización esté habilitada en la BIOS o UEFI de tu sistema.
- **Errores de red**: Revisa las configuraciones de red de la VM, especialmente si usas modos NAT o Bridge.

---
