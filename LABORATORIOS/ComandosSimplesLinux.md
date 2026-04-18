# Manual de Comandos Básicos en Terminal de Linux

Este manual está diseñado para guiar al usuario en el uso de los comandos básicos desde la terminal en un entorno Linux. 
---

## **1. Navegación del Sistema de Archivos**

### **1.1. Cambiar de Directorio**
**Comando:** `cd [ruta]`  
- El usuario utiliza este comando para moverse entre directorios.  
  **Ejemplo:**  
  ```bash
  cd /home/usuario/Documentos
  ```
  
- Para regresar al directorio anterior:  
  ```bash
  cd -
  ```

### **1.2. Listar Contenido**
**Comando:** `ls [opciones]`  
- Este comando lista el contenido de un directorio.  
  **Ejemplo:**  
  ```bash
  ls -l
  ls -a
  ```

### **1.3. Mostrar el Directorio Actual**
**Comando:** `pwd`  
- Muestra la ruta completa del directorio en el que se encuentra el usuario.  
  **Ejemplo:**  
  ```bash
  pwd
  ```

---

## **2. Gestión de Archivos y Directorios**

### **2.1. Crear un Directorio**
**Comando:** `mkdir [nombre]`  
- Crea un directorio nuevo en la ruta actual.  
  **Ejemplo:**  
  ```bash
  mkdir Proyectos
  ```

### **2.2. Eliminar un Archivo o Directorio**
**Comando:** `rm [opciones] [nombre]`  
- Elimina archivos o directorios.  
  **Ejemplo:**  
  ```bash
  rm archivo.txt       # Elimina un archivo
  rm -r carpeta        # Elimina un directorio y su contenido
  ```

### **2.3. Copiar Archivos o Directorios**
**Comando:** `cp [origen] [destino]`  
- Copia archivos o directorios a una nueva ubicación.  
  **Ejemplo:**  
  ```bash
  cp archivo.txt /ruta/destino
  cp -r carpeta /ruta/destino
  ```

### **2.4. Mover o Renombrar Archivos**
**Comando:** `mv [origen] [destino]`  
- Mueve archivos o directorios o los renombra.  
  **Ejemplo:**  
  ```bash
  mv archivo.txt /ruta/destino
  mv archivo.txt archivo_renombrado.txt
  ```

---

## **3. Gestión de Permisos**

### **3.1. Cambiar Permisos**
**Comando:** `chmod [modo] [archivo]`  
- Modifica los permisos de un archivo o directorio.  
  **Ejemplo:**  
  ```bash
  chmod 755 script.sh
  ```

### **3.2. Cambiar Propietario**
**Comando:** `chown [usuario:grupo] [archivo]`  
- Cambia el propietario de un archivo o directorio.  
  **Ejemplo:**  
  ```bash
  chown usuario:grupo archivo.txt
  ```

---

## **4. Información del Sistema**

### **4.1. Ver Espacio en Disco**
**Comando:** `df -h`  
- Muestra el espacio disponible en el disco.  

### **4.2. Ver Memoria Disponible**
**Comando:** `free -h`  
- Muestra el uso de memoria RAM y de intercambio.  

---

## **5. Gestión de Procesos**

### **5.1. Ver Procesos Activos**
**Comando:** `ps aux`  
- Muestra los procesos en ejecución del sistema.  

### **5.2. Finalizar un Proceso**
**Comando:** `kill [PID]`  
- Finaliza un proceso mediante su identificador (PID).  
  **Ejemplo:**  
  ```bash
  kill 1234
  ```

### **5.3. Comando Avanzado: htop**
- Si está instalado, permite gestionar procesos con una interfaz interactiva.  
  **Ejemplo:**  
  ```bash
  htop
  ```

---

## **6. Red y Conexiones**

### **6.1. Ver Dirección IP**
**Comando:** `ip addr show`  
- Muestra las direcciones IP configuradas en el sistema.  

### **6.2. Probar Conectividad**
**Comando:** `ping [dominio]`  
- Verifica si un dominio o dirección IP es accesible.  
  **Ejemplo:**  
  ```bash
  ping google.com
  ```

---

## **7. Descarga y Gestión de Archivos**

### **7.1. Descargar Archivos**
**Comando:** `wget [URL]`  
- Descarga un archivo desde la web.  
  **Ejemplo:**  
  ```bash
  wget http://ejemplo.com/archivo.zip
  ```

### **7.2. Extraer Archivos Comprimidos**
- **ZIP:**  
  ```bash
  unzip archivo.zip
  ```
- **TAR.GZ:**  
  ```bash
  tar -xvzf archivo.tar.gz
  ```

---

