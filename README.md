# valheim-modpack-template

This is a **modpack template** designed specifically for creating and publishing Valheim modpacks to [Thunderstore.io](https://thunderstore.io/). It provides all the necessary structure and automation to get you started publishing modpacks.

## Purpose

This template serves as a foundation that can be forked and customized to create a new Valheim modpack with minimal setup. It's intentionally kept minimal and generic to allow for easy customization.

## Features

- **Ready-to-use template structure** for Valheim modpacks
- **Automated publishing** to Thunderstore via GitHub Actions
- **Minimal dependencies** to start with (BepInEx and Jotunn)
- **Example configuration files** to demonstrate proper structure
- **Comprehensive documentation** for customization and publishing

## Getting Started

### 1. Clone the Template

```bash
# Clone via HTTPS
git clone https://github.com/lxhwes/valheim-modpack-template.git

# OR clone via SSH
git clone git@github.com:lxhwes/valheim-modpack-template.git

# Navigate to the project directory
cd valheim-modpack-template
```

### 2. Customize the Modpack

1. **Update manifest.json** with your modpack details and dependencies
2. **Replace icon.png** with your own modpack icon (must be at least 256x256 pixels)
3. **Modify configuration files** in the config directory
4. **Add any custom plugins** to the plugins directory
5. **Update documentation** to reflect your modpack's features

See the [Customization Guide](CUSTOMIZATION.md) for detailed instructions.

### 3. Publish to Thunderstore

1. **Commit and push** your changes to GitHub
   ```bash
   git add .
   git commit -m "Customize modpack for initial release"
   git push origin main
   ```

2. **Create a release** on GitHub
   - Go to your repository on GitHub
   - Click on "Releases" in the right sidebar
   - Click "Create a new release"
   - Set the tag version (e.g., `v1.0.0`) matching your manifest.json version
   - Add a title and description
   - Click "Publish release"

3. **Monitor the GitHub Action** to ensure successful publishing
   - Go to the "Actions" tab in your repository
   - You should see the workflow running after creating the release
   - Once completed, your modpack will be available on Thunderstore

## Template Structure

```
valheim-modpack-template/
├── .github/workflows/   # GitHub Actions workflow for Thunderstore publishing
│   └── publish.yml      # Workflow configuration for automated publishing
├── config/              # Configuration files for included mods
│   ├── BepInEx/         # BepInEx configuration directory
│   │   └── config/      # Mod-specific configurations
│   └── custom_mod_settings.json  # Example JSON configuration
├── plugins/             # Custom plugins (if any)
├── icon.png             # Modpack icon (required by Thunderstore)
├── manifest.json        # Thunderstore manifest file
├── README.md            # This documentation file
└── CUSTOMIZATION.md     # Detailed customization instructions
```

This structure follows Thunderstore's requirements for modpacks and provides a clean organization for your files.

## Prerequisites

Before using this template, you'll need:

1. A [GitHub](https://github.com/) account
2. A [Thunderstore](https://thunderstore.io/) account
3. Basic knowledge of Git and GitHub
4. Git installed on your local machine
5. A Thunderstore API token (for publishing)

## Setting Up Thunderstore Publishing

### Generate a Thunderstore API Token

1. Log in to your [Thunderstore](https://thunderstore.io/) account
2. Go to your account settings
3. Navigate to the "API Tokens" section
4. Create a new token with the "Upload Packages" permission
5. Copy the generated token

### Add the Token to GitHub Secrets

1. Go to your GitHub repository settings
2. Navigate to "Secrets and variables" > "Actions"
3. Click "New repository secret"
4. Name: `THUNDERSTORE_TOKEN`
5. Value: Your Thunderstore API token
6. Click "Add secret"

This token allows the GitHub Action to publish your modpack to Thunderstore on your behalf.

## Troubleshooting

### Common Issues

- **Action fails with authentication error**: 
  - Verify your Thunderstore API token is correctly set in GitHub secrets
  - Ensure the token has the "Upload Packages" permission

- **Package validation fails**: 
  - Check that your manifest.json is correctly formatted
  - Ensure all required files are present (especially icon.png)
  - Verify that all dependencies in manifest.json exist on Thunderstore

- **Version conflict**: 
  - Make sure you're using a new version number that hasn't been published before
  - Version in manifest.json must match the GitHub release tag (without the 'v' prefix)

- **GitHub Action fails**: 
  - Check the Action logs for specific error messages
  - Verify that your repository structure matches the template

### Getting Help

If you encounter issues not covered here:
- Check the [Thunderstore Documentation](https://thunderstore.io/docs/)
- Visit the [Valheim Modding Discord](https://discord.gg/RBq2mzeu4z)
- Open an issue on the template repository

## License

This template is provided under the MIT License. You are free to use, modify, and distribute it as needed.

## Credits

- [Valheim](https://www.valheimgame.com/) by Iron Gate Studio
- [Thunderstore](https://thunderstore.io/) for the mod distribution platform
- [BepInEx](https://github.com/BepInEx/BepInEx) for the modding framework
- [Jotunn](https://github.com/Valheim-Modding/Jotunn) for the Valheim mod library

---

*This template is designed to be a starting point. Feel free to modify any part of it to suit your specific needs. Have fun!*
