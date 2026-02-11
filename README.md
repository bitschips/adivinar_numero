# Tipos de diagramas:
1. Diagrama de Clases (UML de Estructura)
Este diagrama define la arquitectura de tu código siguiendo el modelo de Orientación a Objetos (como en Java).
Clase JuegoAdivinanza: Contiene los atributos internos (numeroSecreto, intentos) y la lógica pura (comprobarIntento).
Clase Interfaz/DOM: Maneja lo que el usuario ve (el input, el botón y los mensajes en pantalla).
Objetivo: Separar la "inteligencia" del juego de los "botones" de la web.

2. Diagrama de Secuencia (UML de Comportamiento)
Representa el paso a paso cronológico desde que ocurre una acción hasta que se recibe una respuesta.
Flujo: Usuario → Clic en Botón → Validación en JS → Consulta a la Clase Juego → Respuesta al HTML → Actualización del <title> de la página.
Objetivo: Visualizar cómo "viaja" la información entre los distintos archivos y funciones.

3. Diagrama de Flujo / Bloques (Lógica con Validaciones)
Es el que has incluido en tu README.md usando Mermaid.js.
Nodos de Decisión: Incluye los rombos de control (¿Es un número válido?, ¿Está en el rango 1-100?, ¿Es el número correcto?).
Caminos de Error: Define qué mensajes mostrar si el usuario se equivoca al escribir (texto en lugar de números).
Objetivo: Servir como mapa lógico para programar todas las posibilidades (casos de éxito y casos de error).

# Arquitectura del Juego

```mermaid
graph TD
    A[Usuario] -->|Escribe número| B(Interfaz HTML)
    B -->|Llama función| C{Clase Juego}
    C -->|Verifica| D[Resultado]
    D -->|Actualiza| B
```

# Diagrama de secuencia
```mermaid
sequenceDiagram
    participant U as Usuario
    participant H as HTML (Interfaz)
    participant J as JS (Clase Juego)

    U->>H: Introduce número y clica botón
    H->>J: juego.comprobarIntento(valor)
    Note over J: Incrementa intentos
    J-->>H: Devuelve "Más alto" o "Ganaste"
    H->>U: Actualiza texto en pantalla
```
# Validación de errores
```mermaid
graph TD
    A[Inicio: Usuario pulsa Botón] --> B{¿Input es un número?}
    B -- No (Texto o Vacío) --> C[Mostrar Error: 'Introduce un número válido']
    B -- Sí --> D{¿Está entre 1 y 100?}
    D -- No --> E[Mostrar Aviso: 'Fuera de rango']
    D -- Sí --> F[Clase Juego: comprobarIntento]
    F --> G{¿Es el correcto?}
    G -- No --> H[Actualizar Intentos y dar Pista]
    G -- Sí --> I[¡Victoria! Cambiar Color y Título]
    H --> J[Esperar nueva entrada]
    C --> J
    E --> J
```

# Subgrafos
```mermaid
graph TB
    subgraph Navegador_Usuario
        A[Input Numero] --> B[Validación Formato]
    end
    
    subgraph Logica_Interna
        B -- "Si es válido" --> C{Comparar Secreto}
        C -- "Menor" --> D[Pista: Más alto]
        C -- "Mayor" --> E[Pista: Más bajo]
        C -- "Igual" --> F[¡Victoria!]
    end

    D --> G[Actualizar HTML]
    E --> G
    F --> G
```
