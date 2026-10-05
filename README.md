# Documentación del Flujo: Automatización de Reporte del Clima (Bucaramanga)

##  Descripción General

Este flujo automatizado consulta en tiempo real las condiciones meteorológicas de Bucaramanga mediante una API pública, registra los datos históricos en una hoja de cálculo en la nube, y distribuye notificaciones automáticas tanto por correo electrónico (Gmail) como por mensajería instantánea (Telegram).

---

## Componentes y Arquitectura del Flujo

El flujo consta de 5 nodos conectados secuencialmente en n8n:

1. **Node 1: Disparador Manual (`When clicking 'Execute workflow'`)**
* **Propósito:** Inicia la ejecución del flujo a petición del usuario mediante un botón de prueba en el lienzo de n8n.




2. **Node 2: Petición HTTP (`HTTP Request`)**
* **Propósito:** Realiza una petición `GET` a la API de **Open-Meteo** para obtener los datos meteorológicos actuales de Bucaramanga (coordenadas de latitud y longitud configuradas).


* **Datos extraídos clave:** Temperatura actual (`temperature_2m`), humedad relativa (`relative_humidity_2m`) y código del clima (`weather_code`).




3. **Node 3: Registro en Google Sheets (`Append row in sheet`)**
* **Propósito:** Inserta de forma automática una nueva fila en una hoja de Google Drive (`Resumen diario (clima/noticias)`) con los valores extraídos de la API.


* **Columnas mapeadas:** *Temperatura*, *Humedad* y *Codigo_Clima*.




4. **Node 4: Envío de Correo (`Send a message` - Gmail)**
* **Propósito:** Envía una notificación formal a la bandeja de entrada del usuario (`fellpechatgpt74@gmail.com`) con el resumen de las condiciones climáticas actuales utilizando un asunto personalizado ("Resumen Diario del Clima").




5. **Node 5: Alerta de Telegram (`Send a text message`)**
* **Propósito:** Envía una notificación instantánea al chat personal de Telegram del usuario mediante un bot personalizado (`@ClimaBucaramangaBot`).


* **Configuración técnica:**
* **Bot Token:** Autenticación generada a través de *BotFather*.
* **Chat ID:** Identificador numérico personal del usuario (`8801824603`) obtenido mediante la consulta a la API de Telegram (`getUpdates`).


* **Cuerpo del mensaje:** Muestra de forma dinámica la temperatura y la humedad en tiempo real usando expresiones de n8n vinculadas al nodo HTTP/Sheets.





---

## Resumen del Proceso de Ejecución

Al ejecutar el flujo, n8n consulta la API de Open-Meteo, guarda el registro persistente en Google Sheets para mantener un histórico, y de manera simultánea despacha el reporte al correo electrónico y al chat de Telegram del usuario, cerrando un ciclo de automatización de punta a punta exitoso.