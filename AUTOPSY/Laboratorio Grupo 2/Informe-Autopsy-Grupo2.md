
# Laboratorio: Caso de Estudio Forense
**Análisis de imagen de disquete con Autopsy**

|Curso|	Forense Digital|
|Caso|	Joe Jacobs / imagen de disquete|
|Herramienta principal|	Autopsy 4.23.0 en Windows Server|
|Archivo de evidencia|	image.zip|
|Hash MD5 de la evidencia|	b676147f63923e1f428131d59b1d6a72|
|Repositorio guía|	https://github.com/SVelizDonoso/forense-autopsy|
|Integrantes	|Diana Herran,Jesus Cuastumal, Tania Diaz Hernandez, Cesar Ferrer, Angelica Leon|
|Fecha|	05 08 2026|

| Herramienta | Función Principal | Estado |
| :--- | :---: | ---: |
| GitHub | Repositorio de código | ✅ Completado |
| Markdown | Documentación | 🏗️ En proceso |

Este informe documenta el procedimiento realizado, las evidencias observadas y las respuestas del caso práctico.
 
1. Introducción
El presente laboratorio consiste en el análisis forense de una imagen de disco flexible recuperada como evidencia. El análisis se realizó principalmente con Autopsy en Windows Server, complementando el proceso con PowerShell para verificar hashes, preparar la imagen, buscar cadenas de texto y reconstruir archivos comprimidos.
El caso busca determinar información relevante asociada a Joe Jacobs, incluyendo el proveedor identificado en un documento eliminado, una contraseña oculta, archivos enmascarados y otras escuelas presentes en una hoja de cálculo recuperada.
2. Objetivo
Analizar la imagen forense de un disquete recuperado, preservar la integridad de la evidencia y responder las preguntas del caso mediante la identificación, recuperación y examen de archivos eliminados u ocultos.
3. Evidencia y herramientas utilizadas
Elemento	Detalle
Archivo original	image.zip
Hash MD5 esperado	b676147f63923e1f428131d59b1d6a72
Imagen preparada	image.dd
Herramienta forense	Autopsy 4.23.0
Sistema de análisis	Windows Server
Herramientas complementarias	PowerShell, Explorador de Windows, Microsoft Excel

4. Metodología y evidencias
4.1 Preparación de carpetas y descarga
Se creó una estructura de trabajo en el escritorio del usuario Administrator. La carpeta principal fue Forense_Joe_Jacobs, con subcarpetas para capturas, evidencia, exportados y el caso de Autopsy.
C:\Users\Administrator\Desktop\Forense_Joe_Jacobs
├── capturas
├── evidencia
├── exportados
└── Caso_Autopsy
4.2 Verificación de integridad con hash MD5
Se calculó el hash MD5 de image.zip usando certutil. El valor obtenido coincidió con el hash entregado en la guía del laboratorio, por lo tanto se confirmó que la evidencia no había sido alterada.
certutil -hashfile image.zip MD5
 
Figura 1. Verificación del hash MD5 del archivo original image.zip.
Posteriormente se creó una copia de trabajo llamada copia_image.zip y se verificó que mantuviera el mismo hash MD5.
copy image.zip copia_image.zip
certutil -hashfile copia_image.zip MD5
 
Figura 2. Copia de trabajo creada y verificación del mismo hash MD5.
4.3 Extracción de la imagen forense
La copia de trabajo fue descomprimida y el archivo resultante se copió con extensión .dd para facilitar su carga como imagen de disco en Autopsy.
Expand-Archive .\copia_image.zip -DestinationPath .\evidencia
copy .\evidencia\image .\evidencia\image.dd
 
Figura 3. Extracción del archivo image y creación de image.dd.
4.4 Creación del caso en Autopsy
Se creó un nuevo caso en Autopsy llamado Caso_Joe_Jacobs, usando una carpeta específica para almacenar los datos del análisis.
 
Figura 4. Creación del caso Caso_Joe_Jacobs en Autopsy.
4.5 Carga de la imagen como fuente de datos
Se agregó la imagen de disco como fuente de datos usando la opción Disk Image or VM File.
 
Figura 5. Selección del tipo de fuente de datos: Disk Image or VM File.
 
Figura 6. Selección de la ruta de la imagen image.dd y zona horaria America/Bogota.
4.6 Archivos relevantes identificados
Una vez cargada la imagen, Autopsy mostró los archivos relevantes del caso: cover page.jpgc, Jimmy Jungle.doc y Scheduled Visits.exe. También se observó que Jimmy Jungle.doc aparecía con marca de archivo eliminado.
 
Figura 7. Archivos relevantes encontrados en la imagen del disquete.
4.7 Recuperación de Jimmy Jungle.doc
El archivo Jimmy Jungle.doc fue identificado como eliminado. En la pestaña Text de Autopsy se observó el contenido del documento, el cual contenía el nombre y la dirección del proveedor.
 
Figura 8. Contenido recuperado de Jimmy Jungle.doc con proveedor y dirección.
4.8 Identificación de la contraseña
Autopsy no mostró texto extraído directamente desde cover page.jpgc, por lo que se realizó una búsqueda de cadenas dentro de la imagen completa image.dd. Se encontró un mensaje que indicaba que el archivo de horarios debía abrirse con la misma contraseña enviada anteriormente, y además se encontró la cadena pw=goodtimes.
$path = "C:\Users\Administrator\Desktop\Forense_Joe_Jacobs\evidencia\image.dd"
$bytes = [System.IO.File]::ReadAllBytes($path)
$text = [System.Text.Encoding]::ASCII.GetString($bytes)

[regex]::Matches($text, "[ -~]{4,}") | Where-Object {
    $_.Value -match "goodtimes|gootimes|pw=|password"
}
 
Figura 9. Búsqueda de cadenas en image.dd: hallazgo de pw=goodtimes.
4.9 Análisis de Scheduled Visits.exe
El archivo Scheduled Visits.exe tenía extensión .exe, pero Autopsy lo reconoció como application/zip. Esto evidencia que el sospechoso lo enmascaró como ejecutable aunque realmente correspondía a un archivo ZIP.
 
Figura 10. Scheduled Visits.exe identificado como application/zip a pesar de tener extensión exe.
Al revisar la vista hexadecimal se observó la firma 50 4B 03 04, característica de un archivo ZIP. También se observó el nombre Scheduled Visits.xls dentro del contenido.
 
Figura 11. Firma ZIP 50 4B 03 04 y referencia a Scheduled Visits.xls en el contenido hexadecimal.
4.10 Exportación inicial y reconstrucción del ZIP
Al exportar Scheduled Visits.exe desde Autopsy y renombrarlo como ZIP, Windows indicó que el archivo comprimido era inválido. Esto mostró que el archivo exportado estaba incompleto, por lo que se reconstruyó directamente desde image.dd buscando las firmas de inicio y fin del ZIP.
 
Figura 12. Error al intentar abrir el ZIP exportado inicialmente.
 
Figura 13. Reconstrucción del ZIP desde image.dd: inicio, final y tamaño recuperado.
4.11 Extracción de Scheduled Visits.xls
El ZIP reconstruido mostró el archivo Scheduled Visits.xls y la columna Password indicaba Yes. Se utilizó la contraseña goodtimes para extraerlo.
 
Figura 14. ZIP reconstruido mostrando Scheduled Visits.xls protegido con contraseña.
4.12 Apertura de la hoja de cálculo recuperada
Al abrir Scheduled Visits.xls en Excel, se visualizaron las escuelas visitadas por Joe Jacobs, incluyendo Smith Hill High School y otras escuelas vecinas.
 
Figura 15. Scheduled Visits.xls abierto en Excel con la programación de visitas a escuelas.
 
5. Respuestas a las preguntas del caso
Pregunta 1. ¿Quién es el proveedor de marihuana de Joe Jacobs y cuál es la dirección listada del proveedor?
El proveedor identificado es Jimmy Jungle. La dirección listada es 626 Jungle Ave Apt 2, Jungle, NY 11111. Esta información fue recuperada desde el archivo eliminado Jimmy Jungle.doc.
Pregunta 2. ¿Qué dato crucial está disponible dentro de coverpage.jpg y por qué el dato es crucial?
El dato crucial es pw=goodtimes. Es crucial porque corresponde a la contraseña utilizada para abrir el archivo comprimido que estaba oculto o enmascarado como Scheduled Visits.exe. La contraseña permitió acceder a Scheduled Visits.xls.
Pregunta 3. ¿Qué otras escuelas vecinas a Smith Hill frecuentaba Joe Jacobs?
Además de Smith Hill High School, Joe Jacobs frecuentaba las siguientes escuelas:
•	Key High School
•	Leetch High School
•	Birard High School
•	Richter High School
•	Hull High School
Pregunta 4. Para cada archivo, ¿qué procesos hizo el sospechoso para enmascararlo de otros?
Archivo	Proceso de ocultamiento o enmascaramiento
Jimmy Jungle.doc	Fue eliminado para ocultar la información del proveedor.
cover page.jpgc	Aparentaba ser un archivo gráfico, pero en la evidencia se encontró información asociada a la contraseña.
Scheduled Visits.exe	Fue presentado con extensión .exe, aunque el análisis demostró que era un archivo ZIP.
Scheduled Visits.xls	Estaba dentro de un ZIP protegido con contraseña.

Pregunta 5. ¿Qué procesos realizó usted como analista para examinar el contenido completo de cada archivo?
Como analista, primero se descargó image.zip y se verificó su integridad mediante el hash MD5. Luego se creó copia_image.zip como copia de trabajo y se confirmó que mantuviera el mismo hash. Posteriormente se descomprimió la evidencia y se preparó image.dd.
En Autopsy se creó el caso Caso_Joe_Jacobs, se agregó image.dd como Disk Image or VM File y se revisaron los archivos relevantes. Jimmy Jungle.doc fue analizado en la pestaña Text para obtener el proveedor y su dirección. Para cover page.jpgc y la contraseña, se realizó una búsqueda de cadenas dentro de image.dd, identificando pw=goodtimes. Para Scheduled Visits.exe, se verificó el tipo MIME application/zip y la firma hexadecimal 50 4B 03 04. Como el ZIP exportado inicialmente estaba incompleto, se reconstruyó el archivo desde image.dd y se extrajo Scheduled Visits.xls usando la contraseña goodtimes.
6. Conclusión
El análisis forense permitió recuperar información relevante de la imagen del disquete. Se comprobó que el sospechoso aplicó varias técnicas de ocultamiento: eliminación de archivos, cambio de extensión y protección con contraseña.
El documento eliminado Jimmy Jungle.doc permitió identificar al proveedor como Jimmy Jungle y su dirección. La búsqueda de cadenas permitió encontrar la contraseña pw=goodtimes. Finalmente, Scheduled Visits.exe fue identificado como un ZIP enmascarado, reconstruido desde la imagen forense y usado para extraer Scheduled Visits.xls, donde se encontraron las escuelas frecuentadas por Joe Jacobs.
7. Lista de evidencias incluidas
Evidencia	Descripción
Figura 1	Hash MD5 de image.zip.
Figura 2	Hash MD5 de copia_image.zip.
Figura 3	Creación de image.dd.
Figura 4	Creación del caso en Autopsy.
Figura 5	Selección de fuente de datos Disk Image or VM File.
Figura 6	Carga de image.dd.
Figura 7	Archivos encontrados en Autopsy.
Figura 8	Contenido de Jimmy Jungle.doc.
Figura 9	Contraseña pw=goodtimes.
Figura 10	Scheduled Visits.exe reconocido como application/zip.
Figura 11	Firma ZIP 50 4B 03 04.
Figura 12	Error del ZIP exportado inicialmente.
Figura 13	ZIP reconstruido desde image.dd.
Figura 14	Scheduled Visits.xls protegido con contraseña.
Figura 15	Escuelas en Scheduled Visits.xls.

