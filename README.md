# Proyecto-Digital
Sensor de medida del agua 
En el presente repositorio se expondrá en que conssitió el proyecto realizado en la aignatura de electrónica digital, mostrando el paso a paso realizado

- Juan David Gomez 
- David Leonardo Botia
- Yuliana
  
Bienvenidos a nuestro repositorio del proyecto final de nuestra clase de electrónica digital de la Universidad Nacional de Colombia del semestre 2025-I, el cual consistía en el diseño y posterior implementación de un Sensor para la medicion de agua, realizando una versión digital de este mismo mediante el uso de sensores ultrasónicos, FPGA y ESP32.
##
🎯 Objetivo General

🛠️ Diseñar e implementar un sistema de medición de nivel de agua en tiempo real, utilizando un sensor ultrasónico HC-SR04, un microcontrolador ESP32 para la adquisición y transmisión de datos, y una FPGA para el procesamiento y control de señales digitales, con el fin de generar un prototipo funcional y escalable para aplicaciones de monitoreo.
##
🎯 Objetivos Específicos

📏 Diseñar el sistema de adquisición de datos

- Utilizar el sensor HC-SR04 con el ESP32 para medir la distancia hasta la superficie del agua.

🧠 Implementar el procesamiento digital en FPGA

- Gestionar el pulso de disparo y la medición del eco del sensor mediante lógica digital.

📲 Integrar interfaz de usuario o canal de salida

- Mostrar el nivel de agua en una app o tablero vía Bluetooth usando el ESP32.

🔍 Verificar y validar el sistema completo

- Probar el prototipo con distintos niveles de agua usando simulaciones y mediciones reales.

📚 Documentar todo el proceso de desarrollo

- Incluir diagramas, requerimientos, códigos, bitácoras y resultados del proyecto.

##
![Imagen de WhatsApp 2025-07-20 a las 21 04 23_a6fc7017](https://github.com/user-attachments/assets/d2cbcf3b-8a8b-4d64-a418-706c8ba06e98)
##
✅ Requerimientos Funcionales

🧭 Medición de distancia usando el sensor ultrasónico
   - El sensor ultrasónico HC-SR04 mide la distancia al nivel de agua mediante pulsos de eco.
     
⚙️Procesamiento digital de la señal de tiempo de retorno en una FPGA.
   - La FPGA calcula el tiempo del eco y convierte el dato en una señal digital interpretable.
     
💧 Cálculo del nivel de agua y generación de alerta si es bajo o alto..
   - Se transforma la distancia en una medida de nivel y se compara con umbrales definidos.
     
📡 Transmisión de datos desde la FPGA al ESP32 mediante UART.
   - La FPGA envía el dato procesado al ESP32 mediante comunicación serial.
     
🌐 Comunicación del dato medido a un servidor o aplicación.
   - El ESP32 transmite el nivel de agua a una aplicación via bluetooth (ESP32)
     
🧪 Sistema implementado 
   - Se revisa el hardware y se verifica  mediante simulación.
##
Diagrama ASM/ Maquina de estados/ diagramas funcionales:
##
Diagrama RTL del SoC y su mòdulo:
##
Simulaciones:
##
Video simulacion:
##
Logs de make log-prn, make log-syn diagramas de flujo 
##
¿Còmo interactùa con entornos externos?
##
Video del proyecto
##
