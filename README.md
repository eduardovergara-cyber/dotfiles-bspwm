# 🖥️ dotfiles-bspwm

Mi entorno de trabajo en Linux, configurado desde cero con el gestor de ventanas en mosaico **bspwm**. Lo uso en **Arch Linux** y **Parrot OS**, en máquinas virtuales de VMware.

---

## 🧱 Componentes

| Componente | Programa | Configuración |
|------------|----------|---------------|
| Gestor de ventanas | bspwm | [`bspwm/bspwmrc`](bspwm/bspwmrc) |
| Atajos de teclado | sxhkd | [`sxhkd/sxhkdrc`](sxhkd/sxhkdrc) |
| Terminal | kitty | instalada en `~/.local/kitty.app` |
| Lanzador | rofi | `rofi -show run` |
| Barra | polybar | próximamente |
| Compositor | picom | próximamente |
| Notificaciones | dunst | configuración por defecto |
| Fondo de pantalla | feh | definido en `bspwmrc` |

## ⚙️ Configuración destacada

- **`alt` como tecla modificadora** en lugar de `super`, porque dentro de VMware el sistema anfitrión (Windows) captura la tecla Windows
- **10 escritorios virtuales** y márgenes de 12 px entre ventanas, sin bordes
- **División automática por el lado más largo** (`automatic_scheme longest_side`): cada ventana nueva aprovecha mejor el espacio
- **El foco sigue al mouse** (`focus_follows_pointer`)
- **Reglas por aplicación**: por ejemplo, GIMP se abre flotante en el escritorio 8
- **Integración con VMware** (`vmware-user`) para el portapapeles y el ajuste de resolución
- **sxhkd compilado desde el código fuente** e iniciado solo si no está corriendo (`pgrep -x sxhkd || ...`)
- **Script propio de redimensión** ([`bspwm/scripts/bspwm_resize`](bspwm/scripts/bspwm_resize)): ajusta la ventana enfocada en pasos de 20 px si es flotante o de 100 px si está en mosaico, y si no puede crecer hacia un lado lo intenta hacia el opuesto

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

## 🐧 Diferencias entre distribuciones

| | Arch Linux | Parrot OS |
|---|---|---|
| Gestor de paquetes | `pacman` / AUR | `apt` |
| Base | Instalación mínima desde cero | Debian, orientada a seguridad |
| Uso | Sistema personal | Laboratorio de seguridad |

## 📦 Instalación

```bash
# Arch Linux
sudo pacman -S bspwm sxhkd rofi dunst feh

# Parrot OS / Debian
sudo apt install bspwm sxhkd rofi dunst feh
```

```bash
git clone https://github.com/eduardovergara-cyber/dotfiles-bspwm.git
cd dotfiles-bspwm
cp -r bspwm sxhkd ~/.config/
chmod +x ~/.config/bspwm/bspwmrc ~/.config/bspwm/scripts/bspwm_resize
```

> Cambia la ruta del fondo de pantalla en `bspwmrc` por una imagen tuya.

## 💡 Qué aprendí

- Cómo arranca una sesión gráfica en Linux (display manager, `.xinitrc`, X11)
- Que bspwm no maneja el teclado: delega los atajos en sxhkd y se controla por completo con `bspc`
- Dónde vive la configuración de usuario (`~/.config`) y cómo versionarla con Git
- Compilar e instalar programas desde el código fuente cuando el paquete no está disponible
- Diferencias de empaquetado y filosofía entre Arch y Debian
