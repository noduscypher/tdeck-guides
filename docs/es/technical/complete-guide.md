# Guía Completa T-Deck Plus Meshtastic

**Versión 1.0 | Abril 2026**

> Léelo en: [PT](../../pt/technical/complete-guide.md) · [EN](../../en/technical/complete-guide.md) · [JA](../../ja/technical/complete-guide.md)

> 📝 **Nota sobre esta versión:** el archivo original perdió formato significativo (varias tablas, bloques de código y diagramas ASCII se perdieron). Esta versión reconstruye la estructura en Markdown limpio. Donde hubo pérdida total de contenido, se marca con ⚠️ para revisar antes de publicar. Marcadores ✓ indican reconstrucciones de alta confianza.

---

## Índice

1. [Introducción](#introducción)
2. [Requisitos](#requisitos)
3. [Modo Programming (DFU/Flash)](#modo-programming-dfuflash)
4. [Instalación de Firmware — Erase + Flash](#instalación-de-firmware--erase--flash)
5. [Actualización de Firmware (Normal)](#actualización-de-firmware-normal)
6. [Configuración Inicial](#configuración-inicial)
7. [Bluetooth — Modo Programming](#bluetooth--modo-programming)
8. [Bluetooth — Desactivar para Uso Standalone](#bluetooth--desactivar-para-uso-standalone)
9. [Mapas Offline (Opcional)](#mapas-offline-opcional)
10. [Atajos de Teclado](#atajos-de-teclado)
11. [Troubleshooting](#troubleshooting)
12. [Recursos Adicionales](#recursos-adicionales)
13. [Checklist de Instalación Completa](#checklist-de-instalación-completa)

---

## Introducción

El LilyGO T-Deck Plus es un dispositivo standalone completo para Meshtastic con pantalla, teclado QWERTY, GPS integrado y LoRa. Esta guía cubre desde la instalación inicial hasta la configuración avanzada.

**Características principales:**

- ESP32-S3 + SX1262 LoRa
- GPS L76K integrado
- Pantalla TFT 2,8" táctil
- Teclado físico QWERTY
- Batería 18650 (no incluida)
- MicroSD para mapas offline

---

## Requisitos

### Hardware

- LilyGO T-Deck Plus
- Cable USB-C (datos, no solo carga)
- Batería 18650 (recomendado: 3000 mAh+)
- Antena LoRa 868 MHz (Europa) o 915 MHz (EEUU)
- MicroSD FAT32 ≤32 GB (opcional, para mapas)

### Software

- Navegador Chromium (Chrome, Edge, Brave)
- Conexión a internet activa
- App Meshtastic (Android/iOS) para pairing Bluetooth

### Avisos Importantes

> ⚠️ **Nunca enciendas el dispositivo sin antena conectada** — puedes dañar el módulo LoRa.
>
> ⚠️ Usa siempre cable USB-C con soporte de datos (no solo de carga).
>
> ⚠️ Define la región correcta (`EU_868` para Portugal) para cumplir con la regulación de ANACOM (Autoridade Nacional de Comunicações — autoridad portuguesa de telecomunicaciones).

---

## Modo Programming (DFU/Flash)

Para flashear firmware o hacer erase, el T-Deck Plus necesita entrar en modo DFU.

### Procedimiento

1. Apaga el T-Deck (switch lateral en OFF).
2. Pulsa y mantén el trackball (botón central).
3. Mientras mantienes el trackball, enciende el T-Deck (switch ON).
4. Espera 2–3 segundos manteniendo el trackball pulsado.
5. Suelta el trackball.
6. Conecta el cable USB-C al ordenador.

### Confirmación de Modo DFU

- La pantalla del T-Deck queda completamente negra (sin backlight).
- En el ordenador aparece un dispositivo: **"USB JTAG/serial debug unit"** o **"ESP32-S3"**.

> **Nota:** si no funciona a la primera, repite el proceso. Algunos cables USB-C a USB-C pueden dar problemas — prueba un cable USB-A a USB-C.

---

## Instalación de Firmware — Erase + Flash

**Cuándo usar:** primera instalación, dispositivo con problemas graves, cambio de tipo de firmware (p. ej., alpha → beta).

### Paso a Paso

1. **Entra en Modo Programming**
   Sigue el procedimiento de la sección anterior.

2. **Accede al Web Flasher**
   - Abre un navegador Chromium
   - Ve a: <https://flasher.meshtastic.org>

3. **Selecciona el Dispositivo**
   - Haz clic en "Select Target Device"
   - Elige: **"LILYGO T-Deck"** (funciona para T-Deck Plus)

4. **Elige Versión de Firmware**
   - **Recomendado:** selecciona la versión BETA más reciente (evita alpha en producción)
   - No uses versiones alpha para uso diario

5. **Configura Full Erase**
   - ✅ Activa la opción: **"Full Erase and Install"**
   - Esto borra toda la flash, incluidas configuraciones antiguas

6. **Conecta el Dispositivo**
   - Haz clic en "Connect"
   - Selecciona puerto: "USB JTAG/serial debug unit"
   - Haz clic en "Connect"

7. **Flash**
   - Haz clic en "Flash" y luego en "CONTINUE"
   - Espera a que termine (1–3 minutos)
   - **NO desconectes el USB durante el proceso**

8. **Finalización**
   - Cuando veas "Flash Complete", cierra el navegador
   - Apaga el T-Deck (switch OFF)
   - Vuelve a encenderlo (switch ON)
   - El dispositivo arranca con firmware fresco y MUI (Meshtastic UI)

---

## Actualización de Firmware (Normal)

**Cuándo usar:** actualizar a la versión más reciente sin perder configuraciones.

### Paso a Paso

1. **Entra en Modo Programming**
   Sigue el procedimiento: apaga → pulsa trackball → enciende → espera → suelta → conecta USB.

2. **Web Flasher**
   - Ve a: <https://flasher.meshtastic.org>
   - Selecciona: "LILYGO T-Deck"
   - Elige la versión de firmware deseada

3. **Update (Sin Erase)**
   - ❌ **Desactiva** la opción "Full Erase"
   - Haz clic en "Flash"

4. **Flash y Reboot**
   - Espera a que termine
   - Apaga y vuelve a encender el T-Deck
   - Las configuraciones se mantienen (región, nombre del nodo, canales, etc.)

> **Nota:** si tienes problemas tras la actualización, haz Full Erase + Flash.

---

## Configuración Inicial

Tras el flash inicial, configura los parámetros esenciales.

### Vía Meshtastic Web Client

1. **Conecta vía USB Serial**
   - Ve a: <https://client.meshtastic.org>
   - Haz clic en "Connect" → "USB Serial"
   - Selecciona el puerto del T-Deck

2. **Define la Región LoRa**
   - Menú lateral: **"LoRa"**
   - Region: selecciona **EU_868** (Portugal/Europa)
   - Haz clic en "Save"

   > ⚠️ **Obligatorio en Portugal:** EU_868 (868 MHz, 500 mW, 10% duty cycle), según ANACOM.

3. **Configura la Posición GPS**
   - Menú: **"Position"**
   - GPS Mode: "Enabled"
   - GPS Update Interval: 120 s (ajusta según necesidad)
   - Position Broadcast: actívalo si quieres compartir tu localización
   - Haz clic en "Save"

4. **Node Info (Opcional)**
   - Menú: **"Settings" → "Device"**
   - Node Name: define el nombre de tu nodo
   - Role: CLIENT (uso normal) o ROUTER (nodo fijo)

5. **Canales (Opcional)**
   - Menú: **"Channels"**
   - Configura canales adicionales o únete a una mesh existente
   - El Primary Channel viene preconfigurado

### Vía MUI en el T-Deck

Tras la configuración inicial vía web/app, puedes ajustar en el propio T-Deck:

- **Settings → LoRa** → verifica la región
- **Settings → Position** → estado del GPS
- **Map** → visualiza nodos y posición GPS

---

## Bluetooth — Modo Programming

**Cuándo usar:** pairing inicial con smartphone, configuración vía app, troubleshooting.

### Activar Bluetooth Programming Mode

#### Vía MUI (Firmware 2.6+)

1. En el T-Deck, ve a: **Settings** (icono de engranaje)
2. Selecciona: **"Reboot/Shutdown"**
3. Haz clic en el icono Bluetooth (centro de la pantalla)
4. La pantalla muestra: **">> Programming Mode <<"** con un passcode (p. ej., 123456)

#### Pairing con Smartphone

1. Abre la app Meshtastic (Android/iOS)
2. Ve a Bluetooth → Scan
3. Selecciona tu T-Deck (nombre del nodo)
4. Introduce el passcode mostrado en la pantalla del T-Deck
5. Conecta y configura vía la app

### Salir de Bluetooth Programming Mode

1. Pulsa y mantén el icono Bluetooth en la pantalla del T-Deck
2. El dispositivo reinicia y vuelve a MUI normal

> **Importante:**
> - Bluetooth Programming Mode es para pairing/configuración inicial.
> - Para uso diario con Bluetooth, cambia a BaseUI (próxima sección).
> - MUI no soporta Bluetooth permanentemente activo (usa la misma API).

---

## Bluetooth — Desactivar para Uso Standalone

**Cuándo usar:** operación standalone sin smartphone, ahorrar batería, usar MUI con mapas.

### Opción 1: Usar MUI (Modo Standalone Recomendado)

MUI (Meshtastic UI) es la interfaz visual completa, pero no soporta Bluetooth simultáneo.

- **Ventajas:** mapas, teclado, visualización completa de la mesh
- **Desventaja:** sin Bluetooth para la app

Para asegurarte de que el Bluetooth está apagado:

**Vía Meshtastic CLI:**

```bash
meshtastic --set bluetooth.enabled false
```

> ✓ Reconstruido: `meshtastic --set bluetooth.enabled false` (standard CLI syntax, confident)

**O vía Web Client:**

- Settings → Bluetooth → Enabled: OFF → Save

### Opción 2: BaseUI (Bluetooth Permanente)

Si necesitas Bluetooth activo para la app 24/7:

#### Cambiar a BaseUI

**Vía CLI:**

> ⚠️ **[VERIFICAR]** Comando CLI ausente en la fuente. Confirmar la sintaxis correcta contra la documentación actual de Meshtastic — la clave de configuración para cambiar a BaseUI puede variar entre versiones. Como alternativa, usa la ruta vía Web Client de abajo.

**O vía Web Client:**

- Settings → Device → Display Type: BaseUI
- Save y reboot

**Características de BaseUI:**

- Interfaz simple (sin mapas visuales)
- Bluetooth funciona permanentemente
- Menor consumo de recursos
- Ideal para control vía app

### Comparación Rápida

> ⚠️ Tabla reconstruida a partir del contexto descriptivo del propio documento. Verificar contra documentación oficial antes de publicar.

| Característica | MUI | BaseUI |
|---|---|---|
| Mapas visuales | ✅ | ❌ |
| Teclado y navegación completa | ✅ | Parcial |
| Bluetooth permanente para app | ❌ (solo Programming Mode) | ✅ |
| Consumo de recursos | Más alto | Más bajo |
| Caso de uso | Standalone con mapas | Control vía app |

> **Recomendación:** usa MUI para standalone con mapas. Cambia a BaseUI si necesitas que la app del smartphone esté siempre conectada.

---

## Mapas Offline (Opcional)

Añadir mapas offline al T-Deck Plus permite visualizar la mesh network sobre cartografía real.

### Requisitos

- MicroSD FAT32, ≤32 GB
- Pack de tiles de mapa (OpenStreetMap, Google Maps, Atlas)

### Estructura de Carpetas Correcta

La estructura de tiles sigue el formato standard XYZ de OpenStreetMap:

```
/maps/
└── <nombre-del-pack>/
    ├── 0/                  ← zoom level 0 (mundo entero)
    ├── 1/
    ├── 2/
    ├── ...
    ├── 8/                  ← zoom levels esenciales para Portugal
    ├── 9/
    ├── 10/
    ├── 11/
    ├── 12/                 ← zoom alto, muchos tiles
    ├── 13/
    ├── 14/
    └── 15/                 ← zoom máximo
```

**Importante:**

- Cada carpeta numerada (0–15) corresponde a un zoom level.
- Dentro de cada carpeta de zoom hay subcarpetas `X/` y archivos `Y.png` (formato XYZ).
- Los zoom levels bajos (0–5) cubren el mundo entero con pocos tiles.
- Los zoom levels altos (8–15) contienen muchos tiles y son esenciales para detalle local.
- Para Portugal, los zoom 8–14 son lo que realmente importa.
- Los zoom 1–5 pueden estar vacíos sin problema.

### Instalación Paso a Paso

#### Método 1: Pack Listo Atlas (Recomendado Global)

1. **Descarga el pack de mapas:**
   - Atlas (global, incluye Europa): <https://github.com/meshtastic/device-ui/tree/master/maps>
   - Archivo: `atlas.zip` ⚠️ *[nombre exacto del archivo a verificar en el repo]*

2. **Extrae el ZIP:**
   - Descomprime `atlas.zip`
   - Obtienes una carpeta `atlas/` con subcarpetas `0/` a `15/`

3. **Copia al SD:**
   - Crea la carpeta `/maps/` en la raíz del SD
   - Copia la carpeta `atlas/` completa a `/maps/atlas/`
   - Estructura final: `/maps/atlas/[0-15]/...`

4. **Verifica el contenido:**
   - Abre `/maps/atlas/12/` (u otro zoom alto)
   - Debe contener subcarpetas y archivos `.png`
   - Si está vacío, el pack está corrupto

#### Método 2: Pack Portugal (mapsportugal115OSM.zip)

Para usar el pack específico de Portugal:

1. **Extrae el archivo ZIP:**
   - Descomprime `mapsportugal115OSM.zip`
   - Obtienes una estructura de carpetas

2. **Navega hasta la carpeta `openstreetmap/`** dentro del ZIP extraído.

3. **Copia el contenido de la carpeta `openstreetmap/`:**
   - Entra en la carpeta `openstreetmap/` del ZIP extraído
   - Selecciona TODAS las carpetas numeradas (1, 2, 3... hasta 15)
   - Copia (Ctrl+C / Cmd+C)

4. **Pega en el SD dentro de la carpeta "Google Maps":**
   - Inserta el SD en el ordenador
   - Navega hasta: `/maps/Google Maps/` en el SD
   - Pega (Ctrl+V / Cmd+V) todo el contenido copiado

5. **Estructura final en el SD:**

   ```
   /maps/
   └── Google Maps/
       ├── 1/
       ├── 2/
       ├── ...
       └── 15/
   ```

6. **Verifica la instalación:**
   - Abre `/maps/Google Maps/12/` (u otro zoom alto) en el SD
   - Debe contener subcarpetas numeradas y archivos
   - Si está vacío, repite la extracción y copia

7. **Usar en el T-Deck:**
   - Inserta el SD en el T-Deck
   - Ve a Map
   - Long-press en el icono del mapa → selecciona "Google Maps"
   - Haz zoom en Portugal y espera el GPS fix

> **Nota:** el nombre de la carpeta "Google Maps" en el SD es solo una etiqueta — puedes renombrarla a "Portugal" u otro nombre si prefieres. Lo importante es la estructura interna (carpetas 1–15 con tiles).

#### Método 3: Generar Tiles Personalizados

**Script Python MapDL:**

```bash
# Descarga el script
wget https://gist.githubusercontent.com/droberin/b333a216d860361e329e74f59f4af4ba/raw/MapDL.py

# Edita coordenadas para Portugal
# lat: 37 a 42
# lon: -9.5 a -6
# zoom: 8-14

python3 MapDL.py

# Copia el output a /maps/portugal/
```

**MOBAC (Mobile Atlas Creator):**

- Descarga: <https://mobac.sourceforge.io>
- Selecciona región: Portugal
- Zoom levels: 8–14
- Output format: OSM Tile format
- Exporta y copia al SD

### Usar Mapas en el T-Deck

1. Inserta el SD en el T-Deck (slot lateral).
2. Enciende el dispositivo.
3. Ve a Map (icono del mapa).
4. Long-press en el icono del mapa (o Settings → Map).
5. Selecciona la fuente: `atlas`, `Google Maps`, `portugal`, o el nombre que hayas usado.
6. Haz zoom en Portugal (trackball + scroll).
7. GPS fix afuera — la posición aparece en el mapa.

### Troubleshooting de Mapas

**El mapa no aparece:**

- ¿SD formateado en FAT32? (no NTFS/exFAT)
- ¿Estructura `/maps/<nombre>/[0-15]/` correcta?
- ¿Las carpetas de zoom 8–14 contienen archivos `.png`?

**El mapa carga lento:**

- SD lenta (se recomienda class 10 o superior).
- Demasiados zoom levels (borra 1–7 si no los usas).

**Coordenadas incorrectas:**

- Espera el GPS fix (puede tardar 5–10 min en cold start).
- Sal al exterior con vista de cielo abierto.

---

## Atajos de Teclado

> ⚠️ **[CONTENIDO ORIGINAL PERDIDO — verificar antes de publicar]**
>
> La tabla completa de atajos de teclado del T-Deck Plus no pudo recuperarse de la fuente dañada. Sugerimos restituirla a partir de una de estas fuentes:
>
> - Repositorio oficial de device-ui: <https://github.com/meshtastic/device-ui>
> - Documentación oficial Meshtastic: <https://meshtastic.org/docs>
> - Discusiones de la comunidad Meshtastic Portugal: <https://meshtastic.pt>

### Notas Generales (recuperadas de la fuente)

- **ALT + tecla / SYM + tecla:** pulsa y suelta (no hace falta mantener pulsado).
- Los atajos `SYM + tecla` no están activos en los tabs SND y SET.
- La tecla **Fn** puede necesitar activación en firmware 2.7+.

---

## Troubleshooting

### El Dispositivo No Entra en Modo DFU

**Síntomas:** la pantalla se enciende normal, el ordenador no detecta dispositivo de flash.

**Soluciones:**

1. Repite la secuencia: apagar → trackball → encender → esperar 3 s → soltar.
2. Prueba el cable: usa USB-A a USB-C (en lugar de C-C).
3. Drivers: Windows puede necesitar drivers CH340/CP210x.
4. Inténtalo 3–5 veces: el timing es crítico.

### Bluetooth No Conecta

**Síntomas:** la app no ve el dispositivo, el pairing falla.

**Soluciones:**

**Si estás en MUI:**

- Bluetooth no funciona en MUI normal.
- Usa Bluetooth Programming Mode: Settings → Reboot → icono Bluetooth.
- O cambia a BaseUI permanentemente.

**Si Bluetooth está desactivado:**

```bash
meshtastic --set bluetooth.enabled true
```

> ✓ Reconstruido: `meshtastic --set bluetooth.enabled true` (standard CLI syntax, confident)

**Si tras una actualización de firmware:**

- Puede desactivarse solo.
- Reactívalo vía CLI o web client.
- Si persiste, haz rollback a un firmware anterior (2.5.18 conocido por funcionar).

**Problemas con iPhone:**

- Algunos firmwares (early 2.6.x) tienen bugs con iOS.
- Usa el web client por USB como alternativa.
- Actualiza al latest beta firmware.

### El GPS No Hace Fix

**Síntomas:** la posición no se actualiza, "No GPS fix" persiste.

**Soluciones:**

1. Sal al exterior: el GPS necesita vista de cielo abierto.
2. Espera 5–15 min: el first fix tarda (cold start).
3. Verifica la configuración:

   ```bash
   meshtastic --get position
   ```

   > ✓ Reconstruido: `meshtastic --get position` (standard CLI syntax, confident)

4. Pines GPIO correctos (T-Deck Plus):
   - RX: GPIO 34
   - TX: GPIO 12
   - Configura vía web client: Position → GPS Pins

5. Antena GPS: algunos modelos tienen antena cerámica integrada, otros necesitan antena externa.

### Los Mapas No Aparecen

Ver la sección [Troubleshooting de Mapas](#troubleshooting-de-mapas) en [Mapas Offline](#mapas-offline-opcional).

### El Dispositivo No Enciende

**Soluciones:**

1. **Batería:** verifica que la 18650 esté insertada correctamente (polos +/-).
2. **Carga:** conecta USB-C, espera 30 min.
3. **Switch lateral:** confirma la posición ON.
4. **Reset:** pulsa el botón RESET (pequeño orificio lateral).

### Firmware Brick / No Arranca

**Soluciones:**

1. **Full Erase + Flash:** Modo DFU → Full Erase → Flash de firmware beta.
2. **Prueba diferentes firmwares:** 2.5.x vs 2.6.x.
3. **Meshtastic CLI noproto:**

   ```bash
   meshtastic --noproto
   ```

   > ✓ Reconstruido: `meshtastic --noproto` (standard CLI syntax, confident)

4. **Factory reset vía CLI:**

   ```bash
   meshtastic --factory-reset
   ```

   > ✓ Reconstruido: `meshtastic --factory-reset` (standard CLI syntax, confident)

### No Aparecen Nodos / Mesh Vacía

**Soluciones:**

1. **Región correcta:** EU_868 en Portugal.
2. **Antena conectada:** sin antena = sin TX/RX.
3. **Frequency slot:** verifica si la mesh local usa un slot diferente.
4. **Range:** los nodos deben estar dentro del alcance LoRa (~5 km urbano, 20 km+ rural).
5. **Canales:** el Primary Channel debe coincidir con la mesh local.

### Consumo de Batería Alto

**Soluciones:**

1. **Desactiva Bluetooth** si no lo usas.
2. **GPS update interval:** auméntalo a 300 s+ (en vez de 120 s).
3. **Screen timeout:** activa el timeout para que la pantalla se apague.
4. **Role:** define como CLIENT (no ROUTER) si es móvil.

> Para detalles completos, ver [battery-energy-guide.md](battery-energy-guide.md).

---

## Recursos Adicionales

### Enlaces Oficiales

- **Meshtastic Web Flasher:** <https://flasher.meshtastic.org>
- **Meshtastic Web Client:** <https://client.meshtastic.org>
- **Documentación Oficial:** <https://meshtastic.org/docs>
- **Firmware Releases:** <https://github.com/meshtastic/firmware/releases>

### Comunidad Portugal

- **Meshtastic PT:** <https://meshtastic.pt>
- **Mapa de Nodos Portugal:** <https://meshtastic.pt/map>
- **Malha PT:** <https://malha.meshtastic.pt>
- **FAQ PT:** <https://meshtastic.pt/faq>
- **Facebook Meshtastic Portugal:** grupo activo con soporte

### Recursos de Mapas

- **MUI Maps GitHub:** <https://github.com/meshtastic/device-ui/tree/master/maps>
- **MapDL Script:** <https://gist.github.com/droberin/b333a216d860361e329e74f59f4af4ba>
- **MOBAC:** <https://mobac.sourceforge.io>
- **Wind & Wireless Guide:** <https://windandwireless.com/pages/techlab/meshtasticmaps>

### CLI Python

```bash
# Instalar CLI
pip3 install --upgrade "meshtastic[cli]"

# Comandos útiles
meshtastic --info                              # Info del nodo
meshtastic --nodes                             # Lista nodos de la mesh
meshtastic --set lora.region EU_868            # Define región
meshtastic --set bluetooth.enabled false       # Desactiva Bluetooth
meshtastic --get device                        # Configuración completa
meshtastic --factory-reset                     # Reset total
```

### Regulación Portugal (ANACOM)

- **Frecuencia:** 868 MHz (EU_868)
- **Potencia máxima:** 500 mW (27 dBm)
- **Duty cycle:** 10%
- **Alternativa:** 433 MHz, 10 mW, 10% duty cycle

---

## Checklist de Instalación Completa

- [ ] Antena LoRa conectada
- [ ] Batería 18650 insertada
- [ ] Firmware flasheado (beta recomendado)
- [ ] Región LoRa: EU_868
- [ ] GPS activado y pines correctos
- [ ] Nombre del nodo configurado
- [ ] Bluetooth: Programming Mode probado + desactivado (si standalone)
- [ ] Mapas offline instalados (opcional)
- [ ] GPS fix obtenido (exterior)
- [ ] Primer nodo de la mesh detectado
- [ ] App del smartphone pareada (si usas Bluetooth)

---

## Changelog de la Guía

- **v1.0 (Abril 2026):** versión inicial completa.

---

> **Contribuciones:** esta guía es open-source. Mejoras y correcciones son bienvenidas vía la comunidad Meshtastic PT.

---

☥ cypherpunks write code /// nature writes resilience /// we bridge both ☥

Licenciado bajo CC BY-SA 4.0 · <https://creativecommons.org/licenses/by-sa/4.0/>

*walk quietly, but keep the signal alive.*

₿uilt with Love 🧡 in cooperation with Nature 🌿
