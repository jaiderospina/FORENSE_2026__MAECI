
# Informe de Caso Forense M57-Jean

**Guía base: LABORATORIO_M57_JON.pdf**

Curso	Forense / Ciberseguridad

Caso	M57-Jean
Estudiante:	Jesús Andrés Cuastumal
Angelica Leon
Cesar Ferrer
Diana Herran
Tania Diaz
Fecha	08/05/2026

**1. Objetivo**

Analizar la imagen forense del caso M57-Jean con el fin de identificar artefactos relevantes del sistema y del usuario, reconstruir actividad reciente, validar aspectos de integridad de la evidencia y documentar técnicamente los hallazgos obtenidos conforme a la guía del laboratorio.

**2. Descripción de la evidencia**

La evidencia principal del caso corresponde a la imagen forense segmentada nps-2008-jean.E01 y nps-2008-jean.E02, asociada al escenario M57-Jean. A partir de esta imagen se realizo la exploración del sistema de archivos, la extracción de hives del registro de Windows y la recuperación del archivo NTUSER.DAT del usuario Jean.
Como evidencia complementaria de trabajo se obtuvieron copias de los archivos DEFAULT, SAM, SECURITY, SOFTWARE, SYSTEM y NTUSER.DAT, los cuales fueron analizados posteriormente con RegRipper. La carga inicial de la imagen y su selección dentro de FTK Imager quedaron documentadas en las figuras del desarrollo, especialmente en las evidencias correspondientes a la apertura de nps-2008-jean.E01.

**3. Herramientas utilizadas**

Para el desarrollo del caso se utilizaron tres herramientas principales. FTK Imager se empleó para cargar la imagen forense, explorar la estructura del sistema de archivos, exportar artefactos críticos y ejecutar la verificación de la imagen.
RegRipper se utilizó para interpretar los hives del registro de Windows y extraer información puntual relacionada con zona horaria, apagado del sistema, arquitectura, nombre del equipo, versión del sistema operativo, documentos recientes, historial de URLs, correos no leídos e impresoras.
Autopsy se utilizó para montar la imagen completa dentro de un caso forense, configurar la zona horaria correcta y recuperar artefactos de actividad reciente, como documentos, Shell Bags, mensajes de correo y descargas web. El uso de estas herramientas se encuentra soportado por las figuras y capturas incluidas en las secciones posteriores del informe.

**4. Parte Uno: Extracción de archivos críticos del sistema**

En la primera fase del laboratorio se realizó la incorporación de la imagen forense en FTK Imager y la posterior extracción manual de los archivos críticos del sistema desde la ruta Windows\System32\config.

4.1 Carga inicial de la evidencia en FTK Imager
Siguiendo la guía del laboratorio, en FTK Imager se seleccionó la opción Image File como tipo de fuente de evidencia. Esta selección es correcta porque la evidencia entregada corresponde a una imagen forense almacenada en archivo y no a un disco físico o lógico conectado al equipo.

<div align="center">

 ![](/RETO_M57/M57-Grupo2/Figuras/Figura1.png)

**Figura 1. Seleccion de la fuente de evidencia como Image File en FTK Imager.**
</div>
Posteriormente se ubicó y selecciono el archivo nps-2008-jean.E01, que corresponde al primer segmento de la imagen forense del caso M57-Jean. A partir de este archivo FTK Imager reconoce y monta la evidencia para su exploracion.

![Fig2](/Figuras/Figura2.png)
Figura 2. Seleccion del archivo nps-2008-jean.E01 para cargar la evidencia forense.

4.2 Navegación a la carpeta de configuración del sistema
Una vez cargada la evidencia, se navegó por el árbol de directorios hasta la ruta Windows\System32\config. En esta ubicación se encuentran los principales hives del registro de Windows, los cuales son fundamentales en una investigación forense porque conservan información del sistema operativo, cuentas locales, configuraciones, programas instalados y políticas de seguridad.

![Fig3](/Figuras/Figura3.png)
Figura 3. Acceso a la carpeta Windows\System32\config dentro de la imagen forense.
Durante la revisión visual de esta carpeta se identificaron, entre otros, los archivos default, SAM, SECURITY, software y system, los cuales fueron exportados a una carpeta de trabajo separada para su análisis posterior con RegRipper.

4.3 Exportación y validación de los archivos del sistema
Conforme a la guía, se creó una carpeta denominada archivos_del_sistema para organizar la evidencia extraída. Luego se exportaron los cinco archivos requeridos: DEFAULT, SAM, SECURITY, SOFTWARE y SYSTEM. La comprobación final consistió en verificar que los cinco archivos estuvieran presentes en la carpeta de destino.

![Fig4](/Figuras/Figura4.png)
Figura 4. Verificación de la exportación de los cinco archivos críticos del sistema.
Resultado de esta fase: la evidencia base para el análisis del registro del sistema quedó correctamente preparada, lo que permite continuar con la identificación de zona horaria, último apagado, arquitectura del equipo, nombre del host y versión del sistema operativo.
Evidencia(s): Figuras 1 a 4.
Análisis: La fase de adquisición manual de hives se completó de acuerdo con la guía LABORATORIO_M57_JON.pdf. La exportación correcta de estos archivos garantiza que el análisis posterior pueda hacerse sobre copias de trabajo, manteniendo separada la evidencia original y facilitando la documentación pericial.

**5. Actividad A: Relevancia forense de los archivos exportados**
Los archivos exportados desde Windows\System32\config corresponden a hives del registro de Windows. En una investigación forense son especialmente importantes porque almacenan configuraciones persistentes del sistema y del entorno de seguridad, lo que permite reconstruir estados del equipo, identificar usuarios, software, actividad técnica y políticas aplicadas.
DEFAULT: contiene configuraciones del perfil por defecto utilizado por Windows antes de que un usuario inicie sesión y como base para nuevos perfiles. Su utilidad forense radica en que permite observar parámetros generales del entorno y ciertos valores iniciales que podrían heredarse a cuentas nuevas.
SAM: almacena información de las cuentas locales del sistema, incluyendo nombres de usuario, identificadores RID y datos asociados a autenticación. En forense resulta clave para identificar usuarios locales, privilegios y artefactos relacionados con credenciales.
SECURITY: conservas políticas de seguridad locales y secretos LSA. Puede aportar información sobre configuraciones de seguridad, relaciones de confianza y algunos secretos protegidos por el sistema, por lo que es relevante en investigaciones sobre acceso, privilegios o persistencia.
SOFTWARE: contiene información sobre el sistema operativo y sobre programas instalados o configurados. Es útil para determinar la versión de Windows, fecha de instalación, propietario registrado, aplicaciones disponibles y configuraciones de software con valor probatorio.
SYSTEM: almacena parámetros esenciales del sistema, control sets, nombre del equipo, servicios, configuración de hardware y zona horaria. Es uno de los hives más importantes para reconstruir la configuración técnica del equipo y validar correctamente la línea de tiempo del caso.
NTUSER.DAT: aunque no esta en System32\config, su extracción también fue fundamental. Este archivo corresponde al perfil del usuario Jean y almacena actividad específica del usuario, como documentos recientes, URLs escritas, configuraciones del explorador y otros artefactos de interés forense.
En conjunto, estos archivos permiten articular una visión tanto del sistema como del comportamiento del usuario. Por ejemplo, en esta práctica el hive SYSTEM permitió determinar la zona horaria y el último apagado, mientras que NTUSER.DAT revelo la presencia de m57biz.xls entre los documentos recientes del usuario.

**6. Parte Uno: Análisis con RegRipper**
En esta fase se utilizó la herramienta RegRipper desde la línea de comandos para interpretar la información contenida en los hives exportados. El primer análisis realizado correspondió a la determinación de la zona horaria del sistema, dato indispensable para interpretar correctamente las marcas temporales del caso.

6.1 Determinacion de la zona horaria
Se ejecuto el plugin timezone sobre el archivo system mediante el comando rip.exe -r [ruta_del_archivo] -p timezone. La salida obtenida permitió identificar la configuración horaria almacenada en la clave TimeZoneInformation del registro de Windows.

![Fig5](/Figuras/Figura5.png)
Figura 5. Ejecución del plugin timezone sobre el archivo system mediante RegRipper.
La evidencia muestra que la clave analizada fue ControlSet001\Control\TimeZoneInformation, con fecha de ultima escritura 2008-05-14 06:55:57Z. Adicionalmente, se observaron los valores DaylightName = GMT Daylight Time, StandardName = GMT Standard Time, Bias = 0 y ActiveTimeBias = -60.
Interpretación forense: el valor ActiveTimeBias = -60 indica un desfase de menos 60 minutos con respecto al tiempo de referencia, lo que equivale a GMT-1. Este hallazgo es especialmente importante porque la propia guía advierte que, al procesar la imagen en Autopsy, la zona horaria del entorno puede no coincidir con la configuración real del sistema analizado. Por ello, esta evidencia debe usarse como base para ajustar correctamente la linea de tiempo del caso.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\system" -p timezone
Evidencia(s): Figura 5.
Análisis: La determinación de la zona horaria constituye un paso critico de validez temporal. Antes de correlacionar documentos recientes, correos, navegación o eventos del sistema, es necesario normalizar todas las marcas de tiempo usando este hallazgo, para evitar interpretaciones erradas sobre el orden real de los hechos.

6.2 último apagado del sistema
Se ejecuto el plugin shutdown sobre el archivo system para determinar la fecha y hora del último apagado registrado por el sistema operativo.

![Fig6](/Figuras/Figura6.png)
Figura 6. Ejecución del plugin shutdown sobre el archivo system mediante RegRipper.
La salida del comando indica que la clave analizada fue ControlSet001\Control\Windows y que el valor ShutdownTime corresponde a 2008-07-21 01:31:32Z. La misma marca temporal aparece también como fecha de ultima escritura de la clave.
Interpretación forense: este dato representa el último momento en que el sistema registro un apagado. Es una referencia temporal importante para delimitar el intervalo de actividad del equipo y para correlacionar eventos cercanos, como apertura de documentos, actividad del usuario, correos o navegación web. Al igual que en el caso de la zona horaria, esta marca debe interpretarse teniendo en cuenta el desfase identificado previamente.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\system" -p shutdown
Evidencia(s): Figura 6.
Análisis: La coincidencia entre LastWrite time y ShutdownTime fortalece la consistencia del hallazgo. Este valor servirá como punto de referencia para la linea de tiempo del caso, especialmente al contrastarlo con documentos recientes y otros artefactos de actividad del usuario.

6.3 Arquitectura del procesador
Se ejecuto el plugin processor_architecture sobre el archivo system con el fin de identificar la arquitectura del procesador registrada por el sistema.

![Fig7](/Figuras/Figura7.png) 
Figura 7. Ejecucion del plugin processor_architecture sobre el archivo system mediante RegRipper.
La salida reporto PROCESSOR_ARCHITECTURE = x86, junto con el identificador del procesador x86 Family 6 Model 14 Stepping 8, GenuineIntel y la revision 0e08.
Interpretación forense: el valor x86 corresponde a una arquitectura de 32 bits. Este hallazgo coincide con la referencia mostrada en la guía del laboratorio y ayuda a contextualizar el entorno técnico del equipo analizado, lo que puede ser útil al evaluar compatibilidad de software, artefactos del sistema y comportamiento de aplicaciones instaladas.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\system" -p processor_architecture
Evidencia(s): Figura 7.
Análisis: Determinar la arquitectura del sistema es importante porque permite comprender mejor el entorno operativo de la evidencia. En este caso se establece que el sistema investigado corresponde a una plataforma de 32 bits.

6.4 Nombre del equipo
Se ejecuto el plugin compname sobre el archivo system para identificar el nombre asignado al equipo dentro del sistema operativo.

![Fig8](/Figuras/Figura8.png)
Figura 8. Ejecucion del plugin compname sobre el archivo system mediante RegRipper.
La salida mostró los siguientes valores relevantes: ComputerName = JEAN-13FBF038A3, Hostname = jean-13fbf038a3, NV Hostname = jean-13fbf038a3 y DhcpDomain = localdomain.
Interpretación forense: estos datos permiten individualizar el host analizado y asociar artefactos del sistema a un nombre concreto de equipo. En este caso, el nombre registrado del equipo es JEAN-13FBF038A3. Aunque la guía presenta como ejemplo un nombre distinto, para el informe debe prevalecer la evidencia obtenida directamente durante la practica, ya que es la que documenta la ejecución real realizada por el estudiante.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\system" -p compname
Evidencia(s): Figura 8.
Análisis: La identificación del ComputerName y del Hostname ayuda a contextualizar el sistema dentro de una red o entorno corporativo. Este dato puede ser útil para correlacionar registros, artefactos de red, configuraciones de usuario y otros elementos del caso.

6.5 Versión del sistema operativo y fecha de instalación
Para identificar la versión del sistema operativo se cambió el archivo de entrada al hive software y se ejecutó el plugin winver.

![Fig9](/Figuras/Figura9.png)
Figura 9. Ejecucion del plugin winver sobre el archivo software mediante RegRipper.
La salida del comando reporto los siguientes datos: ProductName = Microsoft Windows XP, CSDVersión = Service Pack 3, BuildLab = 2600.xpsp.080413-2111, RegisteredOwner = Jean User e InstallDate = 2008-05-13 21:29:32Z.
Interpretación forense: el sistema analizado corresponde a Microsoft Windows XP Service Pack 3. Adicionalmente, la fecha de instalación registrada fue 13 de mayo de 2008 a las 21:29:32 UTC, dato que permite contextualizar cronológicamente la vida útil del sistema y contrastar eventos posteriores con la antigüedad de la instalación.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\software" -p winver
Evidencia(s): Figura 9.
Análisis: La identificacion de la versión exacta del sistema operativo es relevante para entender el entorno donde ocurrieron los hechos, las aplicaciones compatibles y el comportamiento esperado de los artefactos forenses. En este caso, la evidencia coincide con la referencia general de la guía respecto a Windows XP Service Pack 3.

**7. Actividad B: Otros plugins de RegRipper**

RegRipper dispone de numerosos plugins que facilitan la interpretación de hives del registro de Windows. Además de los utilizados en esta práctica, existen otros complementos de gran valor forense que permiten ampliar el análisis del sistema y del usuario.
userassist: recupera información sobre programas ejecutados por el usuario desde el entorno gráfico de Windows. Es útil para inferir frecuencia de uso de aplicaciones y actividad interactiva.
run y runonce: examinan claves de inicio automático. Son relevantes para detectar persistencia, ejecución automática de programas y posibles mecanismos de malware.
runmru: obtiene el historial de comandos escritos en la ventana Ejecutar de Windows. Permite identificar acciones manuales del usuario, rutas abiertas y programas invocados directamente.
shellbags: reconstruye información de carpetas abiertas por el usuario, incluso si ya no existen. Es especialmente útil para conocer rutas exploradas y actividad en dispositivos externos o ubicaciones de red.
usb y usbstor: recuperan rastros de dispositivos USB conectados al equipo. Estos plugins son muy importantes en investigaciones de exfiltracion de información o uso de medios removibles.
muicache: muestra programas cuya interfaz fue cargada en el sistema. Puede complementar evidencia de ejecución de aplicaciones, incluso cuando otras huellas son limitadas.
typedpaths: obtiene rutas escritas manualmente en el Explorador de Windows. Aporta contexto sobre directorios o ubicaciones accedidas por el usuario.
recentdocs: usado en esta práctica, permite conocer documentos recientes. Su importancia radica en que ayuda a vincular al usuario con archivos concretos y con marcas temporales del registro.
La disponibilidad de estos plugins convierte a RegRipper en una herramienta muy versatil para el análisis de registro. Su uso combinado permite complementar la investigación automatizada de herramientas como Autopsy con una interpretación más puntual de artefactos específicos.

**8. Parte Dos: Análisis de NTUSER.DAT**
En la segunda parte del laboratorio se procedió a extraer y analizar el archivo NTUSER.DAT del usuario Jean. Este archivo contiene información de actividad específica del perfil, como documentos recientes, historial de navegación, rutas utilizadas y otras preferencias del usuario.

8.1 Localización y extracción de NTUSER.DAT
Dentro de FTK Imager se navegó hasta la ruta Documents and Settings\Jean, correspondiente al perfil del usuario investigado. Desde esta ubicación se realizó la búsqueda del archivo NTUSER.DAT.

 
Figura 10. Navegación a la carpeta del perfil del usuario Jean dentro de la imagen forense.
Al desplazarse dentro del contenido de la carpeta se identificó el archivo NTUSER.DAT junto con archivos asociados como LOG y FileSlack. Posteriormente se exporto el archivo principal hacia la carpeta de trabajo archivos_del_sistema para su análisis con RegRipper.

 
Figura 11. Localizacion del archivo NTUSER.DAT dentro del perfil del usuario Jean.
Durante la exportación se generó también un archivo NTUSER.DAT.copy0. La revision posterior permitió establecer que se trata de una copia duplicada producida por una segunda exportación, mientras que el archivo de trabajo principal corresponde a NTUSER.DAT.

8.2 Análisis de documentos recientes con recentdocs
Se ejecuto el plugin recentdocs sobre el archivo NTUSER.DAT con el objetivo de identificar los últimos documentos y carpetas abiertos por el usuario.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\NTUSER.DAT" -p recentdocs
La salida del análisis mostro que, en la clave Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs, el elemento mas reciente registrado fue m57biz.xls, seguido por tag-cloud.jpg, My Pictures, t1soft.flipflops.jpg y LightBlueTop.gif. Ademas, en la subclave .xls se confirmo nuevamente la presencia de m57biz.xls con LastWrite Time 2008-07-20 01:28:04Z.
Interpretación forense: la presencia de m57biz.xls como documento reciente es un hallazgo especialmente relevante para el caso, ya que corresponde al archivo mencionado en la narrativa del incidente. Esto sugiere que el usuario interactuó con una hoja de cálculo directamente relacionada con la información sensible investigada.
Evidencia(s): Figuras 10 y 11, salida de consola del plugin recentdocs.
Análisis: El plugin recentdocs permite reconstruir parte de la actividad del usuario desde el registro de Windows. En este caso, la aparición de m57biz.xls fortalece la hipotesis de que el archivo fue abierto recientemente en el equipo analizado, lo cual lo convierte en un artefacto clave para la línea de tiempo y la atribución de eventos.

8.3 Historial de navegación con typedurls
Se ejecuto el plugin typedurls sobre el archivo NTUSER.DAT para recuperar las direcciones URL escritas manualmente por el usuario en Internet Explorer.

 
Figura 12. Ejecucion del plugin typedurls sobre NTUSER.DAT mediante RegRipper.
La salida del comando mostro la clave Software\Microsoft\Internet Explorer\TypedURLs con fecha de ultima escritura 2008-07-18 05:02:18Z. Entre las direcciones recuperadas se encuentran http://www.aim.com/, http://ebay.com/, http://leather-backpacks.com/, http://google.com/, http://www.ebay.com/, http://gap.com/, http://www.kant.com/, http://www.google.com/ y una URL de redirección de Microsoft relacionada con Internet Explorer.
Interpretación forense: este artefacto evidencia actividad de navegación y consultas realizadas por el usuario desde el navegador Internet Explorer. Aunque las URL observadas no prueban por sí solas la exfiltración de información, sí aportan contexto sobre los habitos de uso del sistema y pueden complementar la linea de tiempo de actividad del usuario.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\NTUSER.DAT" -p typedurls
Evidencia(s): Figura 12.
Análisis: El plugin typedurls recupera un historial de direcciones ingresadas por el usuario y constituye una fuente útil para perfilar actividad web. En este caso se observan accesos a sitios generales y comerciales, lo que permite corroborar el uso interactivo del navegador por parte del usuario analizado.

8.4 Correos no leídos con unreadmail
 
Figura 14. Localización del plugin unreadmail.pl dentro del paquete plugins20130429.
 
Figura 13. Archivo histórico de descargas de RegRipper donde se identifica el paquete plugins20130429.zip.
Una vez incorporado el plugin unreadmail.pl a la carpeta plugins de RegRipper, se repitió el análisis sobre el archivo NTUSER.DAT para comprobar la existencia de correos no leídos en el perfil del usuario.

 
Figura 16. Ejecución final de los plugins unreadmail y printers después de incorporar los complementos faltantes.
La salida del plugin reporto que la clave Software\Microsoft\Windows\CurrentVersion\UnreadMail no fue encontrada en el hive NTUSER.DAT analizado.
Interpretación forense: la ausencia de esta clave sugiere que no se encontraron registros de correos no leídos en el perfil del usuario al momento de la adquisición de la evidencia. Este resultado coincide con la orientación general de la guía, en la que se indica que no se evidencian correos pendientes de lectura.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\NTUSER.DAT" -p unreadmail
Evidencia(s): Figura 16.
Análisis: Tras resolver la falta inicial del plugin, el resultado definitivo del análisis fue negativo para correos no leidos. Esto permite descartar, con base en este artefacto específico, evidencia de mensajes pendientes de apertura en el entorno del usuario.

8.5 Impresoras o documentos impresos con printers
 
Figura 15. Localizacion del plugin printers.pl dentro del paquete plugins20130429.
De igual manera, una vez agregado el plugin printers.pl al directorio plugins de RegRipper, se repitio la consulta sobre NTUSER.DAT para identificar impresoras registradas o rastros de impresion asociados al usuario.
La salida mostro la clave Software\Microsoft\Windows NT\CurrentVersion\PrinterPorts con LastWrite Time Sun Jul 20 00:01:12 2008; sin embargo, se indico expresamente que dicha clave no contenia valores.
Interpretación forense: aunque la ruta de configuración de impresoras existe dentro del perfil, no se recuperaron valores asociados a impresoras específicas. En consecuencia, no se obtuvo evidencia concluyente de impresoras configuradas o actividad de impresion a partir de este artefacto.
Comando ejecutado: rip.exe -r "C:\Users\manom\OneDrive\Documents\FORENSE_2026\archivos_del_sistema\NTUSER.DAT" -p printers
Evidencia(s): Figura 16.
Análisis: El resultado final del plugin printers fue negativo en cuanto a valores útiles para la investigación. Esto es coherente con la guía del laboratorio, la cual anticipa que no se identifican impresiones relevantes en este punto del caso.

**9. Parte Tres: Análisis en Autopsy**

En la tercera parte del laboratorio se continuo el análisis mediante Autopsy, herramienta utilizada para montar la imagen forense, procesar su contenido y extraer artefactos de actividad reciente del sistema.
9.1 Creacion del caso y seleccion del host
Se creo un nuevo caso en Autopsy con el nombre M57-Jean. Durante el asístente de adicion de la fuente de datos se dejo marcada la opcion Generate new host name based on data source name, lo cual es suficiente para organizar la evidencia dentro del caso.

 
Figura 17. Seleccion del host al agregar la fuente de datos en Autopsy.
9.2 Seleccion del tipo de fuente de datos
Conforme a la guía, en la etapa Select Data Source Type se selecciono la opcion Disk Image or VM File, debido a que la evidencia corresponde a una imagen forense segmentada en formato E01.

 
Figura 18. Seleccion del tipo de fuente de datos como Disk Image or VM File.
9.3 Carga de la imagen y ajuste de zona horaria
Posteriormente se selecciono el archivo nps-2008-jean.E01 como fuente de datos. En una primera vista se observa que Autopsy propone la zona horaria del entorno local, en este caso America/Bogota, la cual no coincide con la configuracion real del sistema obtenida previamente con RegRipper.

 
Figura 19. Carga inicial de la imagen forense con la zona horaria local propuesta por Autopsy.
Siguiendo estrictamente la guía y los resultados del plugin timezone, se corrigio la zona horaria a (GMT-1:00) Etc/GMT+1, equivalente al hallazgo de ActiveTimeBias = -60. Esta correccion es fundamental para mantener consistencia en la interpretacion de la linea de tiempo.

 
Figura 20. Ajuste manual de la zona horaria a GMT-1:00 Etc/GMT+1 segun la evidencia del registro.
9.4 Configuración del módulo de ingesta
En la fase Configure Ingest se verifico que el módulo Recent Activity estuviera habilitado, tal como lo solicita la guía. Este módulo permite recuperar actividad reciente del usuario, incluyendo navegación, documentos y otros artefactos de interes.

 
Figura 21. Configuración del módulo de ingesta con Recent Activity seleccionado.
9.5 Procesamiento y adicion de la fuente de datos
Una vez confirmada la configuración, Autopsy inicio el procesamiento de la imagen y la adicion de la fuente de datos a la base local del caso.

 
Figura 22. Proceso de adicion y análisis inicial de la fuente de datos en Autopsy.
Finalmente, la fuente de datos fue agregada correctamente y el sistema comenzo a mostrar resultados de análisis en el panel lateral, incluyendo artefactos de actividad reciente como Recent Documents, Shell Bags y USB Device Attached, entre otros.

 
Figura 23. Fuente de datos agregada con exito y visualizacion de resultados iniciales en Autopsy.
Evidencia(s): Figuras 17 a 23.
Análisis: La incorporacion exitosa de la imagen en Autopsy complementa el análisis puntual hecho con RegRipper, ya que permite correlacionar artefactos en una interfaz íntegrada. El ajuste de zona horaria fue un paso critico para preservar la validez temporal del examen, mientras que el módulo Recent Activity habilito la recuperacion automática de rastros relevantes para la investigación.
9.6 Hallazgos relevantes obtenidos en Autopsy
El procesamiento de la imagen en Autopsy permitió recuperar artefactos adicionales que complementan y corroboran parte de la información obtenida previamente con RegRipper.
En la vista Recent Documents se identificaron nueve resultados, entre ellos accesos a m57biz.LNK, m57biz.xls, tag-cloud.lnk, LightBlueTop.lnk, t1soft.flipflops.lnk y NTUSER.DAT. Este hallazgo refuerza la evidencia de que el archivo m57biz.xls fue abierto desde el entorno del usuario analizado.

 
Figura 24. Artefactos de Recent Documents en Autopsy, incluyendo referencias a m57biz.xls y otros archivos recientes.
La vista Shell Bags mostró 42 resultados asociados a rutas y objetos explorados por el usuario, tales como My Documents, My Computer, My Network Places, Recycle Bin, Mozilla Firefox 3 Beta 5.lnk, VMware Shared Folders.lnk e install_flash_player.exe. Este tipo de artefacto es útil para reconstruir la exploracion de carpetas y accesos dentro del sistema.

 
Figura 25. Resultados de Shell Bags recuperados por Autopsy.
En el apartado E-Mail Messages se observaron 261 resultados provenientes de outlook.pst. Entre los asuntos visibles se encuentran Welcome to Microsoft Outlook 2000!, Google Alert - m57.biz, y varios correos reenviados desde cuentas del dominio m57.biz. Aunque esta vista no equivale por sí sola a correos no leidos, sí demuestra la presencia de correo electrónico recuperable dentro de la evidencia.

 
Figura 26. Mensajes de correo recuperados en Autopsy a partir de outlook.pst.
Finalmente, en Web Downloads se identificaron tres resultados, incluyendo referencias a descargas relacionadas con Flash Player y AIM. Este hallazgo aporta contexto adicional sobre la actividad de navegación y descarga realizada desde el sistema.

 
Figura 27. Resultados de Web Downloads recuperados en Autopsy.
Análisis complementario: Los artefactos recuperados por Autopsy fortalecen el valor probatorio del examen porque permiten correlacionar actividad reciente, elementos explorados, correo electrónico y descargas. En especial, la presencia simultánea de m57biz.xls en RegRipper y en Recent Documents de Autopsy constituye un punto de consistencia importante dentro del caso.

**10. Validación de hash**

Se realizo la validación de integridad de la imagen forense nps-2008-jean.E01 mediante la funcion de verificacion de FTK Imager. Este procedimiento permite comparar el hash calculado por la herramienta con el valor de verificacion almacenado en la evidencia generada en formato EnCase.

 
Figura 28. Resultado de la verificación de la imagen forense en FTK Imager.
Durante la verificación se obtuvo un Sector count de 20971520. En el apartado MD5 Hash, FTK Imager reporto un Computed hash de 7d7d27ea3e274b8a161089c7f8aa4a63 y un Stored verification hash de 78a52b5bac78f4e711607707ac0e3f93.
Sin embargo, el campo Verify result indicó N/A - bad blocks found in image. Adicionalmente, en Bad Blocks List se reportaron bloques defectuosos en el rango 1000576-1000639. También se mostró un valor SHA1 calculado de a6c126fb46164a178bf78d503abd0ce39d6c9aa2.
Interpretación forense: la herramienta no pudo confirmar una verificación íntegra del hash almacenado debido a la presencia de bloques defectuosos dentro de la imagen. En consecuencia, el resultado no debe reportarse como coincidencia exitosa de hash, sino como una verificación incompleta o afectada por bad blocks.
Este hallazgo debe documentarse con especial cuidado en el informe, ya que la validación de integridad es un paso critico del tratamiento de evidencia digital. La presencia de bad blocks no implica automáticamente manipulacion dolosa, pero sí obliga a dejar constancia técnica de que la comprobacion automática del hash almacenado no fue satisfactoria en terminos plenos.
Evidencia(s): Figura 28.
Análisis: La imagen fue sometida a verificación formal en FTK Imager, cumpliendo con la exigencia metodologica del laboratorio. No obstante, el resultado obtenido evidencio bloques defectuosos y una falta de coincidencia verificable entre el hash almacenado y el proceso de comprobacion, por lo que la integridad debe reportarse como observada con limitacion técnica y no como validada positivamente.

**11. Conclusiones**
El análisis forense permitió identificar artefactos relevantes del sistema y del usuario a partir de FTK Imager, RegRipper y Autopsy, cumpliendo la metodología planteada en la guía.
Se confirmo evidencia de actividad reciente del usuario Jean, destacandose la presencia del archivo m57biz.xls en RecentDocs y en Autopsy, lo que lo convierte en un elemento clave dentro de la investigación.
No se hallo evidencia de correos no leidos ni de impresoras configuradas con valores útiles en NTUSER.DAT, aunque sí se recuperaron correos y otros artefactos complementarios desde Autopsy.
La imagen forense pudo ser procesada y analizada, pero la validación de hash en FTK Imager presento bad blocks, por lo que la verificación de integridad debe reportarse con esa limitacion técnica.
