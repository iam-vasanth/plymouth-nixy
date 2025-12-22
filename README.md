# Do not use as of right now. WIP⚠️

# Nixy Plymouth Theme

A minimalist Plymouth boot theme for NixOS featuring a pulsating Nix logo animation.

## Preview

*[WIP]*

## Installation

### Using Flakes (Recommended)

1. **Add this theme as an input to your flake:**

```
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    # Stable branch can also be used
    plymouth-nixy = {
      url = "github:iam-vasanth/plymouth-nixy";
      inputs.nixpkgs.follows = "nixpkgs-url";
    }
    # ... your other inputs
  };
}
```

2. **Pass the theme to your NixOS configuration:**

```
outputs = { self, nixpkgs, plymouth-nixy, ... }: {
  nixosConfigurations.**yourhostname** = nixpkgs.lib.nixosSystem {
    system = "x86_64-linux";
    modules = [ ./configuration.nix ];
    specialArgs = {
      inherit plymouth-nixy;
    };
  };
};
```

3. **Update your `configuration.nix`:**

```
{ config, pkgs, plymouth-nixy, ... }:

{
  boot.plymouth = {
    enable = true;
    theme = "nixy";
    themePackages = [ plymouth-nixy.packages.x86_64-linux.default ];
  };
  
  # Optional: Enable silent boot for a cleaner experience
  boot.consoleLogLevel = 0;
  boot.initrd.verbose = false;
  boot.kernelParams = [
    "quiet"
    "splash"
    "loglevel=3"
    "rd.systemd.show_status=false"
    "rd.udev.log_level=3"
    "udev.log_priority=3"
    "vt.global_cursor_default=0"
  ];
}
```

4. **Rebuild your system:**

```
sudo nixos-rebuild switch --flake .#yourhostname
```

## Customization

### Changing the Logo

Replace `nixy/logo.png` with your own logo. For best results, use a PNG with transparency.

Recommended resolution `300x300 pixels` or `250x250 pixels`

### Adjusting Animation Speed

Edit `nixy/nixy.script` and modify the `glow.speed` variable:

```
glow.speed = 0.02;  // Lower = slower, Higher = faster
```

### Changing Pulse Range

Modify the alpha bounds in `nixy/nixy.script`:

```
if (glow.alpha >= 1.0) {  // Maximum brightness
    glow.alpha = 1.0;
    glow.direction = -1;
}
if (glow.alpha <= 0.3) {  // Minimum brightness
    glow.alpha = 0.3;
    glow.direction = 1;
}
```

### Background Color

Change the background color in `nixy/nixy.script`:

```
Window.SetBackgroundTopColor(0.0, 0.0, 0.0);     // R, G, B (0.0 to 1.0)
Window.SetBackgroundBottomColor(0.0, 0.0, 0.0);
```

## Troubleshooting

### Theme doesn't appear

1. Ensure Plymouth is properly enabled in your configuration
2. Check that the theme is installed:
   ```
   plymouth-set-default-theme --list
   ```
3. Verify the theme is set:
   ```
   plymouth-set-default-theme
   ```

### Password prompt issues

If the password prompt doesn't appear or looks incorrect, rebuild your initrd:
```
sudo nixos-rebuild boot
```

## Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest improvements

## License

MIT License - Feel free to use and modify as you wish.

## Credits

Logo : [NixOS branding](https://nixos.org/branding/)

---

**Note**: If you encounter any issues, please check the [Issues](https://github.com/iam-vasanth/nixy/issues) page or create a new one. I'll do my best to help!