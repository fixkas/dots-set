# dots-set

Three XFCE rices, one folder each. Every rice is self-contained: panel CSS, an
Alacritty config, one zipped GTK theme, and — where the look needs them —
fastfetch files and wallpapers.

`darverest` is my current setup; the other two are alternatives.

There is no installer. Everything below is copy-paste.

## Get the files

```bash
git clone https://github.com/fixkas/dots-set.git
cd dots-set
```

The commands in each rice section assume you are at the repo root.

## Repo layout

```
dots-set/
├── README.md
├── .gitignore
├── darverest/
│   ├── gtk.css
│   ├── alacritty.toml
│   ├── darverest-dark.zip
│   └── wallpapers/          # mv1.png, mv2.png
├── grovy/
│   ├── gtk.css
│   ├── alacritty.toml
│   ├── dusky-grove-dark.zip
│   ├── fastfetch/           # config.jsonc, logo.txt
│   └── wallpapers/          # Window_Cat.png
└── monochrome/
    ├── gtk.css
    ├── alacritty.toml
    └── darkish-dark.zip
```

## What goes where

| File in a rice          | Destination                           |
| ----------------------- | ------------------------------------- |
| `<rice>/*.zip`          | extract into `~/.themes/`             |
| `<rice>/gtk.css`        | `~/.config/gtk-3.0/gtk.css`           |
| `<rice>/alacritty.toml` | `~/.config/alacritty/alacritty.toml`  |
| `<rice>/fastfetch/`     | `~/.config/fastfetch/`                |
| `<rice>/wallpapers/`    | anywhere; pick it in Desktop settings |

`gtk.css` is panel-focused overrides. The rules target XFCE panel widgets —
workspace switcher, clock, notification, sound, battery, action button, status
tray — and layer on top of the GTK theme. Note that each file opens with a
global reset (`border: none; box-shadow: none; …`), so GTK3 apps that read it
also lose borders and shadows. It is not a full application theme; the zip
supplies the application and window-manager styling.

After copying `gtk.css`, restart the panel so the changes show up:

```bash
xfce4-panel -r
```

Theme zips also ship `xfwm4/` window borders, so each rice's commands select the
matching window-manager theme and reload it with `xfwm4 --replace &`.

Each rice writes the same config paths, so applying another rice replaces the
previous one. `grovy` also installs a fastfetch config; the other rices leave
any existing one in place. Themes coexist in `~/.themes/`.
