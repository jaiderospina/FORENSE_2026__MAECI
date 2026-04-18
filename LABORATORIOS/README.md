### HASH

## 1. **Cálculo de Hash desde CMD (Símbolo del sistema)**

### Paso 1: Usar `certutil`
El comando `certutil` en Windows permite calcular los hashes de archivos de manera fácil. A continuación se muestran algunos ejemplos.

#### **Sintaxis Básica:**
```cmd
certutil -hashfile <ruta_del_archivo> <tipo_de_hash>
```
Donde:
- `<ruta_del_archivo>` es el archivo del que deseas obtener el hash.
- `<tipo_de_hash>` es el algoritmo de hash (MD5, SHA1, SHA256, etc.).

#### Ejemplo para obtener el hash MD5 de un archivo:
```cmd
certutil -hashfile C:\ruta\a\tu\archivo.txt MD5
```

#### Ejemplo para obtener el hash SHA-1 de un archivo:
```cmd
certutil -hashfile C:\ruta\a\tu\archivo.txt SHA1
```

#### Ejemplo para obtener el hash SHA-256 de un archivo:
```cmd
certutil -hashfile C:\ruta\a\tu\archivo.txt SHA256
```

**Salida esperada:**
El comando devolverá el hash correspondiente en el terminal. El resultado será algo así:

```
SHA256 hash(es) for file C:\ruta\a\tu\archivo.txt:
<hash>
CertUtil: -hashfile comando completado correctamente.
```

---

## 2. **Cálculo de Hash desde PowerShell**

### Paso 1: Usar el cmdlet `Get-FileHash`
PowerShell tiene un cmdlet nativo llamado `Get-FileHash` que permite calcular el hash de un archivo de manera similar al uso de `certutil`, pero con más flexibilidad en la salida.

#### **Sintaxis Básica:**
```powershell
Get-FileHash <ruta_del_archivo> -Algorithm <tipo_de_hash>
```

Donde:
- `<ruta_del_archivo>` es el archivo del que deseas obtener el hash.
- `<tipo_de_hash>` es el algoritmo de hash (MD5, SHA1, SHA256, etc.).

#### Ejemplo para obtener el hash MD5 de un archivo:
```powershell
Get-FileHash C:\ruta\a\tu\archivo.txt -Algorithm MD5
```

#### Ejemplo para obtener el hash SHA-1 de un archivo:
```powershell
Get-FileHash C:\ruta\a\tu\archivo.txt -Algorithm SHA1
```

#### Ejemplo para obtener el hash SHA-256 de un archivo:
```powershell
Get-FileHash C:\ruta\a\tu\archivo.txt -Algorithm SHA256
```

**Salida esperada:**
La salida mostrará el hash de la siguiente manera:

```
Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          <hash>                                                                 C:\ruta\a\tu\archivo.txt
```

### Paso 2: Otros algoritmos de hash
Puedes usar otros algoritmos como SHA512, SHA384, o SHA-256. Aquí un ejemplo con SHA512:

```powershell
Get-FileHash C:\ruta\a\tu\archivo.txt -Algorithm SHA512
```

---

## 3. **Notas Adicionales**

- **Certutil**: Está disponible por defecto en todas las versiones modernas de Windows a partir Windows 7.
- **Get-FileHash**: Es un cmdlet de PowerShell que está disponible desde PowerShell 4.0 y versiones posteriores, por lo que necesitarás tener una versión reciente de PowerShell (puedes verificarlo con el comando `$PSVersionTable.PSVersion`).
- **Verificación de integridad**: Puedes usar estos hashes para verificar la integridad de los archivos comparando el hash calculado con el proporcionado por la fuente original.

---
## 4. Utilidades

Varias herramientas útiles, entre ellas HashCompare. Esta herramienta permite el cáculo y rápida comparación del hash de dos ficheros. 


https://securityxploded.com/download-software.php?name=hashcompare
