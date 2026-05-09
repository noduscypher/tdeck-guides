> ⚠️ Antes de começar  
> Este guia foca-se no uso do T‑Deck / T‑Deck Plus com Meshtastic (firmware, UI, mapas, bateria, troubleshooting).  
> Não define limites legais nem boas práticas de planeamento de rede.  
> Para presets EU868, roles (CLIENT, ROUTER, etc.), alcance conservador e notas regulatórias em PT/EN/ES, usa sempre:  
> https://github.com/noduscypher/mesh-guides
# Guia Completo T-Deck Plus Meshtastic

**Versão 1.0 | Abril 2026**

> Read this in: [EN](../../en/technical/complete-guide.md) · [ES](../../es/technical/complete-guide.md) · [JA](../../ja/technical/complete-guide.md)

> 📝 **Nota sobre esta versão:** o ficheiro original perdeu formatação significativa (várias tabelas, blocos de código e diagramas ASCII em falta). Esta versão reconstrói a estrutura em Markdown limpo. Onde houve perda total de conteúdo, está marcado com ⚠️ para revisão antes da publicação. Marcadores ✓ indicam reconstruções de alta confiança.

---

## Índice

1. [Introdução](#introdução)
2. [Requisitos](#requisitos)
3. [Modo Programming (DFU/Flash)](#modo-programming-dfuflash)
4. [Instalação Firmware — Erase + Flash](#instalação-firmware--erase--flash)
5. [Atualização Firmware (Normal)](#atualização-firmware-normal)
6. [Configuração Inicial](#configuração-inicial)
7. [Bluetooth — Modo Programming](#bluetooth--modo-programming)
8. [Bluetooth — Desativar para Uso Standalone](#bluetooth--desativar-para-uso-standalone)
9. [Mapas Offline (Opcional)](#mapas-offline-opcional)
10. [Atalhos de Teclado](#atalhos-de-teclado)
11. [Troubleshooting](#troubleshooting)
12. [Recursos Adicionais](#recursos-adicionais)
13. [Checklist de Instalação Completa](#checklist-de-instalação-completa)

---

## Introdução

O LilyGO T-Deck Plus é um dispositivo standalone completo para Meshtastic com ecrã, teclado QWERTY, GPS integrado e LoRa. Este guia cobre desde a instalação inicial até configuração avançada.

**Características principais:**

- ESP32-S3 + SX1262 LoRa
- GPS L76K integrado
- Ecrã TFT 2.8" touchscreen
- Teclado físico QWERTY
- Bateria 18650 (não incluída)
- MicroSD para mapas offline

---

## Requisitos

### Hardware

- LilyGO T-Deck Plus
- Cabo USB-C (dados, não apenas carga)
- Bateria 18650 (recomendado: 3000 mAh+)
- Antena LoRa 868 MHz (Europa) ou 915 MHz (EUA)
- MicroSD card FAT32 ≤32 GB (opcional, para mapas)

### Software

- Browser Chromium (Chrome, Edge, Brave)
- Internet ativa
- Meshtastic App (Android/iOS) para pairing Bluetooth

### Avisos Importantes

> ⚠️ **Nunca ligues o dispositivo sem antena conectada** — pode danificar o módulo LoRa.
>
> ⚠️ Usa sempre cabo USB-C com suporte de dados (não apenas carga).
>
> ⚠️ Define a região correta (`EU_868` para Portugal) para cumprir regulamentação ANACOM.

---

## Modo Programming (DFU/Flash)

Para flashear firmware ou fazer erase, o T-Deck Plus precisa entrar em modo DFU.

### Procedimento

1. Desliga o T-Deck (switch lateral em OFF).
2. Pressiona e mantém o trackball (botão central).
3. Enquanto pressionas o trackball, liga o T-Deck (switch ON).
4. Aguarda 2–3 segundos mantendo o trackball pressionado.
5. Liberta o trackball.
6. Conecta o cabo USB-C ao computador.

### Confirmação de Modo DFU

- O ecrã do T-Deck fica completamente negro (sem backlight).
- No computador aparece um dispositivo: **"USB JTAG/serial debug unit"** ou **"ESP32-S3"**.

> **Nota:** se não funcionar à primeira, repete o processo. Alguns cabos USB-C para USB-C podem causar problemas — tenta um cabo USB-A para USB-C.

---

## Instalação Firmware — Erase + Flash

**Quando usar:** primeira instalação, dispositivo com problemas graves, mudança de tipo de firmware (ex.: alpha → beta).

### Passo a Passo

1. **Entra em Modo Programming**
   Segue o procedimento da secção anterior.

2. **Acede ao Web Flasher**
   - Abre browser Chromium
   - Vai a: <https://flasher.meshtastic.org>

3. **Seleciona Dispositivo**
   - Clica em "Select Target Device"
   - Escolhe: **"LILYGO T-Deck"** (funciona para T-Deck Plus)

4. **Escolhe Versão Firmware**
   - **Recomendado:** seleciona a versão BETA mais recente (evita alpha em produção)
   - Não uses versões alpha para uso diário

5. **Configura Full Erase**
   - ✅ Ativa a opção: **"Full Erase and Install"**
   - Isto apaga toda a flash, incluindo configurações antigas

6. **Conecta Dispositivo**
   - Clica em "Connect"
   - Seleciona porta: "USB JTAG/serial debug unit"
   - Clica em "Connect"

7. **Flash**
   - Clica em "Flash" e depois em "CONTINUE"
   - Aguarda a conclusão (1–3 minutos)
   - **NÃO desconectes USB durante o processo**

8. **Finalização**
   - Quando vires "Flash Complete", fecha o browser
   - Desliga o T-Deck (switch OFF)
   - Liga novamente (switch ON)
   - O dispositivo arranca com firmware fresco e MUI (Meshtastic UI)

---

## Atualização Firmware (Normal)

**Quando usar:** atualizar para a versão mais recente sem perder configurações.

### Passo a Passo

1. **Entra em Modo Programming**
   Segue o procedimento: desliga → pressiona trackball → liga → aguarda → liberta → conecta USB.

2. **Web Flasher**
   - Vai a: <https://flasher.meshtastic.org>
   - Seleciona: "LILYGO T-Deck"
   - Escolhe a versão de firmware desejada

3. **Update (Sem Erase)**
   - ❌ **Desativa** a opção "Full Erase"
   - Clica em "Flash"

4. **Flash e Reboot**
   - Aguarda a conclusão
   - Desliga e liga o T-Deck
   - As configurações mantêm-se (região, nome do node, canais, etc.)

> **Nota:** se tiveres problemas após o update, faz Full Erase + Flash.

---

## Configuração Inicial

Após o flash inicial, configura os parâmetros essenciais.

### Via Meshtastic Web Client

1. **Conecta via USB Serial**
   - Vai a: <https://client.meshtastic.org>
   - Clica em "Connect" → "USB Serial"
   - Seleciona a porta do T-Deck

2. **Define Região LoRa**
   - Menu lateral: **"LoRa"**
   - Region: seleciona **EU_868** (Portugal/Europa)
   - Clica em "Save"

   > ⚠️ **Obrigatório em Portugal:** EU_868 (868 MHz, 500 mW, 10% duty cycle), conforme ANACOM.

3. **Configura Posição GPS**
   - Menu: **"Position"**
   - GPS Mode: "Enabled"
   - GPS Update Interval: 120 s (ajusta conforme necessidade)
   - Position Broadcast: ativa se quiseres partilhar localização
   - Clica em "Save"

4. **Node Info (Opcional)**
   - Menu: **"Settings" → "Device"**
   - Node Name: define o nome do teu node
   - Role: CLIENT (uso normal) ou ROUTER (node fixo)

5. **Canais (Opcional)**
   - Menu: **"Channels"**
   - Configura canais adicionais ou junta-te a uma mesh existente
   - O Primary Channel vem pré-configurado

### Via MUI no T-Deck

Após a configuração inicial via web/app, podes ajustar no próprio T-Deck:

- **Settings → LoRa** → verifica a região
- **Settings → Position** → estado do GPS
- **Map** → visualiza nodes e posição GPS

---

## Bluetooth — Modo Programming

**Quando usar:** pairing inicial com smartphone, configuração via app, troubleshooting.

### Ativar Bluetooth Programming Mode

#### Via MUI (Firmware 2.6+)

1. No T-Deck, vai a: **Settings** (ícone engrenagem)
2. Seleciona: **"Reboot/Shutdown"**
3. Clica no ícone Bluetooth (centro do ecrã)
4. O ecrã mostra: **">> Programming Mode <<"** com passcode (ex.: 123456)

#### Pairing com Smartphone

1. Abre a Meshtastic App (Android/iOS)
2. Vai a Bluetooth → Scan
3. Seleciona o teu T-Deck (nome do node)
4. Insere o passcode mostrado no ecrã do T-Deck
5. Conecta e configura via app

### Sair de Bluetooth Programming Mode

1. Pressiona e mantém o ícone Bluetooth no ecrã do T-Deck
2. O dispositivo reinicia de volta ao MUI normal

> **Importante:**
> - Bluetooth Programming Mode é para pairing/configuração inicial.
> - Para uso diário com Bluetooth, muda para BaseUI (próxima secção).
> - O MUI não suporta Bluetooth ativo permanentemente (usa a mesma API).

---

## Bluetooth — Desativar para Uso Standalone

**Quando usar:** operação standalone sem smartphone, economizar bateria, usar MUI com mapas.

### Opção 1: Usar MUI (Modo Standalone Recomendado)

O MUI (Meshtastic UI) é a interface visual completa, mas não suporta Bluetooth simultâneo.

- **Vantagens:** mapas, teclado, visualização completa da mesh
- **Desvantagem:** sem Bluetooth para a app

Para garantir que o Bluetooth está desligado:

**Via Meshtastic CLI:**

```bash
meshtastic --set bluetooth.enabled false
```

> ✓ Reconstruído: `meshtastic --set bluetooth.enabled false` (standard CLI syntax, confident)

**Ou via Web Client:**

- Settings → Bluetooth → Enabled: OFF → Save

### Opção 2: BaseUI (Bluetooth Permanente)

Se precisas de Bluetooth ativo para a app 24/7:

#### Mudar para BaseUI

**Via CLI:**

> ⚠️ **[VERIFICAR]** Comando CLI ausente na fonte. Confirmar sintaxe correta contra a documentação atual do Meshtastic — a chave de configuração para mudar para BaseUI pode variar entre versões. Em alternativa, usa o caminho via Web Client abaixo.

**Ou via Web Client:**

- Settings → Device → Display Type: BaseUI
- Save e reboot

**Características do BaseUI:**

- Interface simples (sem mapas visuais)
- Bluetooth funciona permanentemente
- Menor consumo de recursos
- Ideal para controlo via app

### Comparação Rápida

> ⚠️ Tabela reconstruída a partir do contexto descritivo do próprio documento. Verificar contra documentação oficial antes de publicar.

| Característica | MUI | BaseUI |
|---|---|---|
| Mapas visuais | ✅ | ❌ |
| Teclado e navegação completa | ✅ | Parcial |
| Bluetooth permanente para app | ❌ (só Programming Mode) | ✅ |
| Consumo de recursos | Mais alto | Mais baixo |
| Caso de uso | Standalone com mapas | Controlo via app |

> **Recomendação:** usa MUI para standalone com mapas. Muda para BaseUI se precisas que a app no smartphone esteja sempre conectada.

---

## Mapas Offline (Opcional)

Adicionar mapas offline ao T-Deck Plus permite visualizar a mesh network sobre cartografia real.

### Requisitos

- MicroSD card FAT32, ≤32 GB
- Pack de tiles de mapa (OpenStreetMap, Google Maps, Atlas)

### Estrutura de Pastas Correta

A estrutura de tiles segue o formato standard XYZ do OpenStreetMap:

```
/maps/
└── <nome-do-pack>/
    ├── 0/                  ← zoom level 0 (mundo inteiro)
    ├── 1/
    ├── 2/
    ├── ...
    ├── 8/                  ← zoom levels essenciais para Portugal
    ├── 9/
    ├── 10/
    ├── 11/
    ├── 12/                 ← zoom alto, muitos tiles
    ├── 13/
    ├── 14/
    └── 15/                 ← zoom máximo
```

**Importante:**

- Cada pasta numerada (0–15) corresponde a um zoom level.
- Dentro de cada pasta de zoom, há subpastas `X/` e ficheiros `Y.png` (formato XYZ).
- Zoom levels baixos (0–5) cobrem o mundo inteiro com poucos tiles.
- Zoom levels altos (8–15) contêm muitos tiles e são essenciais para detalhe local.
- Para Portugal, os zoom 8–14 são o que realmente interessa.
- Zoom 1–5 podem estar vazios sem prejuízo.

### Instalação Passo a Passo

#### Método 1: Pack Pronto Atlas (Recomendado Global)

1. **Baixa o pack de mapas:**
   - Atlas (global, inclui Europa): <https://github.com/meshtastic/device-ui/tree/master/maps>
   - Ficheiro: `atlas.zip` ⚠️ *[nome exato do ficheiro a verificar no repo]*

2. **Extrai o ZIP:**
   - Descompacta `atlas.zip`
   - Vais ter uma pasta `atlas/` com subpastas `0/` a `15/`

3. **Copia para o SD:**
   - Cria a pasta `/maps/` na raiz do SD
   - Copia a pasta `atlas/` completa para `/maps/atlas/`
   - Estrutura final: `/maps/atlas/[0-15]/...`

4. **Verifica o conteúdo:**
   - Abre `/maps/atlas/12/` (ou outro zoom level alto)
   - Deve ter subpastas e ficheiros `.png`
   - Se estiver vazio, o pack está corrompido

#### Método 2: Pack Portugal (mapsportugal115OSM.zip)

Para usar o pack específico de Portugal:

1. **Extrai o ficheiro ZIP:**
   - Descompacta `mapsportugal115OSM.zip`
   - Vais obter uma estrutura de pastas

2. **Navega até à pasta `openstreetmap/`** dentro do ZIP extraído.

3. **Copia o conteúdo da pasta `openstreetmap/`:**
   - Entra na pasta `openstreetmap/` do ZIP extraído
   - Seleciona TODAS as pastas numeradas (1, 2, 3... até 15)
   - Copia (Ctrl+C / Cmd+C)

4. **Cola no SD dentro da pasta "Google Maps":**
   - Insere o SD no computador
   - Navega para: `/maps/Google Maps/` no SD
   - Cola (Ctrl+V / Cmd+V) todo o conteúdo copiado

5. **Estrutura final no SD:**

   ```
   /maps/
   └── Google Maps/
       ├── 1/
       ├── 2/
       ├── ...
       └── 15/
   ```

6. **Verifica a instalação:**
   - Abre `/maps/Google Maps/12/` (ou outro zoom level alto) no SD
   - Deve ter subpastas numeradas e ficheiros
   - Se estiver vazio, repete a extração e cópia

7. **Usar no T-Deck:**
   - Insere o SD no T-Deck
   - Vai a Map
   - Long-press no ícone do mapa → seleciona "Google Maps"
   - Faz zoom para Portugal e aguarda o GPS fix

> **Nota:** o nome da pasta "Google Maps" no SD é apenas um label — podes renomear para "Portugal" ou outro nome se preferires. O importante é a estrutura interna (pastas 1–15 com tiles).

#### Método 3: Gerar Tiles Personalizados

**Script Python MapDL:**

```bash
# Baixa o script
wget https://gist.githubusercontent.com/droberin/b333a216d860361e329e74f59f4af4ba/raw/MapDL.py

# Edita coordenadas para Portugal
# lat: 37 a 42
# lon: -9.5 a -6
# zoom: 8-14

python3 MapDL.py

# Copia output para /maps/portugal/
```

**MOBAC (Mobile Atlas Creator):**

- Download: <https://mobac.sourceforge.io>
- Seleciona região: Portugal
- Zoom levels: 8–14
- Output format: OSM Tile format
- Exporta e copia para o SD

### Usar Mapas no T-Deck

1. Insere o SD no T-Deck (slot lateral).
2. Liga o dispositivo.
3. Vai a Map (ícone do mapa).
4. Long-press no ícone do mapa (ou Settings → Map).
5. Seleciona a fonte: `atlas`, `Google Maps`, `portugal`, ou outro nome que tenhas usado.
6. Faz zoom in para Portugal (trackball + scroll).
7. GPS fix lá fora — a posição aparece no mapa.

### Troubleshooting Mapas

**Mapa não aparece:**

- SD formatada em FAT32? (não NTFS/exFAT)
- Estrutura `/maps/<nome>/[0-15]/` correta?
- Pastas de zoom 8–14 têm ficheiros `.png`?

**Mapa carrega devagar:**

- SD lenta (recomenda-se class 10 ou superior).
- Demasiados zoom levels (apaga 1–7 se não usares).

**Coordenadas erradas:**

- Aguarda GPS fix (pode demorar 5–10 min inicial).
- Vai para o exterior com vista de céu aberto.

---

## Atalhos de Teclado

> ⚠️ **[CONTEÚDO ORIGINAL PERDIDO — verificar antes de publicar]**
>
> A tabela completa de atalhos de teclado do T-Deck Plus não foi recuperada do source danificado. Sugiro repor a partir de uma destas fontes:
>
> - Repositório oficial do device-ui: <https://github.com/meshtastic/device-ui>
> - Documentação oficial Meshtastic: <https://meshtastic.org/docs>
> - Discussões na comunidade Meshtastic Portugal: <https://meshtastic.pt>

### Notas Gerais (recuperadas do source)

- **ALT + tecla / SYM + tecla:** pressiona e liberta (não é necessário manter pressionado).
- Os atalhos `SYM + tecla` não estão ativos nos tabs SND e SET.
- A tecla **Fn** pode precisar de ativação no firmware 2.7+.

---

## Troubleshooting

### Dispositivo Não Entra em Modo DFU

**Sintomas:** ecrã liga normal, computador não deteta dispositivo de flash.

**Soluções:**

1. Repete a sequência: desliga → trackball → liga → aguarda 3 s → liberta.
2. Testa cabo: usa cabo USB-A para USB-C (em vez de C-C).
3. Drivers: Windows pode precisar de drivers CH340/CP210x.
4. Tenta 3–5 vezes: o timing é crítico.

### Bluetooth Não Conecta

**Sintomas:** a app não vê o dispositivo, pairing falha.

**Soluções:**

**Se em MUI:**

- O Bluetooth não funciona em MUI normal.
- Usa Bluetooth Programming Mode: Settings → Reboot → ícone Bluetooth.
- Ou muda para BaseUI permanentemente.

**Se Bluetooth desativado:**

```bash
meshtastic --set bluetooth.enabled true
```

> ✓ Reconstruído: `meshtastic --set bluetooth.enabled true` (standard CLI syntax, confident)

**Se após firmware update:**

- Pode desativar-se sozinho.
- Reativa via CLI ou web client.
- Se persistir, faz rollback para firmware anterior (2.5.18 conhecido por funcionar).

**Problemas com iPhone:**

- Alguns firmwares (2.6.x early) têm bugs com iOS.
- Usa o web client via USB como alternativa.
- Atualiza para o latest beta firmware.

### GPS Não Fixa

**Sintomas:** posição não atualiza, "No GPS fix" permanente.

**Soluções:**

1. Vai para o exterior: o GPS precisa de vista de céu aberto.
2. Aguarda 5–15 min: o first fix demora (cold start).
3. Verifica configuração:

   ```bash
   meshtastic --get position
   ```

   > ✓ Reconstruído: `meshtastic --get position` (standard CLI syntax, confident)

4. Pins GPIO corretos (T-Deck Plus):
   - RX: GPIO 34
   - TX: GPIO 12
   - Configura via web client: Position → GPS Pins

5. Antena GPS: alguns modelos têm antena cerâmica integrada, outros precisam de antena externa.

### Mapas Não Aparecem

Ver secção [Troubleshooting Mapas](#troubleshooting-mapas) em [Mapas Offline](#mapas-offline-opcional).

### Dispositivo Não Liga

**Soluções:**

1. **Bateria:** verifica que a 18650 está inserida corretamente (polos +/-).
2. **Carregamento:** conecta USB-C, aguarda 30 min.
3. **Switch lateral:** confirma a posição ON.
4. **Reset:** pressiona o botão RESET (pequeno buraco lateral).

### Firmware Brick / Não Arranca

**Soluções:**

1. **Full Erase + Flash:** Modo DFU → Full Erase → Flash firmware beta.
2. **Testa firmwares diferentes:** 2.5.x vs 2.6.x.
3. **Meshtastic CLI noproto:**

   ```bash
   meshtastic --noproto
   ```

   > ✓ Reconstruído: `meshtastic --noproto` (standard CLI syntax, confident)

4. **Factory reset via CLI:**

   ```bash
   meshtastic --factory-reset
   ```

   > ✓ Reconstruído: `meshtastic --factory-reset` (standard CLI syntax, confident)

### Nodes Não Aparecem / Mesh Vazio

**Soluções:**

1. **Região correta:** EU_868 em Portugal.
2. **Antena conectada:** sem antena = sem TX/RX.
3. **Frequência slot:** verifica se a mesh local usa um slot diferente.
4. **Range:** os nodes precisam de estar dentro do alcance LoRa (~5 km urbano, 20 km+ rural).
5. **Canais:** o Primary Channel deve coincidir com a mesh local.

### Consumo de Bateria Alto

**Soluções:**

1. **Desativa Bluetooth** se não for usado.
2. **GPS update interval:** aumenta para 300 s+ (em vez de 120 s).
3. **Screen timeout:** ativa o timeout para o ecrã se desligar.
4. **Role:** define como CLIENT (não ROUTER) se for móvel.

> Para detalhes completos, ver [battery-energy-guide.md](battery-energy-guide.md).

---

## Recursos Adicionais

### Links Oficiais

- **Meshtastic Web Flasher:** <https://flasher.meshtastic.org>
- **Meshtastic Web Client:** <https://client.meshtastic.org>
- **Documentação Oficial:** <https://meshtastic.org/docs>
- **Firmware Releases:** <https://github.com/meshtastic/firmware/releases>

### Comunidade Portugal

- **Meshtastic PT:** <https://meshtastic.pt>
- **Mapa Nodes Portugal:** <https://meshtastic.pt/map>
- **Malha PT:** <https://malha.meshtastic.pt>
- **FAQ PT:** <https://meshtastic.pt/faq>
- **Facebook Meshtastic Portugal:** grupo ativo com suporte

### Recursos de Mapas

- **MUI Maps GitHub:** <https://github.com/meshtastic/device-ui/tree/master/maps>
- **MapDL Script:** <https://gist.github.com/droberin/b333a216d860361e329e74f59f4af4ba>
- **MOBAC:** <https://mobac.sourceforge.io>
- **Wind & Wireless Guide:** <https://windandwireless.com/pages/techlab/meshtasticmaps>

### CLI Python

```bash
# Instalar CLI
pip3 install --upgrade "meshtastic[cli]"

# Comandos úteis
meshtastic --info                              # Info do node
meshtastic --nodes                             # Lista nodes da mesh
meshtastic --set lora.region EU_868            # Define região
meshtastic --set bluetooth.enabled false       # Desativa Bluetooth
meshtastic --get device                        # Configuração completa
meshtastic --factory-reset                     # Reset total
```

### Regulamentação Portugal (ANACOM)

- **Frequência:** 868 MHz (EU_868)
- **Potência máxima:** 500 mW (27 dBm)
- **Duty cycle:** 10%
- **Alternativa:** 433 MHz, 10 mW, 10% duty cycle

---

## Checklist de Instalação Completa

- [ ] Antena LoRa conectada
- [ ] Bateria 18650 inserida
- [ ] Firmware flasheado (beta recomendado)
- [ ] Região LoRa: EU_868
- [ ] GPS ativado e pinos corretos
- [ ] Nome do node configurado
- [ ] Bluetooth: Programming Mode testado + desativado (se standalone)
- [ ] Mapas offline instalados (opcional)
- [ ] GPS fix obtido (exterior)
- [ ] Primeiro node mesh detectado
- [ ] App smartphone pareada (se usares Bluetooth)

---

## Changelog do Guia

- **v1.0 (Abril 2026):** versão inicial completa.

---

> **Contribuições:** este guia é open-source. Melhorias e correções são bem-vindas via comunidade Meshtastic PT.

---

☥ cypherpunks write code /// nature writes resilience /// we bridge both ☥

Licenciado sob CC BY-SA 4.0 · <https://creativecommons.org/licenses/by-sa/4.0/>

*walk quietly, but keep the signal alive.*

₿uilt with Love 🧡 in cooperation with Nature 🌿
