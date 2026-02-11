
# Uno
## Dos
```mermaid
class JuegoAdivinanza {
    constructor(maxNumero = 100) {
        this.maxNumero = maxNumero;
        this.reiniciar();
    }

    // Lógica pura (Model)
    reiniciar() {
        this.numeroSecreto = Math.floor(Math.random() * this.maxNumero) + 1;
        this.intentos = 0;
        this.terminado = false;
    }

    comprobarIntento(numeroUsuario) {
        this.intentos++;
        if (numeroUsuario === this.numeroSecreto) {
            this.terminado = true;
            return "¡Correcto!";
        }
        return numeroUsuario < this.numeroSecreto ? "Más alto ↑" : "Más bajo ↓";
    }
}

// Controlador de la Interfaz (View/Controller)
const juego = new JuegoAdivinanza(100);

function manejarClick() {
    const input = document.getElementById("numeroInput");
    const mensaje = document.getElementById("resultadoMensaje");
    
    const valor = parseInt(input.value);
    const resultado = juego.comprobarIntento(valor);
    
    mensaje.innerText = `${resultado} (Intentos: ${juego.intentos})`;
}
```
