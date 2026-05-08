# Registro Windows.


# RegRipper y sus módulos para interpretar registros de Windows

 **RegRipper** —a veces escrito informalmente como “regriper”—, una herramienta forense para extraer e interpretar información del **Registro de Windows** desde archivos hive como `SYSTEM`, `SOFTWARE`, `SAM`, `SECURITY`, `NTUSER.DAT` y `UsrClass.dat`.

RegRipper trabaja con **plugins o módulos en Perl** que extraen claves, valores, metadatos y marcas temporales del Registro. Puede ejecutarse con un plugin específico, con un perfil completo de hive o automáticamente con todos los plugins aplicables al hive analizado. ([Kali Linux][1])

---

## 1. ¿Qué es el Registro de Windows?

El **Registro de Windows** es una base de datos jerárquica donde Windows y muchas aplicaciones almacenan configuración, estado del sistema, preferencias de usuario, políticas de seguridad, dispositivos conectados, programas instalados y rastros de actividad.

No es un solo archivo. Está dividido en varios archivos llamados **hives**.

### Hives principales

| Hive           | Ubicación típica                                                  | Qué contiene                                                                                          |
| -------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `SYSTEM`       | `C:\Windows\System32\Config\SYSTEM`                               | Configuración del sistema, servicios, drivers, ControlSets, zona horaria, red, USB, nombre del equipo |
| `SOFTWARE`     | `C:\Windows\System32\Config\SOFTWARE`                             | Programas instalados, configuración de Windows, Microsoft, uninstall keys, políticas de software      |
| `SAM`          | `C:\Windows\System32\Config\SAM`                                  | Usuarios locales, grupos, RID, último inicio de sesión, estado de cuentas                             |
| `SECURITY`     | `C:\Windows\System32\Config\SECURITY`                             | Políticas locales, secretos LSA, configuración de seguridad                                           |
| `NTUSER.DAT`   | `C:\Users\<usuario>\NTUSER.DAT`                                   | Actividad y configuración específica del usuario                                                      |
| `UsrClass.dat` | `C:\Users\<usuario>\AppData\Local\Microsoft\Windows\UsrClass.dat` | Shellbags, asociaciones COM, rastros del Explorador y objetos de usuario                              |

RegRipper permite analizar estos hives de forma offline, es decir, sin depender del sistema Windows vivo.

---

## 2. ¿Qué es RegRipper?

**RegRipper** es una herramienta forense de código abierto desarrollada originalmente por Harlan Carvey. Está escrita en Perl y usa plugins para extraer datos específicos del Registro de Windows. Su objetivo no es mostrar todo el Registro como un editor gráfico, sino extraer artefactos relevantes para investigación forense, respuesta a incidentes y análisis de actividad. ([Forensics Wiki][2])

En distribuciones como Kali, el comando se usa para seleccionar un hive, un plugin o un perfil; la salida se imprime en `STDOUT` y puede redirigirse a un archivo. ([Kali Linux][1])

Ejemplo básico:

```bash
regripper -r /evidencia/SYSTEM -f system > system_report.txt
```

Ejemplo con plugin específico:

```bash
regripper -r /evidencia/NTUSER.DAT -p userassist > userassist.txt
```

Ejemplo para listar plugins:

```bash
regripper -l
```

---

## 3. Versiones importantes: RegRipper 3.0 y 4.0

### RegRipper 3.0

RegRipper 3.0 permite ejecutar plugins individuales, perfiles completos o todos los plugins aplicables a un hive mediante la opción `-a`. También conserva la posibilidad de usar perfiles tradicionales como `system`, `software`, `sam`, `security` y `ntuser`. ([GitHub][3])

Opciones comunes:

```bash
-r <hive>      Hive del Registro a analizar
-f <perfil>    Perfil de hive: system, software, sam, security, ntuser
-p <plugin>    Plugin específico
-a             Ejecutar automáticamente plugins aplicables al hive
-aT            Ejecutar plugins de línea de tiempo TLN
-l             Listar plugins
-c             Listar plugins en formato CSV
-d             Verificar si el hive está dirty
-g             Intentar adivinar el tipo de hive
```

### RegRipper 4.0

RegRipper 4.0 incorpora mejoras como formato de tiempo estilo ISO 8601, mapeo MITRE ATT&CK en algunos plugins, “Analysis Tips” y nuevos plugins. También incluye soporte de ejemplo para ejecutar reglas YARA contra datos del Registro mediante `run_yara.pl`. ([GitHub][4])

Una limitación importante: RegRipper **no procesa automáticamente los transaction logs** del Registro. Si el hive está incompleto o “dirty”, puede ser necesario fusionar los logs usando herramientas externas como `yarp + registryFlush.py` o `rla.exe` de Eric Zimmerman antes del análisis. ([GitHub][4])

---

## 4. Conceptos clave para interpretar resultados

### 4.1 Clave

Una **clave** es como una carpeta dentro del Registro.

Ejemplo:

```text
Software\Microsoft\Windows\CurrentVersion\Run
```

Esta clave suele contener programas configurados para ejecutarse al iniciar sesión.

### 4.2 Valor

Un **valor** es un dato dentro de una clave.

Ejemplo:

```text
OneDrive = C:\Users\Ana\AppData\Local\Microsoft\OneDrive\OneDrive.exe
```

### 4.3 LastWrite Time

Muchas claves tienen un timestamp llamado **LastWrite**, que indica la última vez que la clave fue modificada. No siempre equivale a “última ejecución” o “último uso”; depende del artefacto. Es una pista temporal, no una conclusión absoluta.

### 4.4 ControlSet

En el hive `SYSTEM`, Windows guarda configuraciones en `ControlSet001`, `ControlSet002`, etc. La clave:

```text
SYSTEM\Select
```

indica cuál es el ControlSet activo:

```text
Current = 1
```

Si `Current = 1`, normalmente se interpreta `ControlSet001` como `CurrentControlSet`.

---

# 5. HIVE SYSTEM

El hive `SYSTEM` es uno de los más importantes. Contiene información de arranque, servicios, drivers, hardware, red, zona horaria, USB y configuración global del sistema.

## 5.1 Qué contiene `SYSTEM`

| Categoría            | Información útil                                          |
| -------------------- | --------------------------------------------------------- |
| Identidad del equipo | Nombre del host, dominio, configuración de red            |
| Arranque             | Servicios, drivers, ControlSets                           |
| USB                  | Dispositivos conectados, seriales, almacenamiento externo |
| Red                  | Interfaces, IPs, DHCP, gateways, DNS                      |
| Tiempo               | Zona horaria, configuración de reloj                      |
| Servicios            | Servicios instalados, tipo de inicio, rutas               |
| Persistencia         | Servicios maliciosos, drivers sospechosos                 |

---

## 5.2 Módulos comunes para `SYSTEM`

### `compname`

Extrae el nombre del equipo.

**Útil para:**

* Asociar evidencia a un host concreto.
* Confirmar si el hive corresponde al sistema investigado.
* Correlacionar con logs de red, EDR, SIEM o Active Directory.

**Ejemplo interpretativo:**

```text
ComputerName = DESKTOP-9K2FQ1A
```

Esto indica el nombre local configurado para el sistema.

---

### `timezone`

Extrae la zona horaria configurada.

**Por qué importa:**

La zona horaria es crítica para convertir timestamps. Si una máquina está configurada en UTC-5 y otra fuente está en UTC, una mala conversión puede desplazar toda la línea de tiempo.

**Ejemplo:**

```text
TimeZoneKeyName = SA Pacific Standard Time
```

En una investigación, conviene documentar esta zona antes de construir timelines.

---

### `shutdown`

Busca información relacionada con apagado del sistema.

**Puede ayudar a identificar:**

* Último apagado limpio.
* Apagados inesperados.
* Contexto de interrupción de actividad.

**Precaución:** no todos los apagados dejan evidencia completa.

---

### `services`

Enumera servicios instalados.

**Clave típica:**

```text
SYSTEM\CurrentControlSet\Services
```

**Qué revisar:**

* Servicios con nombres extraños.
* Servicios apuntando a rutas temporales.
* Binarios en `AppData`, `Temp`, `ProgramData` o rutas poco comunes.
* Tipo de inicio automático.
* Servicios deshabilitados recientemente.

**Ejemplo sospechoso:**

```text
Service: WinUpdateSvc
ImagePath: C:\Users\Public\svchost.exe
Start: Auto
```

Aunque el nombre parece legítimo, la ruta no es normal para un servicio del sistema.

---

### `svc`

Similar a `services`, suele enfocarse en detalles de configuración de servicios.

**Campos útiles:**

| Campo         | Interpretación                         |
| ------------- | -------------------------------------- |
| `ImagePath`   | Ruta del ejecutable                    |
| `Start`       | Tipo de inicio                         |
| `Type`        | Tipo de servicio o driver              |
| `DisplayName` | Nombre visible                         |
| `ObjectName`  | Cuenta usada para ejecutar el servicio |

---

### `usb`

Extrae rastros de dispositivos USB.

**Claves comunes:**

```text
SYSTEM\CurrentControlSet\Enum\USB
SYSTEM\CurrentControlSet\Enum\USBSTOR
```

**Qué puede revelar:**

* Fabricante.
* Modelo.
* Número de serie.
* Primeras o últimas referencias en el Registro.
* Dispositivos de almacenamiento externo.

**Ejemplo:**

```text
Disk&Ven_SanDisk&Prod_Cruzer_Blade
Serial: 4C530001230915109292
```

Esto sugiere conexión de una unidad USB SanDisk específica.

---

### `usbstor`

Se centra en dispositivos de almacenamiento USB.

**Útil para:**

* Investigar exfiltración de datos.
* Determinar si se conectaron memorias USB.
* Correlacionar con `MountedDevices`, `MountPoints2`, eventos de Windows y artefactos de usuario.

---

### `mountdev` / `mounteddevices`

Analiza dispositivos montados y letras de unidad.

**Clave típica:**

```text
SYSTEM\MountedDevices
```

**Qué contiene:**

* Mapeos entre volúmenes y letras.
* Identificadores de disco.
* Referencias a dispositivos montados.

**Ejemplo interpretativo:**

```text
\DosDevices\E: = <binary volume identifier>
```

Indica que en algún momento se asignó la letra `E:` a un volumen.

---

### `networkcards` / `nic`

Extrae adaptadores de red.

**Puede revelar:**

* Adaptadores físicos y virtuales.
* VPNs.
* Interfaces inalámbricas.
* Adaptadores de virtualización como VMware, VirtualBox o Hyper-V.

---

### `interfaces`

Analiza configuración de interfaces de red.

**Datos típicos:**

* Dirección IP.
* DHCP.
* Gateway.
* DNS.
* Dominio.
* Tiempos de concesión DHCP.

**Ejemplo:**

```text
IPAddress: 192.168.1.25
DhcpServer: 192.168.1.1
NameServer: 8.8.8.8
```

---

### `select`

Interpreta la clave `SYSTEM\Select`.

**Ejemplo:**

```text
Current: 1
Default: 1
Failed: 0
LastKnownGood: 2
```

Sirve para saber qué ControlSet analizar como configuración activa.

---

# 6. HIVE SOFTWARE

El hive `SOFTWARE` contiene información de programas instalados, configuración del sistema operativo, políticas, componentes de Microsoft, rutas de instalación y artefactos de aplicaciones.

## 6.1 Qué contiene `SOFTWARE`

| Categoría            | Información                           |
| -------------------- | ------------------------------------- |
| Programas instalados | Claves `Uninstall`                    |
| Windows              | Versión, build, ProductName           |
| Políticas            | Configuraciones administrativas       |
| Autoruns             | Persistencia en claves globales       |
| Navegadores y apps   | Configuración de software             |
| AppCompat            | Compatibilidad y rastros de ejecución |

---

## 6.2 Módulos comunes para `SOFTWARE`

### `winver`

Extrae versión de Windows.

**Clave típica:**

```text
Microsoft\Windows NT\CurrentVersion
```

**Datos útiles:**

* `ProductName`
* `CurrentBuild`
* `InstallDate`
* `RegisteredOwner`
* `EditionID`

**Ejemplo:**

```text
ProductName: Windows 10 Pro
CurrentBuild: 19045
InstallDate: 2024-08-19
```

---

### `uninstall`

Lista programas instalados.

**Claves típicas:**

```text
Microsoft\Windows\CurrentVersion\Uninstall
Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall
```

**Qué revisar:**

* Programas de acceso remoto.
* Herramientas de administración.
* Software instalado cerca del incidente.
* Aplicaciones sospechosas o no autorizadas.

**Ejemplos de interés forense:**

```text
AnyDesk
TeamViewer
Chrome Remote Desktop
7-Zip
WinSCP
PuTTY
```

---

### `run`

Extrae programas configurados para ejecutarse automáticamente.

**Claves típicas:**

```text
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

**Uso forense:**

* Detectar persistencia.
* Identificar malware que se ejecuta al iniciar sesión.
* Encontrar herramientas legítimas abusadas por atacantes.

**Ejemplo sospechoso:**

```text
SecurityHealth = C:\ProgramData\SecurityHealth\health.exe
```

El nombre imita componentes de Windows, pero la ruta puede ser sospechosa.

---

### `appcompatcache` / `shimcache`

Analiza AppCompatCache, también conocido como ShimCache.

**Qué puede indicar:**

* Existencia o referencia a ejecutables.
* Rutas de binarios.
* Posibles rastros de ejecución, aunque la interpretación depende de la versión de Windows.

**Precaución importante:** ShimCache no siempre prueba ejecución. En muchas versiones de Windows puede indicar que un archivo fue visto o registrado por mecanismos de compatibilidad.

---

### `muicache`

Extrae entradas de MuiCache.

**Qué puede contener:**

* Rutas de ejecutables.
* Nombres descriptivos de aplicaciones.
* Rastros asociados a ejecución o visualización por shell.

**Ejemplo:**

```text
C:\Users\Bob\Downloads\tool.exe = Network Scanner
```

---

### `profilelist`

Lista perfiles de usuario registrados.

**Clave típica:**

```text
Microsoft\Windows NT\CurrentVersion\ProfileList
```

**Datos relevantes:**

* SID del usuario.
* Ruta del perfil.
* Usuarios locales o de dominio que iniciaron sesión.
* Perfiles temporales.

**Ejemplo:**

```text
S-1-5-21-...-1001
ProfileImagePath: C:\Users\maria
```

---

### `policies`

Extrae políticas de sistema o aplicaciones.

**Puede ayudar a detectar:**

* Desactivación de herramientas de seguridad.
* Restricciones administrativas.
* Configuraciones impuestas por GPO.
* Cambios maliciosos de políticas.

---

# 7. HIVE SAM

El hive `SAM` contiene información de cuentas locales y grupos.

## 7.1 Qué contiene `SAM`

| Categoría        | Información                                                   |
| ---------------- | ------------------------------------------------------------- |
| Usuarios locales | Nombre, RID, estado                                           |
| Grupos locales   | Administradores, Usuarios, Invitados                          |
| Logon metadata   | Últimos inicios de sesión, intentos fallidos en algunos casos |
| Estado de cuenta | Deshabilitada, bloqueada, contraseña requerida                |
| Password hints   | En versiones/configuraciones donde aplique                    |

---

## 7.2 Módulos comunes para `SAM`

### `samparse`

Uno de los plugins más usados para interpretar cuentas locales.

**Puede mostrar:**

* Nombre de usuario.
* RID.
* Último inicio de sesión.
* Último cambio de contraseña.
* Conteo de inicios de sesión.
* Estado de cuenta.
* Grupos asociados.

**Ejemplo:**

```text
Username: Administrator
RID: 500
Account disabled: Yes
Last login: 2025-04-12 18:21:03
```

---

### `users`

Lista usuarios locales.

**Útil para:**

* Identificar cuentas creadas por atacantes.
* Ver cuentas deshabilitadas o habilitadas.
* Detectar nombres anómalos como `support$`, `admin2`, `backupsvc`.

---

### `groups`

Extrae grupos locales.

**Grupos clave:**

| Grupo                | Importancia                 |
| -------------------- | --------------------------- |
| Administrators       | Privilegios máximos locales |
| Remote Desktop Users | Capacidad de acceso RDP     |
| Users                | Usuarios estándar           |
| Guests               | Cuentas invitadas           |
| Backup Operators     | Acceso sensible a archivos  |

**Ejemplo de hallazgo:**

```text
Group: Administrators
Members: Administrator, maria, soporte
```

Si `soporte` no debería ser administrador, es un hallazgo relevante.

---

# 8. HIVE SECURITY

El hive `SECURITY` contiene políticas de seguridad local, secretos LSA y configuraciones sensibles. Algunas partes pueden requerir correlación con `SYSTEM`.

## 8.1 Qué contiene `SECURITY`

| Categoría           | Información                             |
| ------------------- | --------------------------------------- |
| Políticas locales   | Configuración de auditoría y seguridad  |
| LSA                 | Secrets, configuración de autenticación |
| Cache domain logons | En ciertos escenarios                   |
| Derechos de usuario | Privilegios asignados                   |
| Políticas de cuenta | Contraseñas, bloqueo, auditoría         |

---

## 8.2 Módulos comunes para `SECURITY`

### `auditpol`

Extrae política de auditoría.

**Útil para saber:**

* Si se auditaban inicios de sesión.
* Si se registraban cambios de privilegios.
* Si había auditoría de procesos.
* Si la máquina tenía visibilidad suficiente para reconstruir eventos.

---

### `lsa`

Analiza configuración LSA.

**Puede ayudar a revisar:**

* Configuraciones de autenticación.
* Parámetros de seguridad.
* Posibles configuraciones debilitadas.

---

### `secrets`

Busca secretos LSA.

**Importante:** la interpretación de secretos LSA puede requerir claves del hive `SYSTEM`. En análisis forense, estos datos son sensibles y deben tratarse con cadena de custodia y controles adecuados.

---

# 9. HIVE NTUSER.DAT

`NTUSER.DAT` es específico de cada usuario. Es uno de los hives más valiosos para reconstruir actividad humana.

## 9.1 Qué contiene `NTUSER.DAT`

| Categoría                | Información                   |
| ------------------------ | ----------------------------- |
| Programas ejecutados     | UserAssist, RunMRU, MUICache  |
| Archivos recientes       | RecentDocs, Office MRU        |
| Explorador               | TypedPaths, MountPoints2      |
| Persistencia por usuario | Run, RunOnce                  |
| Configuración personal   | Escritorio, red, impresoras   |
| RDP                      | Servidores conectados         |
| Búsquedas y comandos     | RunMRU, TypedURLs, TypedPaths |

---

## 9.2 Módulos comunes para `NTUSER.DAT`

### `userassist`

Uno de los artefactos más importantes.

**Clave típica:**

```text
Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist
```

**Qué puede revelar:**

* Programas ejecutados por el usuario.
* Conteo de ejecución.
* Última ejecución aproximada.
* Rutas o identificadores de programas.

**Nota:** algunos valores están codificados con ROT13. RegRipper los decodifica automáticamente.

**Ejemplo:**

```text
Program: C:\Users\maria\Downloads\nmap.exe
Run Count: 3
Last Run: 2025-11-04 14:22:10
```

**Interpretación:** el usuario ejecutó o interactuó con `nmap.exe` desde Descargas. Debe correlacionarse con Prefetch, Amcache, Sysmon o eventos.

---

### `recentdocs`

Extrae documentos recientes abiertos desde el shell.

**Clave típica:**

```text
Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```

**Puede mostrar:**

* Nombres de archivos.
* Extensiones.
* Orden MRU.
* LastWrite de la clave.

**Ejemplo:**

```text
contratos_clientes.xlsx
credenciales.txt
```

Esto puede indicar acceso reciente a esos archivos, pero no necesariamente contenido leído o exfiltrado.

---

### `runmru`

Extrae comandos escritos en la ventana Ejecutar (`Win + R`).

**Clave típica:**

```text
Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU
```

**Ejemplo:**

```text
cmd
powershell
mstsc
\\192.168.1.10\c$
```

**Uso forense:**

* Identificar comandos manuales.
* Ver acceso a recursos compartidos.
* Detectar uso de herramientas administrativas.

---

### `typedpaths`

Extrae rutas escritas en la barra del Explorador.

**Ejemplo:**

```text
C:\Users\maria\Downloads
\\fileserver\finance
```

Puede indicar navegación manual por carpetas locales o compartidas.

---

### `typedurls`

Extrae URLs escritas en Internet Explorer o componentes asociados.

**Ejemplo:**

```text
http://intranet.local
http://192.168.1.50/admin
```

En sistemas modernos, puede tener menor cobertura que artefactos de navegadores actuales, pero sigue siendo útil en ciertos casos.

---

### `mountpoints2`

Relaciona dispositivos o recursos montados en contexto del usuario.

**Clave típica:**

```text
Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2
```

**Puede revelar:**

* Unidades USB accedidas por el usuario.
* Recursos de red.
* Letras de unidad vistas por ese usuario.

**Correlación recomendada:**

* `SYSTEM\USBSTOR`
* `SYSTEM\MountedDevices`
* Eventos 20001/20003 de DriverFrameworks
* Shellbags
* LNK files

---

### `networklist`

Puede mostrar redes conocidas por el usuario o sistema, dependiendo del hive/plugin.

**Datos posibles:**

* Nombre de red.
* Perfil.
* Fecha de primer/último contacto.
* Tipo de red.

---

### `rdp`

Busca rastros de conexiones de Escritorio Remoto.

**Claves comunes:**

```text
Software\Microsoft\Terminal Server Client\Default
Software\Microsoft\Terminal Server Client\Servers
```

**Ejemplo:**

```text
MRU0 = 10.0.0.15
MRU1 = server-contabilidad
```

**Interpretación:** el usuario pudo haber iniciado conexiones RDP hacia esos destinos.

---

### `run`

En `NTUSER.DAT`, analiza autoruns por usuario.

**Clave típica:**

```text
Software\Microsoft\Windows\CurrentVersion\Run
```

**Uso forense:**

* Persistencia en contexto del usuario.
* Malware sin privilegios administrativos.
* Scripts o binarios en perfil de usuario.

**Ejemplo sospechoso:**

```text
Updater = C:\Users\maria\AppData\Roaming\updater.exe
```

---

### `shellbags`

Los Shellbags suelen estar en `UsrClass.dat`, aunque algunos rastros pueden relacionarse con `NTUSER.DAT`.

**Qué indican:**

* Carpetas vistas por el usuario.
* Rutas locales, externas o de red.
* Preferencias de visualización del Explorador.
* Evidencia de navegación por directorios incluso si los archivos ya no existen.

---

# 10. HIVE UsrClass.dat

`UsrClass.dat` se encuentra por usuario y suele ser muy valioso para Shellbags y actividad del Explorador.

## 10.1 Qué contiene

| Categoría      | Información                    |
| -------------- | ------------------------------ |
| Shellbags      | Carpetas visitadas             |
| Objetos COM    | Asociaciones y clases          |
| Explorador     | Interacción con carpetas       |
| Rutas externas | USB, red, volúmenes removibles |

---

## 10.2 Módulos comunes para `UsrClass.dat`

### `shellbags`

**Uso principal:**

Reconstruir carpetas navegadas por el usuario.

**Ejemplo:**

```text
C:\Users\maria\Desktop\Caso
E:\Respaldos
\\NAS01\Finanzas
```

**Interpretación:** el usuario exploró esas rutas mediante el Explorador de Windows u otros componentes shell.

---

# 11. Artefactos forenses importantes y cómo interpretarlos

## 11.1 Programas ejecutados

| Artefacto  | Hive                                 | Módulos típicos  | Qué indica                          |
| ---------- | ------------------------------------ | ---------------- | ----------------------------------- |
| UserAssist | `NTUSER.DAT`                         | `userassist`     | Ejecución o interacción del usuario |
| ShimCache  | `SYSTEM` o `SOFTWARE`, según versión | `appcompatcache` | Presencia/referencia de ejecutables |
| MUICache   | `NTUSER.DAT` / `UsrClass.dat`        | `muicache`       | Rutas y nombres de aplicaciones     |
| RunMRU     | `NTUSER.DAT`                         | `runmru`         | Comandos ejecutados desde Win+R     |
| Autoruns   | `SOFTWARE`, `NTUSER.DAT`             | `run`            | Persistencia o inicio automático    |

### Regla de interpretación

Un solo artefacto rara vez prueba todo. Lo correcto es correlacionar:

```text
UserAssist + Prefetch + Amcache + ShimCache + Event Logs + Sysmon
```

---

## 11.2 Dispositivos USB

| Artefacto        | Hive           | Qué revela                         |
| ---------------- | -------------- | ---------------------------------- |
| `USBSTOR`        | `SYSTEM`       | Dispositivos de almacenamiento USB |
| `USB`            | `SYSTEM`       | Dispositivos USB generales         |
| `MountedDevices` | `SYSTEM`       | Letras de unidad y volúmenes       |
| `MountPoints2`   | `NTUSER.DAT`   | Dispositivos vistos por usuario    |
| Shellbags        | `UsrClass.dat` | Carpetas exploradas en USB         |

### Interpretación típica

Si encuentras:

```text
USBSTOR: SanDisk Cruzer
MountedDevices: E:
MountPoints2 usuario maria: E:
Shellbags: E:\Clientes
RecentDocs: clientes.xlsx
```

Puedes sostener que:

1. Se conectó un USB SanDisk.
2. Se le asignó la unidad `E:`.
3. El usuario `maria` interactuó con esa unidad.
4. Se navegó por `E:\Clientes`.
5. Hubo referencia reciente a `clientes.xlsx`.

No debes afirmar exfiltración sin más evidencia, pero sí hay indicios de acceso a datos en USB.

---

## 11.3 Persistencia

| Ubicación      | Hive         | Módulo                         |
| -------------- | ------------ | ------------------------------ |
| `HKLM\...\Run` | `SOFTWARE`   | `run`                          |
| `HKCU\...\Run` | `NTUSER.DAT` | `run`                          |
| Servicios      | `SYSTEM`     | `services`, `svc`              |
| Drivers        | `SYSTEM`     | `services`, plugins de drivers |
| AppInit DLLs   | `SOFTWARE`   | plugins específicos            |
| IFEO Debugger  | `SOFTWARE`   | plugins específicos            |

### Indicadores sospechosos

* Ejecutables en `AppData`, `Temp`, `ProgramData`.
* Nombres similares a Windows: `svhost.exe`, `scvhost.exe`, `winlogon32.exe`.
* Rutas con comillas malformadas.
* Servicios con descripción vacía.
* Binarios firmados de forma dudosa o no firmados.
* Autoruns creados cerca del momento del incidente.

---

## 11.4 Usuarios y cuentas

| Artefacto            | Hive         | Módulo              |
| -------------------- | ------------ | ------------------- |
| Usuarios locales     | `SAM`        | `samparse`, `users` |
| Grupos locales       | `SAM`        | `groups`            |
| Perfiles             | `SOFTWARE`   | `profilelist`       |
| Actividad de usuario | `NTUSER.DAT` | múltiples           |

### Preguntas que responde

* ¿Qué usuarios existían?
* ¿Qué usuarios tenían privilegios administrativos?
* ¿Qué perfiles iniciaron sesión?
* ¿Se creó una cuenta sospechosa?
* ¿Una cuenta fue habilitada o usada recientemente?

---

## 11.5 Red y movimiento lateral

| Artefacto              | Hive                 | Qué muestra       |
| ---------------------- | -------------------- | ----------------- |
| Interfaces             | `SYSTEM`             | IP, DHCP, DNS     |
| NetworkList            | `SOFTWARE` / usuario | Redes conocidas   |
| Terminal Server Client | `NTUSER.DAT`         | Destinos RDP      |
| TypedPaths             | `NTUSER.DAT`         | Rutas UNC         |
| RunMRU                 | `NTUSER.DAT`         | Comandos manuales |

Ejemplo de hallazgo:

```text
RunMRU: \\10.10.20.15\C$
RDP MRU: 10.10.20.22
TypedPaths: \\fileserver\hr
```

Interpretación: hay indicios de navegación o conexión manual hacia recursos internos.

---

# 12. Cómo ejecutar RegRipper en una investigación

## 12.1 Preparación de evidencia

Antes de analizar:

1. Trabaja sobre copias, no sobre evidencia original.
2. Calcula hash de los hives.
3. Documenta ruta de origen.
4. Preserva timestamps.
5. Considera transaction logs: `.LOG`, `.LOG1`, `.LOG2`.

Ejemplo de estructura:

```text
case001/
  evidence/
    SYSTEM
    SOFTWARE
    SAM
    SECURITY
    NTUSER_maria.DAT
    UsrClass_maria.dat
  reports/
  hashes/
```

---

## 12.2 Comandos básicos

### Analizar `SYSTEM`

```bash
regripper -r evidence/SYSTEM -f system > reports/system.txt
```

### Analizar `SOFTWARE`

```bash
regripper -r evidence/SOFTWARE -f software > reports/software.txt
```

### Analizar `SAM`

```bash
regripper -r evidence/SAM -f sam > reports/sam.txt
```

### Analizar `SECURITY`

```bash
regripper -r evidence/SECURITY -f security > reports/security.txt
```

### Analizar `NTUSER.DAT`

```bash
regripper -r evidence/NTUSER_maria.DAT -f ntuser > reports/ntuser_maria.txt
```

### Ejecutar plugin específico

```bash
regripper -r evidence/NTUSER_maria.DAT -p userassist > reports/userassist_maria.txt
```

### Ejecutar todos los plugins aplicables

```bash
regripper -r evidence/SYSTEM -a > reports/system_all.txt
```

La opción `-a` ejecuta automáticamente plugins aplicables al hive, y `-aT` ejecuta plugins de timeline TLN aplicables. ([GitHub][3])

---

# 13. Cómo leer un reporte de RegRipper

Un reporte suele tener bloques por plugin.

Ejemplo simplificado:

```text
Plugin: userassist
Hive: NTUSER.DAT

C:\Users\maria\Downloads\tool.exe
Last Run: 2025-10-14 21:33:12
Run Count: 2
```

## Preguntas de análisis

Para cada hallazgo, responde:

1. **¿Qué artefacto es?**
   Ejemplo: UserAssist.

2. **¿De qué hive proviene?**
   Ejemplo: `NTUSER.DAT` de `maria`.

3. **¿Qué usuario o sistema representa?**
   Ejemplo: perfil `C:\Users\maria`.

4. **¿Qué timestamp aparece?**
   Ejemplo: última ejecución aproximada.

5. **¿Qué significa realmente?**
   Ejemplo: ejecución o interacción del usuario.

6. **¿Qué NO significa?**
   Ejemplo: no prueba por sí solo exfiltración.

7. **¿Con qué se debe correlacionar?**
   Ejemplo: Prefetch, Amcache, eventos, EDR, Sysmon.

---

# 14. Módulos recomendados por objetivo de investigación

## 14.1 Sospecha de malware

Ejecuta:

```bash
regripper -r SYSTEM -p services
regripper -r SOFTWARE -p run
regripper -r NTUSER.DAT -p run
regripper -r NTUSER.DAT -p userassist
regripper -r SOFTWARE -p appcompatcache
```

Busca:

* Persistencia.
* Servicios raros.
* Binarios en rutas de usuario.
* Ejecutables descargados.
* Herramientas de ataque.

---

## 14.2 Exfiltración por USB

Ejecuta:

```bash
regripper -r SYSTEM -p usb
regripper -r SYSTEM -p usbstor
regripper -r SYSTEM -p mounteddevices
regripper -r NTUSER.DAT -p mountpoints2
regripper -r UsrClass.dat -p shellbags
regripper -r NTUSER.DAT -p recentdocs
```

Busca:

* Dispositivos USB.
* Letras asignadas.
* Usuario que accedió al USB.
* Carpetas exploradas.
* Documentos recientes relacionados.

---

## 14.3 Uso indebido de RDP

Ejecuta:

```bash
regripper -r NTUSER.DAT -p rdp
regripper -r NTUSER.DAT -p runmru
regripper -r NTUSER.DAT -p typedpaths
regripper -r SYSTEM -p interfaces
```

Busca:

* IPs o hosts en MRU de RDP.
* Comando `mstsc`.
* Rutas UNC.
* Redes usadas por el equipo.

---

## 14.4 Creación de cuentas sospechosas

Ejecuta:

```bash
regripper -r SAM -p samparse
regripper -r SAM -p users
regripper -r SAM -p groups
regripper -r SOFTWARE -p profilelist
```

Busca:

* Usuarios nuevos.
* Cuentas administrativas.
* Cuentas habilitadas recientemente.
* Perfiles de usuario inesperados.

---

## 14.5 Reconstrucción de actividad de usuario

Ejecuta:

```bash
regripper -r NTUSER.DAT -p userassist
regripper -r NTUSER.DAT -p recentdocs
regripper -r NTUSER.DAT -p runmru
regripper -r NTUSER.DAT -p typedpaths
regripper -r NTUSER.DAT -p typedurls
regripper -r UsrClass.dat -p shellbags
```

Busca:

* Programas abiertos.
* Archivos recientes.
* Comandos manuales.
* Carpetas visitadas.
* Rutas de red.
* Acceso a unidades externas.

---

# 15. Interpretación de timestamps

Los timestamps del Registro pueden venir de:

* LastWrite de clave.
* Valores internos de artefactos.
* Fechas FILETIME.
* Fechas Unix.
* Fechas codificadas por aplicación.

## Buenas prácticas

* Normaliza todo a UTC o a la zona horaria documentada.
* Conserva la hora original.
* Anota la fuente del timestamp.
* No mezcles LastWrite con “última ejecución” sin validar.
* Correlaciona con Event Logs, Prefetch, Amcache, SRUM, LNK y Jump Lists.

---

# 16. Limitaciones de RegRipper

RegRipper es potente, pero tiene límites.

## 16.1 No procesa automáticamente transaction logs

Si un hive tiene cambios pendientes en `.LOG1` o `.LOG2`, RegRipper no los fusiona automáticamente. Esto está documentado tanto en RegRipper 3.0 como en 4.0 y en páginas de manual de Linux. ([GitHub][4])

## 16.2 Depende de plugins

Si no existe plugin para una clave concreta, RegRipper no la interpretará automáticamente.

## 16.3 La ausencia de evidencia no es evidencia de ausencia

Que un plugin no muestre resultados no significa que el evento no ocurrió. Puede deberse a:

* Artefacto limpiado.
* Versión de Windows diferente.
* Hive incompleto.
* Plugin no compatible.
* Actividad registrada en otra fuente.

## 16.4 Algunos artefactos son ambiguos

Por ejemplo:

* ShimCache no siempre prueba ejecución.
* RecentDocs no siempre prueba apertura completa del archivo.
* Shellbags indican navegación, no necesariamente lectura de archivos.
* RunMRU indica comandos escritos, pero requiere correlación.

---

# 17. Plantilla de reporte forense

Puedes documentar hallazgos así:

```text
Hallazgo: Ejecución de herramienta sospechosa

Fuente:
- Hive: NTUSER.DAT
- Usuario: maria
- Plugin: userassist

Dato observado:
- Ruta: C:\Users\maria\Downloads\nmap.exe
- Run Count: 3
- Timestamp: 2025-11-04 14:22:10 UTC-5

Interpretación:
El artefacto UserAssist sugiere que el usuario interactuó o ejecutó nmap.exe desde su carpeta Downloads.

Limitaciones:
UserAssist por sí solo no prueba intención ni actividad de red. Debe correlacionarse con Prefetch, logs de firewall, Sysmon, EDR o eventos de red.

Severidad:
Media / Alta, según política interna.

Acción recomendada:
Correlacionar con logs de red y buscar otros binarios en Downloads.
```

---

# 18. Flujo recomendado de análisis

## Paso 1: Identificar hives

```text
SYSTEM
SOFTWARE
SAM
SECURITY
NTUSER.DAT por cada usuario
UsrClass.dat por cada usuario
```

## Paso 2: Ejecutar perfiles completos

```bash
regripper -r SYSTEM -f system > system.txt
regripper -r SOFTWARE -f software > software.txt
regripper -r SAM -f sam > sam.txt
regripper -r SECURITY -f security > security.txt
regripper -r NTUSER.DAT -f ntuser > ntuser.txt
```

## Paso 3: Ejecutar plugins específicos

Según hipótesis:

* Malware: `run`, `services`, `appcompatcache`, `userassist`
* USB: `usb`, `usbstor`, `mounteddevices`, `mountpoints2`, `shellbags`
* Usuario: `userassist`, `recentdocs`, `runmru`, `typedpaths`
* RDP: `rdp`, `runmru`, `typedpaths`
* Cuentas: `samparse`, `users`, `groups`, `profilelist`

## Paso 4: Correlacionar

No concluyas solo con RegRipper. Correlaciona con:

* Windows Event Logs.
* Prefetch.
* Amcache.
* ShimCache.
* SRUM.
* LNK files.
* Jump Lists.
* EDR.
* Antivirus.
* Proxy.
* Firewall.
* SIEM.

## Paso 5: Construir timeline

Usa timestamps de:

* LastWrite.
* UserAssist.
* USB.
* RecentDocs.
* Shellbags.
* RDP MRU.
* Instalaciones.
* Servicios.
* Cuentas.

---

# 19. Tabla rápida de módulos y utilidad

| Módulo           | Hive              | Sirve para                   |
| ---------------- | ----------------- | ---------------------------- |
| `compname`       | SYSTEM            | Nombre del equipo            |
| `timezone`       | SYSTEM            | Zona horaria                 |
| `select`         | SYSTEM            | ControlSet activo            |
| `services`       | SYSTEM            | Servicios y drivers          |
| `svc`            | SYSTEM            | Detalle de servicios         |
| `usb`            | SYSTEM            | Dispositivos USB             |
| `usbstor`        | SYSTEM            | Almacenamiento USB           |
| `mounteddevices` | SYSTEM            | Letras y volúmenes           |
| `interfaces`     | SYSTEM            | Configuración de red         |
| `winver`         | SOFTWARE          | Versión de Windows           |
| `uninstall`      | SOFTWARE          | Programas instalados         |
| `run`            | SOFTWARE / NTUSER | Persistencia                 |
| `appcompatcache` | SYSTEM / SOFTWARE | Rastros de ejecutables       |
| `profilelist`    | SOFTWARE          | Perfiles de usuario          |
| `samparse`       | SAM               | Usuarios locales             |
| `users`          | SAM               | Cuentas locales              |
| `groups`         | SAM               | Grupos locales               |
| `auditpol`       | SECURITY          | Política de auditoría        |
| `lsa`            | SECURITY          | Configuración LSA            |
| `userassist`     | NTUSER            | Programas ejecutados         |
| `recentdocs`     | NTUSER            | Documentos recientes         |
| `runmru`         | NTUSER            | Comandos Win+R               |
| `typedpaths`     | NTUSER            | Rutas escritas               |
| `typedurls`      | NTUSER            | URLs escritas                |
| `mountpoints2`   | NTUSER            | Unidades y recursos montados |
| `rdp`            | NTUSER            | Conexiones RDP               |
| `shellbags`      | UsrClass.dat      | Carpetas exploradas          |
| `muicache`       | NTUSER / UsrClass | Rutas y nombres de apps      |

---

# 20. Conclusión

RegRipper es una herramienta esencial para análisis forense del Registro de Windows. Su valor está en que automatiza la extracción de artefactos relevantes mediante plugins, permitiendo interpretar rápidamente hives como `SYSTEM`, `SOFTWARE`, `SAM`, `SECURITY`, `NTUSER.DAT` y `UsrClass.dat`.

La clave para usarlo correctamente es no limitarse a ejecutar perfiles, sino entender:

* Qué hive se está analizando.
* Qué plugin produjo el resultado.
* Qué significa realmente cada artefacto.
* Qué limitaciones tiene.
* Con qué otras fuentes debe correlacionarse.

Como regla práctica:

```text
SYSTEM      = sistema, hardware, servicios, red, USB
SOFTWARE    = programas, Windows, políticas, persistencia global
SAM         = usuarios y grupos locales
SECURITY    = políticas y secretos de seguridad
NTUSER.DAT  = actividad específica del usuario
UsrClass.dat = shellbags y navegación del Explorador
```

RegRipper no reemplaza el análisis forense completo, pero es una excelente herramienta para obtener rápidamente pistas accionables sobre actividad de usuarios, persistencia, dispositivos conectados, cuentas, software instalado y configuración del sistema.

[1]: https://www.kali.org/tools/regripper/ "regripper | Kali Linux Tools"
[2]: https://forensics.wiki/regripper/?utm_source=chatgpt.com "Regripper -"
[3]: https://github.com/keydet89/RegRipper3.0 "GitHub - keydet89/RegRipper3.0: RegRipper3.0 · GitHub"
[4]: https://github.com/keydet89/RegRipper4.0 "GitHub - keydet89/RegRipper4.0: RegRipper4.0 · GitHub"


## Actividad en clase.

Completar la sesión **Laboratorio**  del sitipo  https://dvirus.training/2020/03/30/analisis-del-registro-de-windows-con-regripper/




rip.exe -r software -p uninstall > F:\ESCUELA_GUERRA\2026\FORENSE\install.txt


---

# Referencias.

- https://keepcoding.io/blog/como-funciona-el-registro-de-windows/
- https://www.dragonjar.org/laboratorio-informatica-forense-analisis-del-registro-de-windows.xhtml
