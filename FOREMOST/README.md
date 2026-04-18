### Manual de Recuperación de Datos de una USB usando Foresmont

Este manual explica el procedimiento detallado para recuperar datos de una unidad USB utilizando Foresmont, una herramienta de recuperación forense. Se cubren las etapas del análisis, los comandos necesarios y un caso práctico de recuperación de archivos eliminados.

#### Requisitos previos
1. **Instalar Foresmont**: Tener Foresmont instalado en el sistema. Foresmont es una herramienta disponible para sistemas operativos Linux, por lo que es necesario contar con un entorno Linux o una máquina virtual.

2. **Acceso root**: Para realizar el análisis y la recuperación, se requiere acceso root en el sistema.

3. **Unidad USB sin formatear**: Evitar formatear la USB o realizar cambios en su estructura para no sobrescribir los datos perdidos.

---

### Procedimiento General de Recuperación

#### 1. Conectar la USB y verificar su montaje
   Conecte la unidad USB al equipo y verifique su montaje con el siguiente comando:

   ```bash
   lsblk
   ```

   Este comando mostrará todas las unidades conectadas. Identifica la USB, que podría aparecer como `/dev/sdb1`, cómo se ve en este ejemplo.

imagen 1

#### 2. Crear una imagen de la USB (opcional pero recomendado)
   Crear una copia de seguridad o imagen de la unidad para trabajar con los datos de forma segura.

   ```bash
   dd if=/dev/sdb of=/ruta/donde/guardar/imagen_usb.img
   ```

   Donde:
   - `if=/dev/sdb` es la unidad de la USB.
   - `of=/ruta/donde/guardar/imagen_usb.img` es la ruta y nombre donde se guardará la imagen.

   *Nota: Trabajar sobre la imagen en lugar de la unidad real previene la pérdida de datos.*

#### 3. Iniciar Foresmont en la unidad USB
   Ejecuta Foresmont para analizar la unidad USB o la imagen creada:

   ```bash
   foresmont -a /dev/sdb
   ```

   Aquí, el comando `-a` realiza un análisis exhaustivo de la unidad.

#### 4. Analizar los resultados del escaneo
   Foresmont generará un reporte que detalla los archivos recuperables, áreas de la USB que contienen datos eliminados y archivos corruptos. Este reporte suele estar en formato `.txt` o `.html` en el directorio donde se ejecutó el comando.

#### 5. Recuperar los archivos deseados
   Para recuperar archivos específicos, usa el siguiente comando de Foresmont:

   ```bash
   foresmont -r /dev/sdb -o /ruta/donde/guardar/archivos
   ```

   Donde:
   - `-r` indica recuperación.
   - `-o` especifica el directorio de destino para guardar los archivos recuperados.

---

### Caso Práctico: Recuperación de un archivo de texto borrado

**Supongamos que se eliminó un archivo de texto importante llamado `informe.txt` y que necesitamos recuperarlo.**

1. **Escaneo de la unidad**:
   ```bash
   foresmont -a /dev/sdb
   ```

2. **Revisión del reporte**:
   Después del escaneo, revisa el reporte generado por Foresmont. Busca el nombre del archivo (`informe.txt`) y verifica su estado. Si el archivo está marcado como "recuperable", Foresmont tiene acceso a los sectores necesarios para restaurarlo.

3. **Recuperación del archivo**:
   ```bash
   foresmont -r /dev/sdb -f informe.txt -o /ruta/donde/guardar
   ```

   Donde `-f` permite especificar el nombre del archivo exacto a recuperar.

4. **Verificación de la recuperación**:
   Ve al directorio donde guardaste el archivo (`/ruta/donde/guardar`) y verifica la integridad del archivo `informe.txt`.

---

### Comandos Útiles y Opciones de Foresmont

- `foresmont -i /ruta/imagen.img`: Realiza un análisis en una imagen de disco en lugar de la unidad física.
- `foresmont -l`: Lista todos los archivos recuperables de la unidad o imagen.
- `foresmont -s`: Realiza un escaneo superficial, más rápido pero menos detallado.
- `foresmont -d /dev/sdb`: Borra de forma segura archivos específicos (esto es irreversible).

### Consideraciones Finales

- **Precaución con el uso de la USB**: No escribas ni formatees la unidad USB antes del análisis y recuperación.
- **Trabajar con imágenes**: Siempre que sea posible, realiza los análisis en una imagen para mantener la integridad de la USB.
- **Verificación de integridad**: Después de recuperar archivos importantes, abre los archivos para verificar que están completos y sin errores.

