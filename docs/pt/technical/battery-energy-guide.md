# T-Deck Plus — Guia de Bateria e Energia

## Meshtastic 2.7.x (inclui MUI / Fancy UI)

> Read this in: [EN](../../en/technical/battery-energy-guide.md)

---

## 0. Objetivo

- Aumentar a autonomia do T-Deck Plus em uso diário.
- Usar de forma correta o MUI / fancy UI sem matar a bateria à toa.
- Entender o comportamento "esquisito" do indicador de percentagem quando ligas o carregador.
- Ter um fluxo claro para usar Bluetooth/app só quando preciso.

---

## 1. Hardware e consumo — visão rápida

Componentes que mais consomem no T-Deck Plus:

- ESP32-S3 (CPU + rádio 2.4 GHz)
- Ecrã TFT grande
- Módulo GPS
- Rádio LoRa (especialmente transmissões a alta potência)
- Wi-Fi e Bluetooth quando ativos

Ideia base:

- Desligar tudo o que não for preciso de forma permanente
- Usar ecrã e Bluetooth só o tempo estritamente necessário
- Usar o interruptor físico lateral como "deep sleep manual"

---

## 2. Poupança de bateria em MUI / Fancy UI

### 2.1. Navegação no MUI

- **Trackball:**
  - Clique = Enter / Selecionar
  - Roda = navegar pelos itens
- **Toque no ecrã** (se ativo):
  - Tap = selecionar itens / ícones
- **Menus típicos:**
  - Dashboard / Home
  - Chats / Messages
  - Map
  - Nodes
  - **Settings** (ícone de engrenagem)
  - Reboot / Shutdown (via Settings ou menu de energia)

---

### 2.2. Ecrã (Display)

**Passos:**

1. No MUI, abre **Settings** (engrenagem)
2. Entra em **Display / Screen / UI** (nome depende da build)
3. Ajusta:
   - **Brightness:** mínimo confortável
   - **Screen On Duration / Screen Timeout / Screen Off:** 15–30 segundos
   - Desativa: **Always On / Prevent Sleep / Keep Screen On** (se existir)
4. Volta ao Dashboard

**Efeito:**

- O ecrã TFT é um dos maiores consumidores
- Menos brilho + menos tempo ligado = grande ganho de autonomia

---

### 2.3. GPS

**Passos:**

1. Em **Settings**, abre **Position / Location / GPS**
2. Se não precisas de localização:
   - Desliga **GPS enabled**
3. Se precisas, mas ocasionalmente:
   - Aumenta **fix interval / position interval / beacon interval** para **15–30 minutos** ou mais
4. Guarda e volta atrás

**Efeito:**

- Cada fix de GPS liga o módulo e consome muito
- Intervalos longos ou GPS desligado poupam bastante bateria

---

### 2.4. Beacon e Telemetria (LoRa)

**Passos:**

1. Em **Settings**, abre **Position** (ou equivalente):
   - Ajusta **Position / Beacon interval** para **15–30 minutos** (ou mais, se fizer sentido)
2. Em **Settings**, abre **Telemetry**:
   - Ajusta **Telemetry interval** para **10–30 minutos**
3. Volta ao Dashboard

**Efeito:**

- Menos transmissões LoRa
- Menos acordar do rádio e CPU
- Menos consumo total

---

### 2.5. Wi-Fi

Se não precisas de Wi-Fi:

1. Em **Settings**, abre **Wi-Fi / Network**
2. Define:
   - **Wi-Fi = Off / Disabled**
3. Volta atrás

**Efeito:**

- O rádio 2.4 GHz (Wi-Fi) deixa de estar acordado a varrer redes
- Redução de consumo constante

---

## 3. Bluetooth com MUI / Fancy UI

### 3.1. Conceito MUI + Bluetooth

- O MUI usa a mesma API do firmware que o cliente Bluetooth/Wi-Fi.
- Resultado:
  - Com MUI a correr, o Bluetooth "normal" para a app não está sempre disponível.
  - Para configurar via app, entras num modo especial: **Bluetooth Programming Mode**.
- Nesse modo:
  - O T-Deck mostra um PIN.
  - A app conecta.
  - Quando sais, voltas ao MUI normal.

---

### 3.2. Entrar em Bluetooth Programming Mode

Existem duas vias (dependendo da tua build MUI):

#### Método 1 — No arranque (logo "M")

1. Desliga o T-Deck pelo **switch lateral**.
2. Liga o switch para arrancar.
3. Quando aparecer o **logótipo Meshtastic (M)** no ecrã:
   - Mantém o dedo carregado em cima do logo.
   - Espera até um círculo completar (animação).
4. Quando o círculo encher, o dispositivo arranca em **Bluetooth Programming Mode**.
5. No ecrã vais ver:
   - Uma indicação de "Programming / Bluetooth Mode".
   - Um PIN de emparelhamento.
6. No telemóvel:
   - Abre a app Meshtastic.
   - Procura o nó.
   - Usa o PIN que vês no ecrã para emparelhar.
7. Depois de configurar:
   - Mantém pressionado o **ícone Bluetooth** no ecrã (ou segue a instrução) para reiniciar de volta ao MUI.

#### Método 2 — Pelo menu Reboot/Shutdown

1. Em MUI, abre **Settings**.
2. Vai a **Reboot / Shutdown** (ou menu de energia).
3. No ecrã de reboot, se existirem vários ícones:
   - Power (desligar)
   - Reboot normal
   - Bluetooth
4. Seleciona o **ícone de Bluetooth**:
   - O T-Deck reinicia em **Bluetooth Programming Mode**.
5. Faz o emparelhamento na app (PIN no ecrã).
6. Para voltar ao MUI:
   - Mantém pressionado o ícone Bluetooth até reiniciar.

**Observações gerais:**

- Recomenda-se ter **Wi-Fi desativado** antes de entrar em Programming Mode para evitar conflitos.
- Enquanto estás em Programming Mode, o MUI não está a correr.

---

### 3.3. Usar o BLE de forma amiga da bateria

- **No dia a dia:**
  - Ficas em MUI normal, sem BLE permanente para a app.
- **Quando precisas de mexer em configs pela app:**
  - Desliga Wi-Fi nas Settings.
  - Entra em **Bluetooth Programming Mode** (método 1 ou 2).
  - Configura tudo na app.
  - Sai de Programming Mode (pressiona ícone Bluetooth) para voltar ao MUI.

Desta forma, o Bluetooth só gasta energia quando realmente precisas dele.

---

## 4. Interruptor físico de alimentação

- O T-Deck Plus tem um **switch lateral** que corta a alimentação da placa inteira.
- Recomendações:
  - Usa o switch para desligar quando o dispositivo vai ficar parado várias horas (noite, mochila, bolsa).
  - Isto é, na prática, um "deep sleep manual" com consumo mínimo.

---

## 5. Comportamento da bateria quando ligas o carregador

### 5.1. Sintoma típico

Coisas que muitas pessoas reportam no T-Deck Plus:

- Estás, por exemplo, a **20%** de bateria.
- Ligas o cabo USB para carregar.
- O indicador de bateria salta **logo para 100%**.
- Fica em 100% o tempo todo enquanto está ligado ao carregador.
- Muitas vezes só volta a mostrar um valor mais realista depois de:
  - Desligares o cabo.
  - E às vezes apenas depois de um **reboot**.

Ou seja:

- É **normal não veres a percentagem a subir** (20 → 21 → 22…).
- Em vez disso, salta para 100% assim que deteta "em carga".
- O valor é pouco fiável durante a carga.

---

### 5.2. Porquê que isto acontece

- O T-Deck Plus **não tem fuel gauge dedicado**.
- O firmware estima a percentagem com base na **tensão da célula medida via ADC**.
- Quando ligas o carregador:
  - A tensão à entrada da linha de bateria sobe muito.
  - A leitura que o ESP32 vê é "poluída" / inflacionada pelo carregador.
  - O firmware lê algo perto de 4.2 V ou mais e assume "100%".
- Resultado:
  - Saltos bruscos para 100% quando ligas o cabo.
  - Quedas bruscas quando desligas e a tensão assenta.
  - Comportamento "saltitão" e pouco fiável.

Isto é uma limitação de projeto (hardware + firmware), não exatamente defeito da tua unidade.

---

### 5.3. Como saber se está realmente carregado

Mais fiável que olhar para a percentagem mostrada:

1. **LED de carga:**
   - Existe um pequeno LED (normalmente azul) visível por uma abertura na caixa.
   - Quando a bateria está a carregar: LED aceso fixo.
   - Quando a bateria atinge carga completa: LED apaga.
   - **Regra prática:** considera a bateria carregada quando o LED apagar, não quando o ecrã diz 100%.

2. **Tensão** (se o firmware mostrar):
   - Em alguns menus de debug/telemetria podes ver a tensão em volts.
   - Por volta de **4,20 V** → bateria cheia.
   - 3,7–3,8 V → a meio.
   - 3,5 V ou menos → perto de descarregada.

---

### 5.4. Regra de uso prático

- Ignora o "100% instantâneo" quando ligas o cabo.
- Usa este fluxo:
  - Liga o T-Deck ao carregador.
  - Deixa a carregar até o **LED de carga apagar**.
  - Se quiseres confirmar, podes reiniciar depois de tirar do cabo e ver o valor de percentagem/tensão em repouso.
- Em uso normal:
  - Começa a pensar em carregar quando a percentagem real (após algum tempo sem cabo) cair abaixo de ~30%.

---

## 6. Checklist rápido de energia e bateria

### 6.1. Poupança em MUI (uso diário)

- [ ] Brilho baixo
- [ ] Screen timeout / Screen off em 15–30 s
- [ ] Wi-Fi OFF, exceto quando realmente precisas
- [ ] GPS:
  - OFF se não precisas de posição, ou
  - Intervalos de fix / beacon ≥ 15–30 min
- [ ] Position / Beacon interval ≥ 15 min (se aceitável)
- [ ] Telemetry interval ≥ 10 min
- [ ] Bluetooth:
  - Não usar BLE permanente com MUI
  - Só usar Bluetooth Programming Mode para configurar via app
- [ ] Switch físico lateral em OFF quando não usas o T-Deck durante horas

### 6.2. Carregamento e leitura de bateria

- [ ] Aceitar que a percentagem salta para 100% quando ligas o cabo
- [ ] Usar o **LED de carga** como indicador principal de "bateria cheia"
- [ ] Opcional: confirmar tensão em volts se o firmware mostrar
- [ ] Não confiar no valor de percentagem durante a carga
- [ ] Após tirar do cabo, se precisares de leitura mais real, podes:
  - Esperar alguns minutos em repouso, ou
  - Fazer um reboot rápido e ver a percentagem/tensão

---

## 7. Notas finais

- O T-Deck Plus é potente mas não é um "smartphone" em termos de gestão de bateria.
- Grande parte da autonomia depende:
  - De como usas o ecrã.
  - De quão agressivos são os intervalos de GPS/LoRa.
  - De manter Wi-Fi/Bluetooth desligados quase sempre.
- O indicador de percentagem é apenas aproximado — foca-te no LED de carga e na tensão (se disponível).

---

Licenciado sob CC BY-SA 4.0 · <https://creativecommons.org/licenses/by-sa/4.0/>

*walk quietly, but keep the signal alive.*

₿uilt with Love 🧡 in cooperation with Nature 🌿
