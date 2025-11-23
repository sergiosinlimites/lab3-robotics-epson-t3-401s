# Laboratorio No. 2 - Robótica Industrial - Análisis y Operación del Manipulador Motoman MH6

## Integrantes

* Sergio Andrés Bolaños Penagos
* Sergio Felipe Rodriguez Mayorga

## Introducción

## Objetivos

## Movimiento EPSON T3-401S
* Descripci´on de las configuraciones home del EPSON T3-401S, indicando la posici´on de cada articulaci´on.
* Describir el procedimiento y cu´ales teclas se usan para realizar el movimiento manual del manipulador EPSON T3-401S por articulaciones, cambiar a movimientos cartesianos y realizar movimientos de traslaci´on y rotación en los ejes X, Y, Z.
* Detallar los niveles de velocidad del EPSON T3-401S para movimientos manuales y su configuraci´on, ¿C´omo se hace el cambio entre niveles de velocidad?, ¿C´omo se identifica en la pantalla el nivel de velocidad establecido?

## Comparación especificaciones técnicas Motoman MH6, ABB IRB140 y EPSON T3-401S
* Comparar las especificaciones t´ecnicas del EPSON T3-401S, Motoman MH6 y el ABB IRB140 en un cuadro comparativo. incluyendo carga m´axima, alcance, n´umero de grados de libertad, velocidad, aplicaciones t´ıpicas, etc.

## Características EPSON RC+ 7.0
* Describir las diferencias entre los diferentes tipos de trayectorias disponibles en el sofware EPSON RC+ 7.0.
* Explicar las aplicaciones principales de EPSON RC+ 7.0 y c´omo se comunica con el manipulador, ¿Qu´e hace EPSON RC+ 7.0 para mover el manipulador?

## Comparación RC+ 7.0, RobotStudio y RoboDK


| **Criterio** | **RoboDK** | **RobotStudio** | **EPSON RC+ 7.0** |
|-------------|------------|-----------------|--------------------|
| **Fabricante / desarrollador** | RoboDK Inc. (Canadá) | ABB Robotics | Epson Robots (Seiko Epson Corporation) |
| **Compatibilidad** | Multimarca (Yaskawa, ABB, KUKA, Fanuc, UR, etc.) | Exclusivo para robots ABB (aunque con más trabajo se pueden incluir otros) | Exclusivo para robots EPSON |
| **Lenguaje de programación** | Python o bloques; genera código para INFORM, RAPID, KRL, etc. | RAPID | SPEL+ |
| **Simulación** | 3D completa, cinemática, colisiones, tiempos de ciclo | 3D con Virtual Controller | Simulación 2D/3D EPSON |
| **Comunicación con robot real** | Ethernet/IP, archivos .JBI/.mod | Conexión con IRC5/OmniCore | Conexión directa por Ethernet/USB |
| **Precisión** | Alta (depende de postprocesador) | Muy alta | Alta para robots EPSON |
| **Programación offline** | Sí | Sí | Sí (SPEL+) |
| **Automatización** | Integración con visión y sensores | Librerías industriales | Visión EPSON, E/S digitales |
| **Gemelo digital** | Sí | Requiere módulos extra | Simulación cercana al robot EPSON |
| **Ventajas** | Multimarca, Python, fácil de usar | Integración total ABB | Integración nativa EPSON |
| **Limitaciones** | Menor fidelidad si no está bien programado | Solo ABB | Solo EPSON |
| **Aplicaciones típicas** | Docencia, investigación | Industria ABB | Industria EPSON |
| **Licencia** | Gratuita + comercial | Comercial | Incluido con robots EPSON |

RoboDK es flexible y multimarca, ideal para educación o uso en robots no tan comerciales. RobotStudio es el entorno oficial para ABB, con precisión industrial. EPSON RC+ 7.0 es el entorno nativo para robots EPSON, facilitando programación en SPEL+, configuración de E/S y simulación de trayectorias del T3‑401S.

- **EPSON RC+ 7.0:** Herramienta principal en el laboratorio para T3‑401S. Nos permite programar, simular y controlar el robot real.
- **RoboDK:** Software para experimentar con robots de múltiples marcas y robótica con Python.
- **RobotStudio:** Es el más completo y avanzado de todos pero con la limitante de que solo funciona para robots de la marca ABB.

## Diseño Gripper
* Dise˜nar un gripper neum´atico por vac´ıo utilizando las entradas y salidas digitales del robot EPSON T3-401S, que tenga la capacidad de levantar un huevo de manera segura y estable.
* Incluyendo diagrama esquem´atico, componentes utilizados y configuraci´on de las E/S digitales del robot.

Se diseñó el siguiente soporte para un gripper neumático, como se puede ver, cuenta con una base la cual se engancha alrededor del eje del robot y se ajusta con un tornillo y tuerca con el fin de sujetarlo externamente y evitar complicaciones en su montaje.

![Gripper Neumático Diseñado](img/AdaptadorGripper.png)\
Adaptador Gripper Neumático Diseñado

A continuación se muestran algunos de los planos más importantes, sin embargo los planos completos se encunetran en el siguiente [PDF](./Planos.pdf)
 
![Gripper Neumático 3D](img/Gripper3D.png)\
Plano de vista isométrica con partes del gripper.

![Gripper Base](img/GripperBase.png)\
Plano de la base del soporte.

![Gripper Base](img/GripperVentosa.png)\
Plano de la ventosa utilizada.

## Diseño y programación de trayectoria


```text
' ==============================================
'  Dos huevos en patrón de CABALLO (matriz 6x5)
'  EPSON RC+ 7.0 / T3-401S
'  - Jump hace bajar/subir.
'  - Out_9 controla la ventosa.
'  - Imprime "regresa a" y "va a", y el total visitado.
' ==============================================

Global Integer i
Global Integer k
Global Integer curA
Global Integer curB
Global Integer pathA(15)
Global Integer pathB(15)

' --- Para impresión y conteo ---
Global Integer visited(30)
Global Integer visitedCount
Global Integer r, c   ' temporales para fila/col de impresión

Function CargarRutas
    ' ----------------- Huevo 1 (empieza en 1) -----------------
    pathA(1) = 1
    pathA(2) = 9
    pathA(3) = 5
    pathA(4) = 18
    pathA(5) = 29
    pathA(6) = 21
    pathA(7) = 25
    pathA(8) = 14
    pathA(9) = 3
    pathA(10) = 7
    pathA(11) = 20
    pathA(12) = 28
    pathA(13) = 24
    pathA(14) = 11
    pathA(15) = 22

    ' ----------------- Huevo 2 (empieza en 30) ----------------
    pathB(1) = 30
    pathB(2) = 17
    pathB(3) = 6
    pathB(4) = 10
    pathB(5) = 2
    pathB(6) = 13
    pathB(7) = 26
    pathB(8) = 15
    pathB(9) = 19
    pathB(10) = 8
    pathB(11) = 4
    pathB(12) = 12
    pathB(13) = 16
    pathB(14) = 27
    pathB(15) = 23
Fend

Function MarcarVisitado(idx As Integer)
    If visited(idx) = 0 Then
        visited(idx) = 1
        visitedCount = visitedCount + 1
    EndIf
Fend

Function ImprimeIdx(prefijo$ As String, idx As Integer)
    ' Convierte idx (1..30) a (col, fila) en matriz 6x5
    r = (idx - 1) / 6 + 1               ' fila 1..5
    c = idx - (r - 1) * 6               ' col  1..6
    Print prefijo$, " idx=", idx, " -> (col=", c, ", fila=", r, ")"
Fend

Function Paletizado_01
    ' ---- Pallet 6x5 (COLS=6, ROWS=5) ----
    Pallet 1, Origen, PuntoX, PuntoY, 6, 5

    ' ---- Cargar rutas de caballo ----
    Call CargarRutas

    ' ---- Inicializar visitados ----
    For i = 1 To 30
        visited(i) = 0
    Next
    visitedCount = 0

    ' Posiciones guardadas iniciales
    curA = pathA(1)    ' (1,1)
    curB = pathB(1)    ' (6,5)

    ' Marcar e imprimir estados iniciales
    Call MarcarVisitado(curA)
    Call MarcarVisitado(curB)
    Print "Inicio cabalgado 2-huevos: matriz 6x5"
    Call ImprimeIdx("H1 inicia en", curA)
    Call ImprimeIdx("H2 inicia en", curB)
    Print "Visitados únicos: ", visitedCount, "/30"

    ' ---- Alternar movimientos ----
    For k = 2 To 15

        ' -------- Huevo 1 ----------
        Call ImprimeIdx("H1: regresa a (PICK)", curA)
        Call MarcarVisitado(curA)
        
        'Off Out_9
        Jump Pallet(1, curA)              ' PICK
        Off Out_9
        Call ImprimeIdx("H1: va a (PLACE)", pathA(k))
        Call MarcarVisitado(pathA(k))

        Jump Pallet(1, pathA(k))          ' PLACE
        On Out_9
       ' Wait 1

        curA = pathA(k)
        Print "Visitados únicos: ", visitedCount, "/30"

        ' -------- Huevo 2 ----------
        Call ImprimeIdx("H2: regresa a (PICK)", curB)
        Call MarcarVisitado(curB)

       ' Off Out_9
        Jump Pallet(1, curB)              ' PICK
         Off Out_9
        Call ImprimeIdx("H2: va a (PLACE)", pathB(k))
        Call MarcarVisitado(pathB(k))

        Jump Pallet(1, pathB(k))          ' PLACE
        On Out_9
        'Wait 1

        curB = pathB(k)
        Print "Visitados únicos: ", visitedCount, "/30"

    Next

    ' --- Fin del paletizado: asegurar ventosa y volver a Home ---
    On Out_9
    Print "Fin: visitados únicos totales = ", visitedCount, "/30"
    Print "Regresando a Home..."
    Jump Origen
    Home
Fend

Function main
    Motor On
    Power High
    Accel 100, 100
    Speed 100
    Home
    On Out_9

    Call Paletizado_01

    ' Si prefieres con botón:
    'Do
    '    If In_9 = On Then
    '        Call Paletizado_01
    '    EndIf
    'Loop
Fend
```
flowchart TD
  %% ===== MAIN =====
  A[main start] --> B[Init: Motor On; Power High; Accel 100,100; Speed 100; Home]
  B --> C[OUT9_ON  (liberar)]
  C --> D[Call Paletizado_01]
  D --> E[main end]

  %% ===== PALETIZADO_01 (setup) =====
  subgraph S[Paletizado_01]
    S1[Definir Pallet 6x5]
    S2[CargarRutas]
    S3[visited[]=0; visitedCount=0]
    S4[curA=pathA(1); curB=pathB(1)]
    S5[Marcar curA y curB; Print inicio; Print visitados]
    S1 --> S2
    S2 --> S3
    S3 --> S4
    S4 --> S5
  end

  D --> S
  S --> Q

  %% ===== LOOP k = 2..15 =====
  Q{ k de 2 a 15 } --> |siguiente k| H1A[H1: print regresa a curA]
  H1A --> H1B[H1: marcar curA]
  H1B --> H1C[H1: Jump curA (pick)]
  H1C --> H1D[H1: OUT9_OFF  (agarrar)]
  H1D --> H1E[H1: print va a pathA(k)]
  H1E --> H1F[H1: marcar pathA(k)]
  H1F --> H1G[H1: Jump pathA(k) (place)]
  H1G --> H1H[H1: OUT9_ON  (liberar)]
  H1H --> H1I[H1: curA=pathA(k); print visitados]

  H1I --> H2A[H2: print regresa a curB]
  H2A --> H2B[H2: marcar curB]
  H2B --> H2C[H2: Jump curB (pick)]
  H2C --> H2D[H2: OUT9_OFF  (agarrar)]
  H2D --> H2E[H2: print va a pathB(k)]
  H2E --> H2F[H2: marcar pathB(k)]
  H2F --> H2G[H2: Jump pathB(k) (place)]
  H2G --> H2H[H2: OUT9_ON  (liberar)]
  H2H --> H2I[H2: curB=pathB(k); print visitados]
  H2I --> Q

  %% ===== FIN =====
  Q --> |fin| F1[OUT9_ON  (liberar)]
  F1 --> F2[Print totales visitados]
  F2 --> F3[Jump Origen]
  F3 --> F4[Home]

* Diagrama de flujo de la rutina de movimiento de huevos con patr´on de caballo de ajedrez.
* Dise˜nar y programar una trayectoria en EPSON RC+ 7.0 que permita manipular dos huevos ubicados inicialmente en los extremos de una cubeta de 30 huevos (6x5). La rutina debe posicionar los huevos en todas las posiciones de la cubeta, moviendo un huevo y luego el otro alternadamente, con la restricci´on de que los huevos solo pueden moverse siguiendo el patr´on de movimiento del caballo en el ajedrez.
* C´odigo desarrollado en EPSON RC+ 7.0 para ejecutar la trayectoria con patr´on de caballo, adjuntado como
 anexo dentro del repositorio.

## Resultados

* Video de simulaci´on en EPSON RC+ 7.0 mostrando la trayectoria completa y evidencia de su implementaci´on física en el manipulador EPSON T3-401S.

* Video demostrativo del gripper neum´atico levantando un huevo de manera segura.