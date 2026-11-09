# Tilt2Drive

**Use the DualSense (PS5) controller's gyroscope as a racing wheel on Linux — with a
full configuration GUI, button remapping, virtual pedals and in-game menu control.**

English summary below • [Descrição completa em português](#em-português)

---

## What it does

Tilt2Drive reads the DualSense controller **read-only** (Bluetooth or USB, raw hidraw)
and drives a `uinput` virtual device that racing games understand:

- **Steering by tilt** — complementary filter (gyro + accelerometer) turns the
  controller's tilt into wheel steering, with angle-based or rate-based modes,
  adjustable response curve (linear by default), softening and deadzone.
- **Virtual pedals** — L2/R2 analog triggers become brake/throttle on dedicated
  axes (0–1023), with per-pedal deadzone, response curve and measured-rest
  subtraction (no pedal creep).
- **Wheel signature** — the virtual device identifies itself with a classic racing
  wheel signature (Logitech G25/G27 family), so games like F1 25 with
  Proton/Wine classify it as a wheel instead of a laggy gamepad stick.
- **Full button remapping, live** — every DualSense button (face, d-pad, sticks
  click, bumpers, Create/Options/PS, touchpad) can emit: Xbox-family gamepad
  buttons, **G27 wheel button codes** (`w1`–`w12`) or **real keyboard keys**
  (`Enter`, `Esc`, arrows — via an extra virtual keyboard device) for navigating
  in-game menus.

### GUI (Tkinter, no web stack)

- Racing HUD: pedal bars (processed values = what the game receives), steering
  gauge with angle history, live telemetry and a rotating wheel.
- **Clickable controller map**: buttons light up blue when pressed on a drawn
  controller; click one to choose its function (double-click or Enter applies,
  live, no restart).
- Sliders **and typeable exact values** for every tuning knob (max angle,
  deadzones, softening, response curve, wheel rotation, pedal curves).
- Per-mode settings (xbox / wheel), config presets (save/load/delete as plain
  `.conf` files), one-click reconnect/neutral recalibration.

### Guarantees

- The real controller is **never written to** — no LED, haptics or any command;
  the hidraw handle is read-only.
- Nothing is uploaded or phoned home; everything runs locally.
- Settings for Steam ("PlayStation controller support") stay off so the game
  sees only the virtual wheel.

## Requirements

- Linux with `uinput` writable by your user
  (`sudo usermod -a -G input $USER`, re-login)
- Python 3 with `tkinter` + `customtkinter`; Pillow too (GUI only)
  (`pip install --user customtkinter Pillow` — the engine is pure stdlib)
- A game or emulator that sees evdev devices (F1 25 via Proton works well)

## Install

```bash
git clone https://github.com/Luiz121213/tilt2drive.git
cd tilt2drive
pip install --user customtkinter Pillow   # GUI deps
./code/tilt2drive-gui            # opens the window (engine starts inside)
./code/tilt2drive --debug        # CLI with HUD in the terminal
```

Calibrate: launch the GUI, hold the controller still in playing position when it
asks; then open the game, pick the **wheel** input class and bind controls.

## Repository layout

| path | what |
|---|---|
| `code/tilt2drive` | CLI: hidraw reader, sensor fusion (`Sensor`), virtual device (`Steer`), defaults, config loader |
| `code/tilt2drive-gui` | Tkinter GUI (imports the CLI module): engine thread, HUD, button map, presets |
| `assets/icons/*` | Pillow-drawn button icons for the GUI map |
| `README.md`, `LICENSE` | you are here |

## In-game mapping tips (F1 25)

- Menus: map a face button to **Enter (teclado)**, another to **Esc (teclado)**,
  and the d-pad to the arrow keys.
- The `w1`–`w12` wheel-family codes are emitted on the main virtual device — a
  bet on G27-style codes; keyboard keys are near-certain.
- Tailored keys are re-applied live while the game is running (remap and test
  instantly).

---

## Em português

**Tilt2Drive transforma o giroscópio do controle DualSense (PS5) em um volante
no Linux**, com interface gráfica completa — para jogar F1 25 (e corridas em
geral) inclinando o controle, com pedais no L2/R2 e navegação de menus pelos
botões do próprio controle.

**O que ele faz:**

- **Direção por inclinação** — filtro complementar (giroscópio + acelerômetro)
  aponta com precisão quanto você inclinou; modos por ângulo ou por taxa;
  **curva de resposta ajustável** (1.0 = linear; >1 começa mais suave; <1 é
  agressivo), suavização adaptativa e zona morta.
- **Pedais virtuais** — L2 = freio, R2 = acelerador, em eixos analógicos
  próprios, com zona morta, curva e subtração de repouso medido (os pedais não
  "andam" parados).
- **Assinatura de volante** — o device virtual se apresenta como um volante
  clássico (família Logitech G25/G27), então jogos sob Proton/Wine (F1 25)
  classificam isso como wheel e não como stick de gamepad cheio de lag.
- **Remapeamento total dos botões, ao vivo** — cada botão do DualSense pode
  emitir: botões de gamepad (família Xbox), **códigos de botões de volante
  G27** (`w1`–`w12`) ou **teclas de teclado de verdade** (`Enter`, `Esc`, setas
  — através de um teclado virtual extra), para controlar os menus do jogo.

**A interface (Tkinter, tudo local):**

- HUD de corrida: barras de pedal (mostram o valor processado, o que o jogo
  recebe), card de esterço com histórico de ângulo, telemetria e volante girando.
- **Mapa do controle clicável**: os botões acendem em azul quando pressionados;
  clicar num botão abre a lista de funções (duplo clique/Enter aplica na hora).
- Cada ajuste tem slider **e campo para digitar o valor exato** (ângulo máximo,
  zonas mortas, suavização, curva, rotação do volante, curvas de pedal).
- Configurações separadas por modo (xbox / volante) e **presets** comuns
  (salvar/carregar/apagar em arquivos `.conf`).

**Garantias:** o controle real é lido **somente em leitura** — nada é escrito
para ele (sem LED, háptico ou comando); não há upload de nada; a configuração
"Suporte a Controle PlayStation" do Steam deve ficar desligada para o jogo
enxergar apenas o volante virtual.

**Instalação:**

```bash
git clone https://github.com/Luiz121213/tilt2drive.git
cd tilt2drive
pip install --user customtkinter Pillow   # deps da GUI (motor é stdlib pura)
./code/tilt2drive-gui            # abre a janela (o motor inicia nela)
```

Requisitos: Linux com `/dev/uinput` escrevível pelo seu usuário; Python 3 com
`tkinter` + `customtkinter`, com Pillow — `pip install --user`
`customtkinter Pillow`. Na primeira calibração segure o controle
parado, na posição de jogar.

**Dica para menus no F1 25:** mapeie ○ → "Enter (teclado)", □ → "Esc (teclado)",
d-pad → setas. Os códigos `w1`–`w12` são uma aposta na família G27; as teclas
de teclado são o caminho quase certo. Vale remapear e testar na hora — o
remap aplica ao vivo, sem fechar o jogo.

**Aviso:** projeto de usuário, sem vínculo com Sony/PlayStation ou
Logitech. "DualSense" e demais marcas pertencem aos respectivos donos.

Licença: [MIT](LICENSE).
