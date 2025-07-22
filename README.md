# Proyecto-Digital
**Sensor de medida del agua**

En el presente repositorio se expondrá en que conssitió el proyecto realizado en la aignatura de electrónica digital, mostrando el paso a paso realizado

- Juan David Gomez 
- David Leonardo Botia
- Yuliana
  
Bienvenidos a nuestro repositorio del proyecto final de nuestra clase de electrónica digital de la Universidad Nacional de Colombia del semestre 2025-I, el cual consistía en el diseño y posterior implementación de un Sensor para la medicion de agua, realizando una versión digital de este mismo mediante el uso de sensores ultrasónicos, FPGA y ESP32.
##
Objetivo general 

- Diseñar e implementar un sistema de medición de nivel de agua en tiempo real, utilizando un sensor ultrasónico HC-SR04, un microcontrolador ESP32 para la adquisición y transmisión de datos, y una FPGA para el procesamiento y control de señales digitales, con el fin de generar un prototipo funcional y escalable para aplicaciones de monitoreo

##
Objetivos especificos

- Diseñar el sistema de adquisición de datos del nivel de agua mediante el sensor ultrasónico HC-SR04, acoplado al ESP32 para capturar la distancia entre el sensor y la superficie del agua.
- Implementar el procesamiento digital de señales en la FPGA para gestionar el control del pulso de disparo (trigger) y la lectura precisa del pulso de eco del sensor HC-SR04.
- Integrar una interfaz de usuario o un canal de salida ( WI-FI usando el ESP32) para mostrar el nivel de agua en una aplicación o dashboard.
- Verificar y validar el sistema completo mediante simulaciones y pruebas físicas del prototipo en diferentes niveles de agua para asegurar la precisión y confiabilidad del sistema.
- Documentar todo el proceso de desarrollo, incluyendo requerimientos funcionales, diagramas ASM, códigos HDL y de microcontrolador, bitácoras de pruebas, y resultados experimentales.


##
![Imagen de WhatsApp 2025-07-20 a las 21 04 23_a6fc7017](https://github.com/user-attachments/assets/d2cbcf3b-8a8b-4d64-a418-706c8ba06e98)

El monitoreo del nivel de agua es esencial para la automatización en riegos, acueductos, y control de fluidos en general. Este proyecto busca integrar tecnologías digitales con procesamiento distribuido para ofrecer una solución educativa y funcional para este tipo de monitoreo.

La arquitectura implementa una solución SoC híbrida, donde la **FPGA realiza la temporización de señales críticas**, y el **ESP32 actúa como nodo de comunicación inteligente**, gestionando el envío de datos por red.
##
**REQUERMIENTOS DEL PROYECTO**
##
**✅ Requerimientos Funcionales**

**🧭 Medición de distancia usando el sensor ultrasónico**
   - El sensor ultrasónico HC-SR04 mide la distancia al nivel de agua mediante pulsos de eco.
     
**⚙️Procesamiento digital de la señal de tiempo de retorno en una FPGA.**
   - La FPGA calcula el tiempo del eco y convierte el dato en una señal digital interpretable.
     
**💧 Cálculo del nivel de agua y generación de alerta si es bajo o alto.**
   - Se transforma la distancia en una medida de nivel y se compara con umbrales definidos.
     
**📡 Transmisión de datos desde la FPGA al ESP32 mediante UART.**
   - La FPGA envía el dato procesado al ESP32 mediante comunicación serial.
     
**🌐 Comunicación del dato medido a un servidor o aplicación.**
   - El ESP32 transmite el nivel de agua a una aplicación via WI-FI (ESP32)
     
**🧪 Sistema implementado**
   - Se revisa el hardware y se verifica  mediante simulación.

##
**✅ Requerimientos No Funcionales**

**⏱️ Tiempo de respuesta adecuado**
- El sistema debe responder a los cambios en el nivel de agua..

**🔁 Confiabilidad operativa**
- El sistema debe funcionar de manera continua y precisa sin fallas durante largos periodos.

**📈 Simulación funcional**
- Compatible con simuladores como Icarus Verilog + GTKWave.
  
**📝 Código documentado**
- Cada módulo está comentado para facilitar su comprensión y mantenimiento.
##
***Módulos Verilog**
| Archivo               | Función                                                                 |
|-----------------------|-------------------------------------------------------------------------|
| `SOC.v`               |  Módulo principal del SoC, conecta CPU, memoria y periféricos.          |
| `top_tb.v`            |  Testbenches para simular comportamiento                                |
| `address_decoder.v`   |  Decodifica direcciones para acceso a memoria y periféricos.            |
| `bench_quark.v`       |  Banco de pruebas para la CPU (núcleo tipo Quark).                      |
| `chip_select.v`       |  Controla la habilitación de módulos según la dirección.                |  
| `uart_tx.v`           |  Envía datos de estado por UART a microcontrolador o dispotivo.         | 
|   mult.v	            |  Módulo principal del multiplicador.                                    |

##
##
Diagrama ASM/ Maquina de estados/ diagramas funcionales:

**Diagrama general**

![Imagen de WhatsApp 2025-07-21 a las 01 15 02_4c9b3a75](https://github.com/user-attachments/assets/cb752654-1489-4428-83fa-19eb750f984f)

**Diagrama del sensor**

![Imagen de WhatsApp 2025-07-21 a las 01 13 21_2349670d](https://github.com/user-attachments/assets/9885896b-79a2-40b0-9d4e-b2cefa933da2)

**Maquinas de Estado**



**MAPAS DIAGRAMA GENERAL**

# Máquina de Estados Finita (FSM) del Sistema de Control de Flujo

Este diagrama representa el flujo de control del sistema de medición de nivel de agua y activación de bomba, basado en el sensor ultrasónico HC-SR04 y una señal de calidad externa.

```mermaid
stateDiagram-v2

    [*] --> S0_Inicio
    S0_Inicio --> S1_ConfigurarHardware : iniciar sistema
    S1_ConfigurarHardware --> S2_LeerEntradas : hardware listo
    S2_LeerEntradas --> S3_EsperarConsulta

    S3_EsperarConsulta --> S2_LeerEntradas : consulta != 1
    S3_EsperarConsulta --> S4_Trigger : consulta == 1

    S4_Trigger --> S5_Echo
    S5_Echo --> S6_EvaluarNivel

    S6_EvaluarNivel --> S8_Flujo0 : con_in < umbral
    S6_EvaluarNivel --> S7_EvaluarCalidad : con_in >= umbral

    S7_EvaluarCalidad --> S8_Flujo1 : calidad == 1
    S7_EvaluarCalidad --> S8_Flujo0 : calidad != 1

    S8_Flujo0 --> S9_SalidaFlujo
    S8_Flujo1 --> S9_SalidaFlujo

    S9_SalidaFlujo --> S2_LeerEntradas : ciclo continuo
```

---

### Estados:

- **S0_Inicio**: Estado inicial del sistema.
- **S1_ConfigurarHardware**: Configuración del hardware.
- **S2_LeerEntradas**: Recolección de datos de entrada.
- **S3_EsperarConsulta**: Espera de señal desde el sistema central.
- **S4_Trigger**: Enviar pulso de disparo al sensor HC-SR04.
- **S5_Echo**: Lectura de tiempo de respuesta del sensor.
- **S6_EvaluarNivel**: Comparación entre "con_in" y "umbral".
- **S7_EvaluarCalidad**: Verifica si la calidad del agua es aceptable.
- **S8_Flujo0**: Desactivar bomba (flujo = 0).
- **S8_Flujo1**: Activar bomba (flujo = 1).
- **S9_SalidaFlujo**: Salida de control de flujo.

---

Este FSM se implementa a una ESP32 y forma parte de sistemas automatizados de control de nivel de agua.



# ⚙️ Diagrama Funcional del Sistema de Control de Agua

```mermaid
flowchart TD
    %% ========== ENTRADAS ==========
    SistemaCentral([Sistema Central]) -->|"calidad (1/0)"| Microcontrolador
    Usuario[[Interfaz Usuario]] -->|"consulta (1/0)"| Microcontrolador
    
    %% ========== PROCESAMIENTO ==========
    subgraph Microcontrolador["Microcontrolador (Procesamiento)"]
        Programa[["Programa Principal"]]
        Comparador{Comparador}
        
        Programa -->|"con_in"| Comparador
        SistemaCentral -->|"calidad"| Programa
    end
    
    %% ========== SENSORES ==========
    subgraph Sensores["🔍 Sensores"]
        Ultrasónico[["HC-SR04\nTrigger/Echo"]] -->|"Distancia (cm)"| Programa
    end
    
    %% ========== ACTUADORES ==========
    subgraph Actuadores["⚡ Actuadores"]
        Bomba[["Bomba de Agua\nFLUJO=1/0"]] <--> Comparador
    end
    
    %% ========== CONEXIONES CRÍTICAS ==========
    Ultrasónico -.->|Interrupción| Programa
    Comparador -->|"FLUJO=1\n(Cumple condiciones)"| Bomba
    Comparador -->|"FLUJO=0\n(Blqueo por calidad)"| Bomba
    
    %% ========== ESTILOS ==========
    classDef entrada fill:#e6ffe6,stroke:#4CAF50
    classDef procesamiento fill:#e6f3ff,stroke:#2196F3
    classDef sensor fill:#fff2e6,stroke:#FF9800
    classDef actuador fill:#ffe6e6,stroke:#F44336
    
    class SistemaCentral,Usuario entrada
    class Microcontrolador,Programa,Comparador procesamiento
    class Ultrasónico sensor
    class Bomba actuador
    
    %% ========== LEYENDA ==========
    linkStyle 0,1 stroke:#4CAF50,stroke-width:2px
    linkStyle 2,3 stroke:#FF9800,stroke-width:2px
    linkStyle 4,5 stroke:#F44336,stroke-width:2px
```
##
**Estados**

- Espera de Consulta: El sistema permanece en espera de una señal de consulta (desde el usuario o el sistema central).
- Medición de Distancia: Se activa el sensor HC-SR04 para medir la distancia al nivel del agua.
- Procesamiento de Datos: El microcontrolador recibe la distancia y evalúa la señal de calidad del agua.
- Comparación de Condiciones: Se compara si la distancia medida y la calidad cumplen los requisitos.
- Decisión de Flujo:
   FLUJO = 1: Se activa la bomba de agua.
  FLUJO = 0: Se bloquea el sistema por incumplimiento de condiciones.




**MAPAS DEL CONTROL DEL AGUA**

# Máquina de Estados Finita (FSM) del Sistema de Control de Flujo

Este diagrama representa el flujo de control del sistema de medición de nivel de agua y activación de bomba, basado en el sensor ultrasónico HC-SR04 y una señal de calidad externa.

```mermaid
stateDiagram-v2

    [*] --> S0_Inicio
    S0_Inicio --> S1_ConfigurarHardware : iniciar sistema
    S1_ConfigurarHardware --> S2_LeerEntradas : hardware listo
    S2_LeerEntradas --> S3_EsperarConsulta

    S3_EsperarConsulta --> S2_LeerEntradas : consulta != 1
    S3_EsperarConsulta --> S4_Trigger : consulta == 1

    S4_Trigger --> S5_Echo
    S5_Echo --> S6_EvaluarNivel

    S6_EvaluarNivel --> S8_Flujo0 : con_in < umbral
    S6_EvaluarNivel --> S7_EvaluarCalidad : con_in >= umbral

    S7_EvaluarCalidad --> S8_Flujo1 : calidad == 1
    S7_EvaluarCalidad --> S8_Flujo0 : calidad != 1

    S8_Flujo0 --> S9_SalidaFlujo
    S8_Flujo1 --> S9_SalidaFlujo

    S9_SalidaFlujo --> S2_LeerEntradas : ciclo continuo
```

---

### Estados:

- **S0_Inicio**: Estado inicial del sistema.
- **S1_ConfigurarHardware**: Configuración del hardware.
- **S2_LeerEntradas**: Recolección de datos de entrada.
- **S3_EsperarConsulta**: Espera de señal desde el sistema central.
- **S4_Trigger**: Enviar pulso de disparo al sensor HC-SR04.
- **S5_Echo**: Lectura de tiempo de respuesta del sensor.
- **S6_EvaluarNivel**: Comparación entre "con_in" y "umbral".
- **S7_EvaluarCalidad**: Verifica si la calidad del agua es aceptable.
- **S8_Flujo0**: Desactivar bomba (flujo = 0).
- **S8_Flujo1**: Activar bomba (flujo = 1).
- **S9_SalidaFlujo**: Salida de control de flujo.

---

Este FSM se implementa en el  microcontrolador  ESP32 y forma parte de sistemas automatizados de control de nivel de agua.

# Diagrama Funcional del Sistema de Control de Nivel de Agua

<img width="1925" height="3845" alt="Untitled diagram _ Mermaid Chart-2025-07-21-212210" src="https://github.com/user-attachments/assets/eac56dbb-dde4-41ea-9434-e9e6b5cd452e" />


---
##
### Estados:


- **GenerarTrigger**: Envía el pulso ultrasónico desde el sensor HC-SR04.
- **EsperaEcho**: Monitorea la señal de Echo del HC-SR04.
- **CapturaEcho**: Mide el tiempo del pulso de retorno.
- **CalcularDistancia**: Calcula la distancia usando el tiempo medido.
- **Resultado**: Guarda o muestra la distancia medida.
- **Error**: Indica una falla por falta de respuesta (Timeout).
- **TriggerON**: Pone Trigger = 1, activando el pulso ultrasónico.
- **ConteoON**: Cuenta ciclos para controlar la duración del Trigger.
- **TriggerOFF**: Pone Trigger = 0, finalizando el pulso.
- **Escucha**: Espera un flanco de subida en Echo para comenzar la medición.
- **Timeout**: Indica que no se recibió Echo a tiempo; salta a error.
- **MedirTiempo**: Mide cuánto tiempo permanece Echo en alto.
- **FinCaptura**: Detecta flanco de bajada en Echo; finaliza el conteo.
- **CalcularDistancia**: Aplica la fórmula de distancia:
     distancia = (con_in × 34300) / (2 × 25e6)




##
Simulación con testbenches
Simulación en verilog-gtkwave (make sim),  o vídeo de simulación en digital (únicamente youtube, no se acepta drive o similares)
##

Diagrama RTL del SoC y su mòdulo:
Diagrama RTL del SoC y de su módulo (make rtl, make rtl top=modulo_especifico)

**RTL DEL SOC**

Este muestra toda la estructura del sistema embebido (SoC): incluye el procesador, los periféricos, los buses de comunicación, la memoria y los módulos personalizados como el del sensor HC-SR04

![Imagen de WhatsApp 2025-07-21 a las 04 35 25_7949625c](https://github.com/user-attachments/assets/8e651e66-d044-4c03-961e-797872daa5ed)

**RLT DEL SENSOR**

Este muestra sólo la lógica del módulo HC-SR04: cómo se manejan las señales echo, trigger, distance, los contadores, registros y comparadores internos.

![Imagen de WhatsApp 2025-07-21 a las 04 36 46_4793faf7](https://github.com/user-attachments/assets/932c94a1-f8c6-473e-a273-fa52fa5789c5)

**RLT PERIP**

Muestra la lógica de integración del módulo sensor como un periférico mapeado a memoria, con señales de dirección, lectura/escritura, selección de chip, etc.

![Imagen de WhatsApp 2025-07-21 a las 04 37 38_97ecb2bf](https://github.com/user-attachments/assets/dde206fa-ba98-4c15-a02d-e81f86635031)


##
Simulaciones:
Archivos fuentes del SoC, simulaciones, etc.


Medir la distancia sensor 

![Imagen de WhatsApp 2025-07-21 a las 18 38 21_c0e87ff0](https://github.com/user-attachments/assets/9fc2444b-c08e-4e10-88e3-dfffe0a1d6a5)



##
Video simulacion:
##
Logs de make log-prn, make log-syn diagramas de flujo 
Logs de make log-pnr, make log-syn, donde se identifique los warnings y los recursos usados en el flujo de síntesis.

##
¿Còmo interactùa con entornos externos?
Explicación sobre el cómo interactúa con aplicaciones externas (mqtt, chuck) etc.
##
Video del proyecto
Vídeo de youtube explicando el flujo de diseño (lo que se ve en el README.md) e implementación, agregar el enlace del vídeo del  README.md del módulo.
##
. En el README.md del proyecto explicar el contexto y la arquitectura.
##

























