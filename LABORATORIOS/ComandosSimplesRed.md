# Guía de Comandos de Red Básicos en Windows y Linux

## 1. Comandos Básicos de Red en Windows

### 1.1 `ipconfig`

**Descripción**: El comando `ipconfig` se usa para mostrar la configuración IP de las interfaces de red en una computadora Windows. Es útil para verificar la dirección IP, máscara de subred, puerta de enlace y otros detalles de red.

**Sintaxis**:
```bash
ipconfig
```

**Ejemplo de uso**:
1. Para ver la configuración básica de red:
   ```bash
   ipconfig
   ```
   Esto mostrará la dirección IP, máscara de subred y puerta de enlace predeterminada de cada adaptador de red.

2. Para mostrar detalles adicionales (DNS, DHCP, etc.):
   ```bash
   ipconfig /all
   ```

### 1.2 `ping`

**Descripción**: `ping` verifica la conectividad con otro dispositivo en la red enviando paquetes ICMP. Permite comprobar si otro dispositivo está disponible y midiendo el tiempo de respuesta.

**Sintaxis**:
```bash
ping <dirección IP o dominio>
```

**Ejemplo de uso**:
1. Para verificar la conectividad con Google:
   ```bash
   ping google.com
   ```

2. Para enviar un número específico de paquetes:
   ```bash
   ping -n 4 google.com
   ```
   En este caso, enviará 4 paquetes en lugar de los predeterminados.

### 1.3 `tracert`

**Descripción**: `tracert` (traceroute en Linux) muestra la ruta que toman los paquetes para llegar a un destino. Es útil para diagnosticar problemas de conectividad y para ver el camino que toman los datos a través de la red.

**Sintaxis**:
```bash
tracert <dirección IP o dominio>
```

**Ejemplo de uso**:
1. Para ver la ruta hacia el servidor de Google:
   ```bash
   tracert google.com
   ```

### 1.4 `netstat`

**Descripción**: `netstat` muestra información sobre las conexiones de red activas, incluyendo las direcciones IP conectadas y los puertos que están en uso.

**Sintaxis**:
```bash
netstat
```

**Ejemplo de uso**:
1. Para ver todas las conexiones activas:
   ```bash
   netstat -a
   ```

2. Para mostrar conexiones activas y el proceso que las generó:
   ```bash
   netstat -b
   ```

### 1.5 `nslookup`

**Descripción**: `nslookup` realiza consultas al servidor DNS y se usa para obtener información sobre una dirección IP o nombre de dominio.

**Sintaxis**:
```bash
nslookup <nombre de dominio o IP>
```

**Ejemplo de uso**:
1. Para obtener la IP de google.com:
   ```bash
   nslookup google.com
   ```

2. Para realizar consultas DNS específicas:
   ```bash
   nslookup -type=mx google.com
   ```
   Esto muestra los registros de correo (MX) asociados a Google.

---

## 2. Comandos Básicos de Red en Linux

### 2.1 `ifconfig`

**Descripción**: `ifconfig` (similar a `ipconfig` en Windows) muestra la configuración de red de las interfaces en Linux. Es útil para ver la dirección IP, máscara de subred y estado de las interfaces de red.

**Sintaxis**:
```bash
ifconfig
```

**Ejemplo de uso**:
1. Para mostrar la configuración de todas las interfaces:
   ```bash
   ifconfig
   ```

2. Para ver solo una interfaz específica (por ejemplo, `eth0`):
   ```bash
   ifconfig eth0
   ```

### 2.2 `ping`

**Descripción**: `ping` se utiliza de la misma manera en Linux que en Windows. Permite verificar la conectividad con otro dispositivo.

**Sintaxis**:
```bash
ping <dirección IP o dominio>
```

**Ejemplo de uso**:
1. Para verificar la conectividad con Google:
   ```bash
   ping google.com
   ```

2. Para limitar el número de paquetes enviados:
   ```bash
   ping -c 4 google.com
   ```
   En este caso, enviará 4 paquetes.

### 2.3 `traceroute`

**Descripción**: `traceroute` (similar a `tracert` en Windows) muestra la ruta que toman los paquetes para llegar a un destino.

**Sintaxis**:
```bash
traceroute <dirección IP o dominio>
```

**Ejemplo de uso**:
1. Para ver la ruta hacia el servidor de Google:
   ```bash
   traceroute google.com
   ```

### 2.4 `netstat`

**Descripción**: `netstat` en Linux muestra estadísticas de la red, incluyendo conexiones activas y las interfaces de red.

**Sintaxis**:
```bash
netstat
```

**Ejemplo de uso**:
1. Para ver todas las conexiones activas:
   ```bash
   netstat -a
   ```

2. Para ver el estado de las conexiones de escucha:
   ```bash
   netstat -l
   ```

3. Para listar solo conexiones TCP activas:
   ```bash
   netstat -t
   ```

### 2.5 `dig`

**Descripción**: `dig` es similar a `nslookup` en Windows. Permite realizar consultas DNS y obtener información detallada sobre nombres de dominio.

**Sintaxis**:
```bash
dig <nombre de dominio>
```

**Ejemplo de uso**:
1. Para consultar la IP de google.com:
   ```bash
   dig google.com
   ```

2. Para obtener solo la respuesta corta:
   ```bash
   dig google.com +short
   ```

3. Para consultar registros MX:
   ```bash
   dig google.com MX
   ```

---

