# What is this?

This is a [nix overlay](https://nixos.org/nixpkgs/manual/#chap-overlays) that adds an installable package to flash [U2F](https://en.wikipedia.org/wiki/Universal_2nd_Factor) software onto a [tomu](https://www.crowdsupply.com/sutajio-kosagi/tomu).

# How do I?

```
git clone https://github.com/teh/tomu-u2f-overlay
cp -r tomu-u2f-overlay/* ~/.config/nixpkgs/overlays
nix-env -iA nixpkgs.tomu-flash-utf

# Insert your tomu key, then:
sudo $(which tomu-flash-u2f)
```

To reset to the original state follow [these instructions](https://github.com/im-tomu/tomu-bootloader#using-toboot).

# Use as normal user on NixOS

Add the following to `/etc/nixos/configuration.nix`, then `nixos-rebuild switch`.

```
services.udev.extraRules = ''
    ACTION=="add|change", KERNEL=="hidraw*", SUBSYSTEM=="hidraw", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="cdab", TAG+="uaccess", MODE="0766",GROUP="users"
  '';
```
