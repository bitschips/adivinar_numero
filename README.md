## Diagrama de clases


## Diagrama de secuencia
´´´mermaid
sequenceDiagram
    actor Usuario
    participant UI as InterfazUsuario
    participant Juego as JuegoAdivinanza
    participant Estado as EstadoJuego
    participant Val as Validador
    participant Hist as Historial

    Note over Usuario,Hist: Inicialización del Juego
    
    Usuario->>Juego: Cargar página
    activate Juego
    Juego->>Estado: new EstadoJuego()
    activate Estado
    Estado->>Estado: generarNumeroAleatorio(1, 100)
    Estado->>Estado: intentosRestantes = 3
    Estado-->>Juego: estado inicial
    deactivate Estado
    
    Juego->>Hist: new Historial()
    activate Hist
    Hist-->>Juego: historial vacío
    deactivate Hist
    
    Juego->>UI: actualizarIntentos(3)
    activate UI
    UI->>Usuario: Mostrar intentos: 3
    deactivate UI
    
    Juego->>UI: configurarEventos()
    activate UI
    UI-->>Juego: eventos configurados
    deactivate UI
    deactivate Juego

    Note over Usuario,Hist: Ciclo de Adivinanza

    loop Mientras intentosRestantes > 0 AND no ganado
        Usuario->>UI: Escribir número y presionar Enter/Botón
        activate UI
        UI->>UI: obtenerIntento()
        UI->>Juego: makeGuess(numero)
        deactivate UI
        
        activate Juego
        Juego->>Val: esNumeroValido(numero)
        activate Val
        Val->>Val: estaEnRango(numero, 1, 100)
        
        alt Número inválido
            Val-->>Juego: false
            deactivate Val
            Juego->>UI: mostrarMensaje("⚠️ Número entre 1 y 100", "hint")
            activate UI
            UI->>Usuario: Mostrar advertencia
            deactivate UI
        else Número válido
            Val-->>Juego: true
            deactivate Val
            
            Juego->>Estado: decrementarIntentos()
            activate Estado
            Estado->>Estado: intentosRestantes--
            Estado-->>Juego: intentos actualizados
            deactivate Estado
            
            Juego->>Hist: agregarIntento(numero)
            activate Hist
            Hist->>Hist: intentos.push(numero)
            Hist->>UI: renderizar()
            activate UI
            UI->>Usuario: Actualizar historial
            deactivate UI
            deactivate Hist
            
            Juego->>Estado: getNumeroSecreto()
            activate Estado
            Estado-->>Juego: numeroSecreto
            deactivate Estado
            
            alt Adivinó correctamente
                Juego->>Estado: ganado = true
                activate Estado
                deactivate Estado
                Juego->>UI: mostrarMensaje("🎉 ¡Adivinaste!", "success")
                activate UI
                UI->>Usuario: Mostrar victoria
                deactivate UI
                Juego->>UI: deshabilitarJuego()
                activate UI
                UI->>UI: btnAdivinar.disabled = true
                UI->>UI: inputAdivinanza.disabled = true
                deactivate UI
                
            else Número incorrecto
                Juego->>Val: obtenerPista(numero, numeroSecreto)
                activate Val
                
                alt numero < numeroSecreto
                    Val-->>Juego: "más alto"
                else numero > numeroSecreto
                    Val-->>Juego: "más bajo"
                end
                deactivate Val
                
                Juego->>Estado: getIntentosRestantes()
                activate Estado
                Estado-->>Juego: intentosRestantes
                deactivate Estado
                
                alt intentosRestantes == 0
                    Juego->>Estado: perdido = true
                    activate Estado
                    deactivate Estado
                    Juego->>UI: mostrarMensaje("😔 Perdiste! Era " + numeroSecreto, "error")
                    activat
    InterfazUsuario ..> MensajeJuego : muestra

```
    Historial --> InterfazUsuario : actualiza
```
