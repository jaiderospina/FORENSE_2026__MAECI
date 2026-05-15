# Laboratorio: Esteganografía - Ocultando Archivos

**Asignatura:** Computación forense  
**Tema:** Esteganografía  
**Documento convertido desde:** `596677432-Lab-2-Esteganografia.pdf`

![Portada - laboratorio de computación forense](images/pagina_01_imagen_01.png)  
*Imagen 1. Portada del laboratorio. Fuente: PDF original, página 1, imagen extraída 1.*

---

## Lab Esteganografía - Ocultando Archivos

En este laboratorio se instalará el software que permite realizar acciones de esteganografía en archivos multimedia de imágenes. La herramienta que se utilizará es **FileFriend**.

## Notas de interés

### 1. ¿Qué es esteganografía?

La esteganografía, del griego *steganos* (“cubierto” u “oculto”) y *graphos* (“escritura”), trata del estudio y aplicación de técnicas que permiten ocultar mensajes u objetos dentro de otros, llamados **portadores**, de modo que no se perciba su existencia.

Es decir, procura ocultar mensajes dentro de otros objetos y establecer un canal encubierto de comunicación, de modo que el propio acto de comunicación pase inadvertido para observadores que tienen acceso a ese canal.

### 2. ¿A qué se conoce como archivo portador?

Es el archivo que transporta o camufla el contenido de otros elementos.

### 3. ¿A qué tipo de archivos u objetos se les puede realizar esteganografía?

Uno de los “contenedores” preferidos de los estenógrafos son los archivos multimedia, como imágenes, audios y videos, ya que suelen ser bastante grandes. Esto permite que el contenido adicional sea más viable que en un documento de texto.

---

## Manos a la obra: practicando en el laboratorio

### 1. Herramientas

- **PC Forense:** Windows 7 o superior.
- **Software:** <http://www.filefriend.net/>
- **Archivo digital:** formato `.jpg`; por ejemplo, `paseo_familiar1.jpg`.

> Asegúrese de verificar el tipo y tamaño del archivo original antes de comenzar.

---

## 2. Instrucciones

## Parte 1 - Ocultando archivos

### a. Descargar FileFriend

Proceda con la descarga de la herramienta **FileFriend** desde el enlace indicado en el apartado de herramientas.

![Página de descarga de FileFriend](images/pagina_03_imagen_02.png)  
*Figura 1. Descargando FileFriend. Fuente: PDF original, página 3, imagen extraída 2.*

No necesita realizar un proceso de instalación. Simplemente acceda al archivo `FileFriend.zip` y extraiga el contenido.

![Contenido del archivo ZIP de FileFriend](images/pagina_03_imagen_01.png)  
*Figura 2. Accediendo al contenido del archivo `.zip`. Fuente: PDF original, página 3, imagen extraída 1.*

### b. Ejecutar FileFriend

Una vez extraído el contenido, ejecute `FileFriend.exe`.

![Pantalla principal de FileFriend](images/pagina_04_imagen_02.png)  
*Figura 3. Pantalla principal de FileFriend. Fuente: PDF original, página 4, imagen extraída 2.*

### c. Seleccionar el archivo portador

En la herramienta se encuentra la opción **JPK**, en la cual se debe referenciar el archivo original que se va a utilizar como **portador** de los archivos que se desean ocultar.

![Selección del archivo portador en FileFriend](images/pagina_04_imagen_01.png)  
*Figura 4. Seleccionando el archivo portador. Fuente: PDF original, página 4, imagen extraída 1.*

Elementos destacados en la figura:

- Archivo portador: `paseo_familiar1.jpg`.
- Campo para personalizar la clave.
- Opción para ocultar archivos.

### d. Agregar archivos al portador

Continúe el laboratorio seleccionando un archivo de cualquier extensión o una carpeta con varios archivos, por ejemplo: `.docx`, `.pdf` u otros.

![Agregar archivos o carpeta al portador](images/pagina_05_imagen_03.png)  
*Figura 5. Agregando otros archivos al portador. Fuente: PDF original, página 5, imagen extraída 3.*

### e. Confirmar el proceso

Seleccione **OK** para agregar los archivos al archivo portador.

![Confirmación de éxito en FileFriend](images/pagina_05_imagen_01.png)  
*Figura 6. Confirmación de proceso. Fuente: PDF original, página 5, imagen extraída 1.*

### f. Verificar el cambio en el archivo portador

Vuelva a verificar el formato y tamaño de la imagen `paseo_familia1.jpg`. Se podrá observar que el archivo tiene ahora un tamaño mayor, en función de los archivos agregados.

![Verificación del tamaño del archivo portador](images/pagina_05_imagen_02.png)  
*Figura 7. Verificando el cambio en el archivo original portador. Fuente: PDF original, página 5, imagen extraída 2.*

---

## Parte 2 - Extrayendo los archivos ocultos

### g. Extraer archivos ocultos

Para realizar la extracción de los archivos agregados a la imagen original portadora, utilice la opción **View and extract files hidden in the JPEG file...**. Se presentará una pantalla con el detalle de los archivos agregados al archivo portador.

![Opción para ver y extraer archivos ocultos](images/pagina_06_imagen_02.png)  
*Imagen 2. Texto de la opción “View and extract files hidden in the JPEG file...”. Fuente: PDF original, página 6, imagen extraída 2.*

![Pantalla de extracción de archivos ocultos](images/pagina_06_imagen_01.png)  
*Figura 8. Verificando el cambio en el archivo original. Fuente: PDF original, página 6, imagen extraída 1.*

Seleccione la ruta destino donde se extraerán los archivos adicionados al archivo portador.

---
