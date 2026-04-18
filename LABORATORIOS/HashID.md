### Manual de Usuario: **HashID**

#### Introducción
**HashID** es una herramienta de línea de comandos diseñada para identificar algoritmos de hash. Es útil para profesionales en seguridad informática, análisis forense digital y administración de sistemas, ya que permite determinar qué tipo de hash se está utilizando en una cadena específica.

---

#### Requisitos previos
Antes de usar **HashID**, es necesario tener instalado lo siguiente:
1. **Python** (versión 2.7 o superior, preferiblemente Python 3).
2. Acceso a un terminal o línea de comandos.

#### Instalación
1. **Clonación del repositorio desde GitHub**  
   Abra una terminal y ejecute:
   ```bash
   git clone https://github.com/psypanda/hashID.git
   ```
   Esto descargará los archivos de **HashID** en su sistema.

2. **Acceder al directorio del proyecto**  
   Cambie al directorio donde se descargó **HashID**:
   ```bash
   cd hashID
   ```

3. **Dar permisos de ejecución** (opcional):  
   Para facilitar la ejecución del script, otorgue permisos con el siguiente comando:
   ```bash
   chmod +x hashid.py
   ```

#### Uso básico
Para usar **HashID**, navegue al directorio donde se encuentra el archivo `hashID.py` y ejecute el script desde la terminal.

##### Identificación de un hash
1. Ejecute el comando:
   ```bash
   python3 hashid.py -m [HASH]
   ```
   - Reemplace `[HASH]` con el valor del hash que desea identificar.

2. Ejemplo:
   ```bash
   python3 hashid.py -m 5d41402abc4b2a76b9719d911017c592
   ```
   Salida esperada:
   ```
   Possible Hashes:
   [+] MD5
   ```

##### Optener ayuda
Para optener ayuda de **HashID**, ejecute:
```bash
python3 hashid.py -h
```

##### Escaneo desde un archivo
**HashID** puede guardar su sálida en un archivo de texto.
1. Ejemplo para guardar el resultardo en un fichero llamaddo tres.txt, ejecute:
   ```bash
   jaider@DESKTOP-8QVES9R:~/hashID$ python3 hashid.py -m -o tres.txt 5d41402abc4b2a76b9719d911017c592
   ```

##### Opciones avanzadas
- **Forzar coincidencias parciales**:
   ```bash
   python3 hashID.py -m [HASH] --force
   ```
   Esto incluirá algoritmos de hash que podrían coincidir parcialmente.
   
- **Salida en colores**:  
   Para resaltar información en colores (si el terminal lo permite), use:
   ```bash
   python3 hashID.py -m [HASH] --color
   ```

#### Solución de problemas
- **Error: "Permission denied"**  
   Asegúrese de tener permisos de ejecución en el script:
   ```bash
   chmod +x hashID.py
   ```

- **Python no está instalado**  
   Instale Python según el sistema operativo:
   - En **Ubuntu**:
     ```bash
     sudo apt-get install python3
     ```
   - En **Windows**: Descargue desde [python.org](https://www.python.org/).

- **Dependencias faltantes**  
   Verifique que tiene instalado el módulo `argparse`. Si no, instálelo:
   ```bash
   pip install argparse
   ```

#### Conclusión
**HashID** es una herramienta potente y sencilla para identificar algoritmos de hash. Su facilidad de uso y soporte para una amplia gama de algoritmos la hacen indispensable para tareas de análisis y seguridad informática.
