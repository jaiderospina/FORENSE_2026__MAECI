# Forense_2024
```
"Hay ciertas pistas en la escena de un crimen que por su naturaleza nadie puede recoger o examinar
¿cómo se recoge el Amor, la Ira, el Odio, el Miedo…?
son cosas que hay que saber buscar” 

Dr. James T Reese.  

```

![avatar](Avatar_JAIDER.png)
---
La **informática forense** es la disciplina científica y técnica que se encarga de la identificación, preservación, extracción, análisis y documentación de evidencias digitales presentes en dispositivos informáticos, sistemas o redes. Su objetivo principal es asegurar que dicha evidencia sea tratada de forma que tenga **validez legal** y pueda ser presentada ante una corte o proceso administrativo, manteniendo la **cadena de custodia** intacta. Así como en procesos de gestión del "día a día" de una organización.

En esencia, busca responder a las preguntas: ¿qué pasó?, ¿quién lo hizo?, ¿cómo ocurrió?, ¿cuándo sucedió? y ¿qué datos fueron comprometidos?


---

### Campos de aplicación

La informática forense abarca diversos ámbitos dependiendo del origen del dato y el objetivo de la investigación:

* **Forense de dispositivos (Disk/Mobile Forensics):** Recuperación y análisis de datos en discos duros, unidades extraíbles, teléfonos inteligentes y tablets. Incluye la recuperación de archivos borrados o fragmentos de datos en espacios no asignados.
* **Forense de red (Network Forensics):** Monitoreo y análisis del tráfico de red para identificar intrusiones, exfiltración de datos o comportamiento anómalo. Se centra en el análisis de paquetes, logs de firewalls y sistemas de detección de intrusos (IDS/IPS).
* **Forense de memoria (RAM Forensics):** Análisis de la memoria volátil para detectar procesos maliciosos, claves de cifrado o rastros de malware que no se escriben en el disco duro (técnicas *fileless*).
* **Forense en la nube (Cloud Forensics):** Desafío creciente que implica obtener evidencia de entornos virtualizados y servicios en la nube donde el investigador no tiene acceso físico al hardware.
* **Forense de sistemas embebidos e IoT:** Aplicada a dispositivos como cámaras IP, sensores industriales o sistemas de control (SCADA), crucial en la protección de infraestructuras críticas.



---

### Importancia en la Ciberseguridad

La informática forense es el pilar fundamental del ciclo de vida de la ciberseguridad, transformándose de una herramienta reactiva a una proactiva:

1.  **Lecciones aprendidas y endurecimiento:** Al analizar cómo ocurrió un ataque, las organizaciones pueden parchear vulnerabilidades específicas y ajustar sus defensas para prevenir la recurrencia del incidente (*root cause analysis*).
2.  **Mitigación de daños y respuesta a incidentes:** Permite contener el impacto de una brecha al identificar qué activos fueron afectados, limitando la propagación del malware o el robo de información.
3.  **Disuasión:** La capacidad de investigar y atribuir ataques genera un efecto disuasorio frente a actores maliciosos, demostrando que las acciones en el entorno digital dejan rastros rastreables.
4.  **Cumplimiento legal y normativo:** En sectores altamente regulados (como el financiero o energético, donde tú colaboras), la capacidad forense es a menudo un requisito normativo para demostrar el debido proceso tras una brecha de seguridad.

En resumen, mientras la ciberseguridad se enfoca en *evitar* el incidente, la informática forense es lo que *garantiza el conocimiento* necesario para evolucionar nuestra postura de defensa frente a amenazas cada vez más sofisticadas.



## Tipos de análisis forense.

- <b>Análisis post-mortem:</b>  Este análisis se realiza con un equipo dedicado específicamente para fines forenses para examinar discos duros, datos o cualquier tipo de información recabada de un sistema que ha sufrido un incidente. En este caso, las herramientas de las que podemos disponer son aquellas que tengamos en nuestro laboratorio para el análisis de discos duros, archivos de logs de firewalls o IDS, etc. 

- <b>Análisis en caliente:</b>  Se lleva a cabo en sistema que se presume a sufrido un incidente o está sufriendo un incidente de seguridad. En este caso, se suele emplear un CD con las herramientas de respuesta ante incidentes y análisis forense compiladas de forma que no realicen modificaciones en el sistema. Una vez hecho este análisis en caliente, y confirmado el incidente, se realiza el análisis post-mortem.








## Material de ayuda

- Repositorio de imágenes forenses "Digital Forensics Tool Testing Images" https://dftt.sourceforge.net/
- Markdown https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
- Markdown 2.   https://markdown.es/sintaxis-markdown/
- Docker https://www.docker.com/products/docker-hub/
- Docker School   https://www.w3schools.io/docker-tutorials/
- FOCA  https://telefonicatech.com/blog/como-analizar-documentos-con-foca
