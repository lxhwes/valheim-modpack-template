Config Directory for valheim-modpack-template

This directory is where you should place configuration files for the mods included in your modpack.

Structure:
Put mod .cfg files directly in this directory, flat:

    config/
    ├── mod1.cfg
    └── mod2.cfg

The mod manager copies the contents of this directory into the game profile's
BepInEx/config/ folder on install, overwriting any file of the same name.

Do NOT create a BepInEx/config/ folder inside this one. That extra layer gets copied
verbatim, so your files end up at BepInEx/config/BepInEx/config/mod1.cfg where no mod
will ever read them.

The easiest way to get correct files: install the mods with r2modman, tune them in-game,
then Settings -> "Browse profile folder" -> open BepInEx/config and copy out the .cfg
files you changed.

When adding configuration files to your modpack:
1. Test the configurations thoroughly to ensure they work as expected
2. Document any significant configuration changes in your main README.md
3. Ensure the configurations are compatible with each other
4. Consider creating a separate document explaining your configuration choices

Note: The example configuration files included in this template are for demonstration purposes only.
You should replace them with your own configurations for the mods you include in your modpack.
