Desarrollo Lab 02

  

Grupo

  

Ernesto Camacho Arroyave

Luz Yamile Perez

Javier Duran

Juan Diego Tejada

  
  

######################################################################

  

Parte A

ANALISIS ARCHIVO: SYSTEM

  

TIMEZONE

Define la zona horaria

![]()

Se determina que es GMT -1

SHUTDOWN

Permite saber la hora de apagado del equipo

![]()

PROCESSOR ARCHITECTURE

Determina si el sistema es de 32 o 64 bits.

![]()

COMPNAME

Identifica el nombre del equipo.

![]()

  
  

ARCHIVO: SOFTWARE

WINVER

Permite conocer la versión del sistema operativo y fecha de instalación.

![]()

Identificamos que es un Windows XP Service Pack 3 instalado el 13 de mayo de 2008

######################################################################

Parte B

Análisis del archivo: NTUSER.DAT

RECENTDOCS

![]()

En este caso identificamos la lista de los documentos a los que accedió de manera reciente.

LastWrite Time: 2008-07-20 01:28:04Z

4 = m57biz.xls

3 = tag-cloud.jpg

1 = My Pictures

2 = t1soft.flipflops.jpg

0 = LightBlueTop.gif

TYPEDURLS

En este caso el historial de navegacion de la usuaria.

![]()

Teniendo en cuenta la funcionalidad de la herramienta para buscar más información es posible instalar plugins con funciones adicionales. En este caso unreadmail y printers.

![]()

En este caso, no había información adicional sobre estos.

  

######################################################################

Parte C

Investigación en Autopsy

![]()

  
  

Reto

  

Usando FTK Imager generamos el proceso de validación del hash

  

![]() FTK1 (cargue de la información a ftk)

![]() FTK2 (Cargue del análisis de hash)

![]() FTK 3 (Muestra el Hash)

  

En el desktop del equipo de jean se encuentran tres archivos descarga en el contexto, uno de excel y las ùltimas url consultadas

![]() Imagen 13 (Muestra los archivos)

  

Al exportar el archivo de excel fue posible abrirlo y verificar que contenìa la informaciòn de los salarios

![]() imagen 14 (Excel con salarios)

Ahora yéndonos a recent podemos corroborar que el usuario utilizò los archivos descritos anteriormente asì como las fechas y horas de uso

![]() Imagen 15 (Información de Recent)

Dirigiendonos por medio de la ruta Local settings-app data-Microsoft-Outlook encontramos el archivo.pst de jean con el fin de constatar los correos enviados y recibidos por ella.

![]() Imagen 16 (outlook pst)

Utilizando el visor pst online logramos ver los archivos que contiene

  
  
  

![]() Imagen 18 (Visor del pst)

Durante el análisis del archivo outlook.pst perteneciente al usuario Jean se identificó un correo electrónico enviado el 19 de julio de 2008 hacia la dirección externa “tuckgorge@gmail.com”. En dicho mensaje se adjuntó el archivo “m57biz.xls”, el cual contenía información sensible de empleados, incluyendo nombres, salarios y números SSN. Adicionalmente, el encabezado del correo presenta indicios de posible suplantación de identidad (spoofing), debido a inconsistencias entre el remitente visible y la dirección real utilizada.

![]() Imagen 18 (Correo electronico con adjunto)

Ruta de retorno: <[simsong@xy.dreamhostps.com](mailto:simsong@xy.dreamhostps.com)>

De: tuckgorge@gmail.com ([alison@m57.biz](mailto:alison@m57.biz))

  

![]() imagen 19 (espeificación del correo)

Conclusiòn

El análisis forense realizado sobre la imagen digital del caso M57 permitió identificar y documentar un incidente de exposición y exfiltración de información sensible perteneciente a la empresa M57.biz. Durante el procedimiento se verificó inicialmente la integridad de la evidencia mediante validación hash MD5 y SHA1 utilizando la herramienta FTK Imager, obteniendo coincidencia entre los valores calculados y los almacenados en la imagen forense, garantizando así la autenticidad de la evidencia analizada.

  

En el perfil del usuario “Jean” se localizó el archivo m57biz.xls, el cual contenía información confidencial de empleados, incluyendo nombres, cargos, salarios y números de seguridad social (SSN). Adicionalmente, se evidenció actividad reciente relacionada con dicho archivo mediante accesos directos y registros del sistema, confirmando interacción por parte del usuario.

  

Posteriormente, mediante el análisis del archivo outlook.pst, se identificó un correo electrónico recibido con características asociadas a suplantación de identidad (spoofing/phishing), donde aparentemente se solicitaba información corporativa urgente en nombre de “Alison”. El análisis técnico de los encabezados reveló inconsistencias entre el remitente visible y la dirección real utilizada, destacándose el uso de la cuenta externa tuckgorge@gmail.com.

  

Finalmente, se confirmó que el usuario Jean respondió al correo sospechoso adjuntando el archivo m57biz.xls, lo que permitió establecer evidencia de exfiltración de información sensible hacia una dirección externa no corporativa. Con base en los hallazgos obtenidos, se concluye que el incidente estuvo asociado a un ataque de ingeniería social mediante correo electrónico, el cual derivó en la divulgación no autorizada de información confidencial de la organización.
