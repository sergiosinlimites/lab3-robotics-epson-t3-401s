# Laboratorio No. 3 – Robótica Industrial – Análisis y Operación del Manipulador EPSON T3-401S

## Integrantes

* Sergio Andrés Bolaños Penagos
* Sergio Felipe Rodriguez Mayorga

## Introducción
En esta práctica se trabajó con el manipulador industrial **EPSON T3-401S**, lo que permitió ampliar la experiencia adquirida previamente en los laboratorios con robots ABB y Yaskawa, incorporando ahora un entorno de programación distinto mediante el software **EPSON RC+ 7.0**.

El objetivo principal del laboratorio consistió en implementar una rutina de **paletizado sobre una matriz 6×5** (una cubeta de huevos), utilizando tanto la definición geométrica del pallet como la generación de trayectorias personalizadas. Para este propósito se desarrolló un sistema capaz de recorrer las 30 posiciones de la matriz siguiendo un **patrón de movimiento tipo caballo de ajedrez**, optimizado para cubrir toda el área de trabajo con **dos objetos (huevos)** que partían desde esquinas opuestas.

Adicionalmente, se emplearon funciones personalizadas para la gestión de índices y el control del **gripper neumático** con el cual se manipulaban los huevos, lo que permitió comprender la estructura del lenguaje de programación del controlador EPSON y la interacción directa con el robot. Este laboratorio facilitó así la comparación entre distintos entornos industriales y reforzó competencias en **planificación de trayectorias, paletización y control de manipuladores**, además de fortalecer habilidades en **prototipado rápido y neumática**, dado que el soporte del gripper fue diseñado desde cero.
## Objetivos
 - Comprender las diferencias entre las características técnicas del manipulador **EPSON T3-401S**.
- Identificar y describir las configuraciones iniciales del manipulador **EPSON T3-401S**, incluyendo la definición de la posición de **Home**.
- Realizar movimientos manuales del manipulador **EPSON T3-401S** en distintos modos de operación (articulaciones, cartesianos, traslaciones y rotaciones).
- Cambiar y controlar los niveles de velocidad para el movimiento manual del manipulador **EPSON T3-401S**.
- Comprender las principales aplicaciones del software **EPSON RC+ 7.0** y su comunicación con el manipulador.
- Comparar y analizar las diferencias entre **RobotStudio**, **RoboDK** y **EPSON RC+ 7.0**.
- Diseñar un **gripper neumático** que permita la manipulación de objetos en el espacio de trabajo del robot **EPSON T3-401S**.
- Diseñar y ejecutar una trayectoria en el software **EPSON RC+ 7.0**, y realizar su implementación física en el manipulador **EPSON T3-401S**.

## Movimiento EPSON T3-401S
1. Descripción de las configuraciones **Home** del **EPSON T3-401S**, indicando la posición de cada articulación.

    La posicion Home por defecto del Robot se puede configurar al gusto, nosotros seleccionamos una posicion en donde la posicion de la articulacion 1 es justo la mitad, permitiendole asi moverse en ambas direcciones como se muestra en la siguiente imagen:![Descripción del Home](img/Home_description.PNG)
    Las respectivas posiciones articulares se muestran en el siguiente cuadro tomado del software :
    imagen:![Descripción de Articulaciones del Home](img/Home_joints_values.PNG)
    **Aca es importante destacar que los valores articulares se miden en pulsos de encoder**
   

2. Describir el procedimiento y las teclas utilizadas para realizar el movimiento manual del manipulador **EPSON T3-401S** por articulaciones, cambiar a movimientos cartesianos y ejecutar movimientos de traslación y rotación en los ejes X, Y y Z.
 
    **Para esto se oprime F6 lo cual abre el robot manager**, una vez ahi se va a la parte de Jog and teach y alli se puede mover el robot por cualquiera de los metodos descritos previamente como se aprecia a continuacion:
    - Movimiento por articulaciones:
    <p align="center">
    <img src="img/Joint_movement.PNG" width="600">
    </p>

    - Movimiento Cartesiano(espacio de la tarea):
    <p align="center">
    <img src="img/Cartesian_movement.PNG" width="800">
    </p>
    Como se aprecia en la imagen las opciones de tool, world,Local y ECP permiten realizar movimientos cartesianos, recordando que al ser un robot de 4 grados de libertad(RRRP), no puede realizar rotaciones en torno a los ejes coordenados , solo moverse en XYZ y rotar en torno a Z(respecto a los globales).

     

3 . Detallar los niveles de velocidad del **EPSON T3-401S** para movimientos manuales y su configuración.  Para detallar estos niveles de velocidad se tienen las siguientes configuraciones en el robot manager:
<p align="center">
    <img src="img/Power_levels.PNG" width="800">
    </p>

 <p align="center">
    <img src="img/Speed_levels.PNG" width="800">
    </p>
 


## Comparación especificaciones técnicas Motoman MH6, ABB IRB140 y EPSON T3-401S
 
# 📘 Comparación de Manipuladores Industriales  
## Motoman MH6 • ABB IRB 140 • EPSON T3-401S

Este documento presenta una comparación detallada entre los robots utilizados en los laboratorios de robótica industrial: **Motoman MH6**, **ABB IRB 140** y **EPSON T3-401S**, analizando sus principales características técnicas, aplicaciones y diferencias operativas.

---

## 📊 Tabla comparativa de especificaciones técnicas

| **Características** | **Motoman MH6** | **ABB IRB140** | **EPSON T3-401S** |
|---------------------|-----------------|----------------|-------------------|
| **Tipo de robot** | Articulado 6 DOF | Articulado 6 DOF | SCARA 4 DOF |
| **Carga máxima** | 6 kg | 6 kg | 3 kg |
| **Alcance horizontal** | 1422 mm | 700 mm | 400 mm |
| **Alcance vertical** | 2486 mm | 1050 mm | 200 mm |
| **Grados de libertad** | 6 | 6 | 4 (X, Y, Z, θ) |
| **Repetibilidad** | ±0.08 mm | ±0.05 mm | ±0.02 mm |
| **Velocidad máx. articulaciones** | S: 220°/s<br>L: 200°/s<br>U: 220°/s<br>R: 410°/s<br>B: 410°/s<br>T: 610°/s | S: 150°/s<br>L: 120°/s<br>U: 120°/s<br>R: 180°/s<br>B: 180°/s<br>T: 220°/s | X/Y: 2000 mm/s<br>Z: 700 mm/s<br>θ: 2000°/s |
| **Temperatura de operación** | 0°C a +45°C | 0°C a +45°C | 5°C a +40°C |
| **Peso** | 130 kg | 240 kg | 14 kg |
| **Tipo de montaje** | Piso, techo, pared | Piso, techo | Piso |
| **Aplicaciones típicas** | Manipulación, procesamiento | Ensamble, manipulación, soldadura | Paletizado ligero, pick-and-place, empaque |
## 🔗 Referencias

### **Motoman MH6**
- https://www.robots.com/industrial-robots/motoman-mh6  
- https://pdf.directindustry.com/pdf/yaskawa-europe-gmbh/mh6d-mh6f/14473-309337.html  

### **ABB IRB 140**
- https://library.e.abb.com/public/a7121292272d40a9992a50745fdaa3b2/3HAC041346%20PS%20IRB%20140-en.pdf  
- https://www.manuallib.com/download/pdf/2014/0624/abb-irb140-industrial-robot-datasheet.pdf  
- https://www.scribd.com/document/649705967/IRB-140-Type-C-Product-Manual-3HAC027400-001-RevC-En  

### **EPSON T3-401S**
- https://epson.com/robots/scara/t3  
- https://files.support.epson.com/docid/cpd5/cpd57658.pdf  
- https://files.support.epson.com/pdf/rbt_t3/rbt_t3_um.pdf  
- https://files.support.epson.com/docid/cpd5/cpd58541.pdf  















## Características EPSON RC+ 7.0
* Describir las diferencias entre los diferentes tipos de trayectorias disponibles en el sofware EPSON RC+ 7.0.
* Explicar las aplicaciones principales de EPSON RC+ 7.0 y c´omo se comunica con el manipulador, ¿Qu´e hace EPSON RC+ 7.0 para mover el manipulador?

## Comparación RC+ 7.0, RobotStudio y RoboDK
* Analizar las diferencias entre EPSON RC+ 7.0, RoboDK y RobotStudio y describir los usos espec´ıficos de cada herramienta, ¿Qu´e significa para usted cada una de esas herramientas?
* Destacando ventajas, limitaciones y aplicaciones de cada herramienta.

## Diseño Gripper
* Dise˜nar un gripper neum´atico por vac´ıo utilizando las entradas y salidas digitales del robot EPSON T3-401S, que tenga la capacidad de levantar un huevo de manera segura y estable.
* Incluyendo diagrama esquem´atico, componentes utilizados y configuraci´on de las E/S digitales del robot.

## Diseño y programación de trayectoria

### Diagrama de flujo de acciones del robot
```mermaid
  
flowchart TD

A[Inicio] --> A1[Encender motores]
A1 --> A2[Configurar velocidad]
A2 --> A3[Ir a Home]

A3 --> B[Marcar H1 y H2 como visitadas]
B --> C[contador = 2]

C --> D{contador ≤ 30?}

D -- No --> Z1[Ir a Home]
Z1 --> Z2[Apagar motores]
Z2 --> Z[Fin del proceso]

D -- Sí --> H1P[H1: PICK]
H1P --> H1EV0[EV = 0]
H1EV0 --> H1PRINT1[Imprimir pos. actual H1]
H1PRINT1 --> H1PL[H1: PLACE]
H1PL --> H1EV1[EV = 1]
H1EV1 --> H1PRINT2[Imprimir nueva pos. H1]
H1PRINT2 --> ADD1[contador = contador + 1]

ADD1 --> H2P[H2: PICK]
H2P --> H2EV0[EV = 0]
H2EV0 --> H2PRINT1[Imprimir pos. actual H2]
H2PRINT1 --> H2PL[H2: PLACE]
H2PL --> H2EV1[EV = 1]
H2EV1 --> H2PRINT2[Imprimir nueva pos. H2]
H2PRINT2 --> ADD2[contador = contador + 1]

ADD2 --> D
```

 ## Vista de planta
 <p align="center">
    <img src="img/Top_view.PNG" width="800">
    </p>


## Resultados

* Video de simulaci´on en EPSON RC+ 7.0 mostrando la trayectoria completa y evidencia de su implementaci´on física en el manipulador EPSON T3-401S.

* Video demostrativo del gripper neum´atico levantando un huevo de manera segura.