# Aurora Hypr

Custom bootc image that layers a Hyprland-focused desktop experience on top of the [Aurora base image](https://getaurora.dev/) (`ghcr.io/ublue-os/aurora:stable`). This repository builds, signs, and optionally publishes the resulting container image along with bootable media artifacts.

## Base Image
- `ghcr.io/ublue-os/aurora:stable`
- Inherits Aurora defaults such as Flatpak curation, automatic updates with rpm-ostree, and uBlue tooling.

## What This Image Adds
- Hyprland ecosystem from the `solopasha/hyprland` COPR: `hyprland`, `hyprpaper`, `hyprpicker`, `hypridle`, `hyprlock`, `hyprsunset`, `hyprpolkitagent`, `hyprsysteminfo`, `hyprpanel`, `hyprland-qt-support`, `hyprland-qtutils`.
- Extra desktop utilities: `wofi` application launcher, `kitty` terminal, `brightnessctl` for backlight control, and the `tmux` terminal multiplexer.

All package customizations live in `build_files/build.sh`. Adjust that script if you want to install more software or enable additional services.

## Building Locally
1. Ensure Podman and bootc prerequisites are available on your system.
2. Build the container image:
   ```bash
   just build aurora-hypr latest
   ```
   Set `IMAGE_NAME`/`DEFAULT_TAG` environment variables if you want values other than the defaults defined in `Justfile`.
3. Push the image to your registry of choice (for example `ghcr.io/<org>/aurora-hypr`).

## Creating Bootable Media
Use Bootc Image Builder via the provided Just recipes:
```bash
just build-iso aurora-hypr latest
just build-qcow2 aurora-hypr latest
just build-raw aurora-hypr latest
```
Generated artifacts are placed in the `output/` directory. Adjust the `disk_config/*.toml` files if you need different partitioning or a custom source image reference.

## Repository Layout
- `Containerfile`: defines the bootc build pipeline and calls the customization script.
- `build_files/build.sh`: installs packages and enables services that differentiate this image from upstream Aurora.
- `Justfile`: local helper commands to build the container image and produce bootable artifacts.