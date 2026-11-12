# Jarvis — bienvenida con dos aplausos para macOS

Un script de Python que escucha el micrófono y, al detectar dos picos de sonido dentro de una ventana de dos segundos, ejecuta una bienvenida: dice una frase, abre YouTube y coloca Claude Desktop y Cursor lado a lado.

Este repositorio es un **fork de [RafaTatay/jarvis](https://github.com/RafaTatay/jarvis)**. Conserva el script original y documenta cómo configurarlo.

## Qué hace

1. Escucha audio mono a 44,100 Hz y calcula su nivel RMS cada 50 ms.
2. Cuenta los picos que superan el umbral configurado.
3. Dice «Bienvenido a casa, señor Tatay.» con la voz del sistema.
4. Abre la URL de YouTube configurada en el navegador predeterminado.
5. Crea `~/Desktop/nuevo_proyecto` si no existe y abre esa carpeta en Cursor.
6. Abre Claude Desktop e intenta acomodar ambas ventanas mediante AppleScript.

La detección usa volumen; otros sonidos fuertes pueden activar la secuencia. Abrir YouTube no garantiza la reproducción automática: depende del navegador.

## Requisitos

- macOS con sesión gráfica.
- Python 3.9 o superior.
- Micrófono disponible.
- Claude Desktop y Cursor instalados.
- Conexión a internet para YouTube.
- Permiso de micrófono para la aplicación desde la que ejecutas Python.
- Permisos de Accesibilidad y, si macOS los solicita, Automatización, para controlar las ventanas mediante System Events.

La bienvenida usa síntesis de voz local; el script no necesita una clave de API ni envía instrucciones a Claude o Cursor.

## Instalación y ejecución

Desde Terminal:

```bash
git clone https://github.com/jorgefsb/jarvis.git
cd jarvis
python3 -m venv .venv
source .venv/bin/activate
python -m pip install sounddevice numpy pyttsx3
python bienvenido_jarvis.py
```

Concede los permisos que solicite macOS y aplaude dos veces. Detén el programa con **Ctrl+C**. Para volver a ejecutarlo:

```bash
cd jarvis
source .venv/bin/activate
python bienvenido_jarvis.py
```

## Configuración

Edita las constantes al inicio de [bienvenido_jarvis.py](bienvenido_jarvis.py):

| Constante | Valor actual | Para qué sirve |
| --- | --- | --- |
| `THRESHOLD` | `0.20` | Umbral RMS: súbelo si se activa con ruido; bájalo si no detecta los aplausos. |
| `COOLDOWN` | `0.1` segundos | Separación mínima entre picos contados. |
| `DOUBLE_WINDOW` | `2.0` segundos | Tiempo máximo entre los dos picos. |
| `YOUTUBE_URL` | URL incluida en el script | Video que se abre en el navegador. |
| `MENSAJE` | `Bienvenido a casa, señor Tatay.` | Frase de bienvenida; puedes personalizar el nombre. |
| `NEW_PROJECT` | `~/Desktop/nuevo_proyecto` | Carpeta que se crea y se abre en Cursor. |

La voz preferida es `Monica`, mediante el comando `say` de macOS. Si ese comando devuelve un error, se intenta usar `pyttsx3`.

## Solución de problemas

| Síntoma | Qué revisar |
| --- | --- |
| No detecta aplausos | Permiso de micrófono, dispositivo de entrada predeterminado y valor de `THRESHOLD`. |
| Se activa con música o ruido | Sube `THRESHOLD` y prueba en un ambiente más silencioso. |
| No abre Claude o Cursor | Comprueba que las aplicaciones estén instaladas con esos nombres. El script busca el CLI de Cursor y, si no lo encuentra, usa `open -a Cursor`. |
| No acomoda las ventanas | Revisa Accesibilidad y Automatización para Terminal o el ejecutor de Python. Los errores de AppleScript pueden quedar silenciados. |
| YouTube abre pero no reproduce | Revisa las reglas de reproducción automática del navegador. |
| Falla al abrir el dispositivo de audio | Comprueba que exista una entrada de audio disponible y que admita 44,100 Hz. |
| No se escucha la bienvenida | Revisa el volumen y la disponibilidad de la voz Monica; puedes probar `say -v Monica "Hola"`. |

## Alcance y estado

Prototipo de automatización de escritorio para macOS. El repositorio contiene el script y esta documentación; no incluye instalador, servicio de inicio automático, pruebas automatizadas ni integración con dispositivos del hogar.

El programa mantiene la escucha mientras está abierto. Tras detectar dos picos, pausa nuevas detecciones durante aproximadamente ocho segundos; esa espera no comprueba que todas las acciones hayan terminado. No configura pantalla completa ni Split View.

Verificación documental: código y metadatos revisados el **12 de noviembre de 2026**. La ejecución con micrófono, voz y ventanas en una Mac no se probó en esta revisión.

## Estructura

```text
.
├── README.md
└── bienvenido_jarvis.py
```

## Autoría y licencia

Autor original: **[RafaTatay](https://github.com/RafaTatay)**. Repositorio de origen: [RafaTatay/jarvis](https://github.com/RafaTatay/jarvis).

Este fork pertenece a [jorgefsb](https://github.com/jorgefsb). No hay un archivo `LICENSE` en este repositorio; no se declara una licencia nueva sobre el código de terceros.

<!-- HERDR-CONTEXT:START -->
## Contexto del proyecto y clasificación en Herdr

Automatización doméstica que detecta dos aplausos y ejecuta una bienvenida, música y apertura de aplicaciones.

**Espacio:** `02 LAB | REV | Jarvis`. **Unidad:** Unidad pendiente de confirmar.

### Clasificación

La etiqueta LAB identifica una clasificación de catálogo pendiente de revalidación; no implica inactividad. REV significa unidad propietaria pendiente de confirmar. Los números ordenan navegación; no son un ranking de ingresos.

Se conserva la documentación original y el crédito a sus autores. Esta sección no certifica disponibilidad actual ni modifica el runtime. Actualizada el 28-oct-2026.

<!-- HERDR-CONTEXT:END -->
