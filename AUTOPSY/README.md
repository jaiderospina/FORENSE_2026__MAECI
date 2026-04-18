# Autopsy

![](/images/autopsy.png)

**Autopsy** es una de las herramientas de análisis forense digital más utilizadas en el mundo. Es una interfaz gráfica basada en el motor de búsqueda **The Sleuth Kit**, que permite a los investigadores analizar discos duros, smartphones y tarjetas de memoria para recuperar archivos borrados, examinar el historial de navegación y encontrar evidencias digitales.

Es una herramienta de código abierto y es fundamental en investigaciones policiales, militares y corporativas debido a su facilidad de uso y potencia.



---

### Manual de Instalación Multiplataforma

A continuación, se detalla el proceso para instalar Autopsy en Windows, Linux y macOS.

#### 1. Windows (Soporte Nativo)
Es la plataforma principal para Autopsy y la más sencilla de instalar.

1.  Ir a la página oficial: [https://www.autopsy.com/download/](https://www.autopsy.com/download/).
2.  Descargar el instalador de 64 bits (`.msi`).
3.  Ejecutar el archivo descargado.
4.  Seguir el asistente de instalación (Next -> Install -> Finish).
5.  **Nota:** No requiere configuración previa de Java, ya que el instalador de Windows incluye su propio entorno de ejecución.

---

#### 2. Linux (Debian/Ubuntu)
En Linux, Autopsy requiere algunos pasos adicionales ya que se debe configurar Java y The Sleuth Kit manualmente.

**Requisitos previos:**
* Java 17 (u 8, dependiendo de la versión).
* The Sleuth Kit (TSK).

**Pasos:**
1.  **Actualizar el sistema:**
    ```bash
    sudo apt update && sudo apt upgrade
    ```
2.  **Instala las dependencias y Java:**
    ```bash
    sudo apt install openjdk-17-jre openjdk-17-jdk libgstreamer1.0-0 testdisk
    ```
3.  **Descarga Autopsy para Linux:**
    En el sitio oficial, descarga el archivo `.zip` (por ejemplo, `autopsy-4.xx.x.zip`).
4.  **Descarga e instala The Sleuth Kit:**
    Descarga el archivo `.deb` de TSK desde el repositorio de Autopsy en GitHub y ejecutarlo:
    ```bash
    sudo dpkg -i sleuthkit-java_x.x.x-1_amd64.deb
    ```
5.  **Configurar Autopsy:**
    Descomprimir el zip de Autopsy, entrar en la carpeta y ejecutar el script de configuración:
    ```bash
    unzip autopsy-4.xx.x.zip
    cd autopsy-4.xx.x
    chmod +x unix_setup.sh
    ./unix_setup.sh
    ```
6.  **Ejecutar:**
    ```bash
    bin/autopsy
    ```

---

#### 3. macOS
La instalación en macOS es similar a la de Linux pero requiere el uso de **Homebrew**.

1.  **Instalar dependencias:**
    Abrir la terminal e instala Java y Sleuth Kit:
    ```bash
    brew install openjdk@17
    brew install sleuthkit
    ```
2.  **Descargar Autopsy:**
    Descargar el archivo `.zip` para Linux/Mac desde la web oficial.
3.  **Configurar variables de entorno:**
    Asegurar que Java esté en el PATH.
4.  **Ejecutar el script de configuración:**
    Al igual que en Linux, descomprimir el archivo, entrar en la carpeta por terminal y ejecutar:
    ```bash
    sh unix_setup.sh
    ```
5.  **Iniciar:**
    ```bash
    ./bin/autopsy
    ```

---

### Primeros pasos tras la instalación
Una vez abierto Autopsy, el flujo de trabajo básico es:

1.  **Create New Case:** Definir el nombre del caso y la ruta donde se guardarán los datos.
2.  **Add Data Source:** Seleccionar el disco, imagen forense (E01, RAW) o carpeta que deseas analizar.
3.  **Configure Ingest Modules:** Eligir qué se desea que Autopsy busque (palabras clave, correos, archivos multimedia, etc.).
4.  **Análisis:** Una vez terminado el proceso, explorar los resultados en el árbol de navegación a la izquierda.

---

Referencias:

- https://www.autopsy.com/
- https://github.com/sleuthkit/autopsy/releases/
- https://github.com/sleuthkit/autopsy/blob/develop/Running_Linux_OSX.md
- https://knowledge.iadb.org/es/conocimiento-abierto/codigo-para-el-desarrollo/solucion-de-codigo-abierto/autopsy

---

## Actividad en clase.

## Instalar la última versión de Autopsy. 
