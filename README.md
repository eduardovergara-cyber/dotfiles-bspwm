# 🖥️ dotfiles-bspwm

Mi entorno de trabajo en Linux con el gestor de ventanas en mosaico **bspwm**, que uso en **Arch Linux** y **Parrot OS** sobre máquinas virtuales de VMware.

Está basado en el tutorial de bspwm de **s4vitar**, con los temas de polybar de **adi1090x** (ver [Créditos](#-créditos)). Lo armé siguiendo ese tutorial y lo adapté a mi entorno.

---

## 🧱 Componentes

| Componente | Programa | Configuración |
|------------|----------|---------------|
| Gestor de ventanas | bspwm | [`bspwm/bspwmrc`](bspwm/bspwmrc) |
| Atajos de teclado | sxhkd | [`sxhkd/sxhkdrc`](sxhkd/sxhkdrc) |
| Barra | polybar | [`polybar/`](polybar/) |
| Scripts de la barra | sh / bash | [`scripts/`](scripts/) |
| Compositor | picom | [`picom/picom.conf`](picom/picom.conf) |
| Lanzador y menú de apagado | rofi | [`polybar/scripts/`](polybar/scripts/) |
| Terminal | kitty | instalada en `~/.local/kitty.app` |
| Notificaciones | dunst | configuración por defecto |
| Fondo de pantalla | feh | definido en `bspwmrc` |

## 📊 La barra (polybar)

La barra está dividida en varios bloques flotantes:

| Bloque | Qué muestra | Script |
|--------|-------------|--------|
| IP local | La IP de la interfaz `ens33` (la red de VMware) | [`ethernet_status.sh`](scripts/ethernet_status.sh) |
| VPN | La IP de `tun0` cuando hay una VPN conectada; si no, "Disconnected" | [`vpn_status.sh`](scripts/vpn_status.sh) |
| Objetivo | La IP y el nombre de la máquina de laboratorio en la que estoy trabajando | [`victim_to_hack.sh`](scripts/victim_to_hack.sh) |
| Escritorios | Los escritorios de bspwm, con íconos | [`workspace.ini`](polybar/workspace.ini) |
| Menú | Botón que abre el menú de apagado de rofi | [`powermenu_alt`](polybar/scripts/powermenu_alt) |

El bloque **Objetivo** lee la IP y el nombre desde `~/.config/bin/target`, un archivo con el formato `IP nombre` que se escribe al empezar a trabajar en una máquina de práctica:

```bash
echo "10.10.10.10 NombreMaquina" > ~/.config/bin/target
```

Ese archivo no se incluye en el repo.

## ✏️ Mis cambios sobre el tutorial

- **`alt` como tecla modificadora** en lugar de `super`, porque dentro de VMware el sistema anfitrión captura la tecla Windows
- **Integración con VMware** (`vmware-user`) para el portapapeles y el ajuste de resolución
- **Comentarios en español** en [`bspwmrc`](bspwm/bspwmrc), explicando cada sección

## ⚙️ Configuración destacada

- **10 escritorios virtuales** y márgenes de 12 px entre ventanas, sin bordes
- **División automática por el lado más largo** (`automatic_scheme longest_side`)
- **El foco sigue al mouse** (`focus_follows_pointer`)
- **sxhkd compilado desde el código fuente** e iniciado solo si no está corriendo (`pgrep -x sxhkd || ...`)
- **Redimensión de ventanas** con [`bspwm/scripts/bspwm_resize`](bspwm/scripts/bspwm_resize): pasos de 20 px si la ventana es flotante o de 100 px si está en mosaico, y si no puede crecer hacia un lado lo intenta hacia el opuesto

## ⌨️ Atajos principales

| Atajo | Acción |
|-------|--------|
| `alt + Enter` | Abrir terminal (kitty) |
| `alt + espacio` | Lanzador de aplicaciones (rofi) |
| `alt + shift + f` | Abrir Firefox |
| `alt + q` | Cerrar ventana |
| `alt + flechas` | Mover el foco entre ventanas |
| `alt + shift + flechas` | Intercambiar ventanas |
| `alt + 1-9, 0` | Cambiar de escritorio |
| `alt + shift + 1-9, 0` | Enviar la ventana a otro escritorio |
| `alt + m` | Alternar entre mosaico y monocle |
| `alt + t / s / f` | Ventana en mosaico / flotante / pantalla completa |
| `alt + Escape` | Recargar la configuración de sxhkd |
| `alt + shift + r` | Reiniciar bspwm |

## 📦 Instalación

```bash
# Arch Linux
sudo pacman -S bspwm sxhkd polybar picom rofi dunst feh

# Parrot OS / Debian
sudo apt install bspwm sxhkd polybar picom rofi dunst feh
```

```bash
git clone https://github.com/eduardovergara-cyber/dotfiles-bspwm.git
cd dotfiles-bspwm
cp -r bspwm sxhkd polybar picom scripts ~/.config/
chmod +x ~/.config/bspwm/bspwmrc ~/.config/bspwm/scripts/bspwm_resize ~/.config/polybar/launch.sh ~/.config/polybar/scripts/* ~/.config/scripts/*.sh
```

**Fuentes:** la barra usa Iosevka, Hack y Hurmit Nerd Font, además de Montserrat, Source Code Pro, Helvetica Rounded y feather. No se incluyen en el repo por su tamaño y su licencia: instálalas desde [Nerd Fonts](https://www.nerdfonts.com/) y desde sus sitios oficiales.

> Ajusta en `bspwmrc` la ruta del fondo de pantalla, en `ethernet_status.sh` el nombre de tu interfaz de red y en `victim_to_hack.sh` la ruta de tu usuario.

## 💡 Qué aprendí

- Cómo arranca una sesión gráfica en Linux (display manager, `.xinitrc`, X11)
- Que bspwm no maneja el teclado: delega los atajos en sxhkd y se controla por completo con `bspc`
- Cómo polybar ejecuta scripts propios y muestra su salida en la barra
- Dónde vive la configuración de usuario (`~/.config`) y cómo versionarla con Git
- Compilar e instalar programas desde el código fuente cuando el paquete no está disponible
- Diferencias de empaquetado y filosofía entre Arch y Debian

## 🙏 Créditos

- **s4vitar:** tutorial de entorno profesional con bspwm, del que vienen la estructura de `bspwmrc` y `sxhkdrc`, el script `bspwm_resize` y los bloques de IP, VPN y objetivo de la barra
- **[adi1090x/polybar-themes](https://github.com/adi1090x/polybar-themes):** temas de polybar y rofi en los que se basan `current.ini`, `workspace.ini`, `colors.ini` y los menús
