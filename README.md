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

## Requirements

- XFCE 4.16 or newer (`xfwm4` and `xfce4-panel`)
- Alacritty
- `unzip`
- Optional: `fastfetch` (only `grovy` uses it)
- The Nerd Font named in a rice's `alacritty.toml`: `darverest` uses
  ShureTechMono Nerd Font, `grovy` and `monochrome` use FiraMono Nerd Font Mono

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

| File in a rice          | Destination                            |
| ----------------------- | -------------------------------------- |
| `<rice>/*.zip`          | extract into `~/.themes/`              |
| `<rice>/gtk.css`        | `~/.config/gtk-3.0/gtk.css`            |
| `<rice>/alacritty.toml` | `~/.config/alacritty/alacritty.toml`   |
| `<rice>/fastfetch/`     | `~/.config/fastfetch/`                 |
| `<rice>/wallpapers/`    | anywhere; pick it in Desktop settings  |

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

## darverest (active)

My current look: cream text on charcoal, with muted earthy accents on the panel
and terminal. Ships two wallpapers.

**Files**

- `gtk.css` — panel overrides
- `alacritty.toml` — terminal colors and font
- `darverest-dark.zip` — GTK theme
- `wallpapers/mv1.png`, `wallpapers/mv2.png`

**Apply**

```bash
mkdir -p ~/.themes ~/.config/gtk-3.0 ~/.config/alacritty

unzip -o darverest/darverest-dark.zip -d ~/.themes
xfconf-query -c xsettings -p /Net/ThemeName -s darverest-dark
xfconf-query -c xfwm4 -p /general/theme -s darverest-dark

cp darverest/gtk.css ~/.config/gtk-3.0/gtk.css
cp darverest/alacritty.toml ~/.config/alacritty/alacritty.toml

xfce4-panel -r
xfwm4 --replace &
```

Set either wallpaper through Desktop settings, or point every workspace at one:

```bash
for prop in $(xfconf-query -c xfce4-desktop -l | grep '/last-image$'); do
  xfconf-query -c xfce4-desktop -p "$prop" -s "$PWD/darverest/wallpapers/mv1.png"
done
```

If that loop prints nothing, set one wallpaper once in Desktop settings and run
it again.

## grovy

A muted sage-green palette on black, with a fastfetch cat logo.

**Files**

- `gtk.css` — panel overrides
- `alacritty.toml` — terminal colors and font
- `dusky-grove-dark.zip` — GTK theme
- `fastfetch/config.jsonc`, `fastfetch/logo.txt` — fastfetch config and logo
- `wallpapers/Window_Cat.png`

**Apply**

```bash
mkdir -p ~/.themes ~/.config/gtk-3.0 ~/.config/alacritty ~/.config/fastfetch

unzip -o grovy/dusky-grove-dark.zip -d ~/.themes
xfconf-query -c xsettings -p /Net/ThemeName -s dusky-grove-dark
xfconf-query -c xfwm4 -p /general/theme -s dusky-grove-dark

cp grovy/gtk.css ~/.config/gtk-3.0/gtk.css
cp grovy/alacritty.toml ~/.config/alacritty/alacritty.toml
cp grovy/fastfetch/config.jsonc grovy/fastfetch/logo.txt ~/.config/fastfetch/

xfce4-panel -r
xfwm4 --replace &
```

The fastfetch config points its logo at `~/.config/fastfetch/logo.txt`, so run
`fastfetch` after copying the files into place — not from inside the repo.

The theme installs as `dusky-grove-dark`, which is what the commands above
select. Its bundled `index.theme` still carries the old internal name `duckai`;
if a tool labels the theme that way, it is the same theme. The zip is left
untouched.

Set the wallpaper through Desktop settings, or:

```bash
for prop in $(xfconf-query -c xfce4-desktop -l | grep '/last-image$'); do
  xfconf-query -c xfce4-desktop -p "$prop" -s "$PWD/grovy/wallpapers/Window_Cat.png"
done
```

## monochrome

The minimal one: white on black, no accent colors, no extra pieces.

**Files**

- `gtk.css` — panel overrides
- `alacritty.toml` — terminal colors and font
- `darkish-dark.zip` — GTK theme

**Apply**

```bash
mkdir -p ~/.themes ~/.config/gtk-3.0 ~/.config/alacritty

unzip -o monochrome/darkish-dark.zip -d ~/.themes
xfconf-query -c xsettings -p /Net/ThemeName -s darkish-dark
xfconf-query -c xfwm4 -p /general/theme -s darkish-dark

cp monochrome/gtk.css ~/.config/gtk-3.0/gtk.css
cp monochrome/alacritty.toml ~/.config/alacritty/alacritty.toml

xfce4-panel -r
xfwm4 --replace &
```
