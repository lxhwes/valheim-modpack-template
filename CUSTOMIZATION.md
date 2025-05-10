# Customization Guide for valheim-modpack-template

## Initial Setup

### 1. Fork or Clone the Template

**Option A: Fork on GitHub (Recommended)**
1. Navigate to the original repository on GitHub
2. Click the "Fork" button in the top-right corner
3. Select your account as the destination

**Option B: Clone and Push to a New Repository**
1. Clone the repository locally:
   ```bash
   git clone https://github.com/original-owner/valheim-modpack-template.git my-modpack
   cd my-modpack
   ```
2. Remove the original remote and add your own:
   ```bash
   git remote remove origin
   git remote add origin https://github.com/your-username/your-repo-name.git
   ```
3. Push to your new repository:
   ```bash
   git push -u origin main
   ```

### 2. Update the manifest.json

The `manifest.json` file is the core of the modpack's Thunderstore package. Edit it to customize your modpack's metadata:

```json
{
  "name": "YourModpackName",              // Your unique modpack name
  "version_number": "1.0.0",              // Semantic versioning (MAJOR.MINOR.PATCH)
  "website_url": "https://github.com/YourUsername/YourRepo", // Your repository URL
  "description": "Your modpack description", // Compelling description (supports markdown)
  "dependencies": [
    "denikson-BepInExPack_Valheim-5.4.2105",
    "ValheimModding-Jotunn-2.12.1"
    // Add more mod dependencies here
  ]
}
```

**Important Notes:**
- The `name` must be unique on Thunderstore and contain only alphanumeric characters, underscores, and dashes
- The `version_number` must follow semantic versioning (MAJOR.MINOR.PATCH)
- The `description` supports markdown formatting
- Each dependency must exist on Thunderstore and be formatted exactly as shown on the mod's page

### 3. Create a Custom Icon

The `icon.png` file is required by Thunderstore and appears on your modpack's page. Replace the placeholder with your own custom icon:

**Requirements:**
- Must be a PNG file format
- Must have square dimensions (at least 256x256 pixels, 512x512 recommended)
- Should be visually representative of your modpack

**Creating an Icon:**
1. Use image editing software like GIMP, Photoshop, or online tools like Canva
2. Create a square canvas (512x512 pixels recommended)
3. Design your icon with clear, recognizable elements
4. Export as PNG and replace the existing `icon.png` file

**Example Command (if you have ImageMagick installed):**
```bash
convert -size 512x512 -background "#4B6F44" -fill white -gravity center -font Helvetica -pointsize 48 label:"My Awesome\nValheim Modpack" icon.png
```

### 4. Update the README.md

The README.md file is what users will see when they visit your repository or modpack page. Edit it as needed to provide comprehensive information:

**Suggested Sections to Include:**
- **Introduction**: Brief overview of modpack
- **Features**: Bullet points highlighting key features and included mods
- **Installation**: Instructions for installing modpack
- **Included Mods**: Complete list with links and version numbers
- **Configuration**: How to configure the modpack (if applicable)
- **Compatibility**: Known compatible/incompatible mods or game versions
- **Credits**: Proper attribution to mod authors and contributors
- **License**: Information about your modpack's license

**README Template:**
```markdown
# Your Modpack Name

Brief description of the modpack.

## Features

- Feature 1: Description
- Feature 2: Description
- Feature 3: Description

## Installation

1. Install [r2modman](https://thunderstore.io/package/ebkr/r2modman/) or [Thunderstore Mod Manager](https://www.overwolf.com/app/Thunderstore-Thunderstore_Mod_Manager)
2. Create a new profile for Valheim
3. Search for "[Your Modpack Name]" and install it
4. Launch the game through the mod manager

## Included Mods

| Mod Name | Author | Version | Description |
|----------|--------|---------|-------------|
| BepInExPack | denikson | 5.4.2105 | Core modding framework |
| Jotunn | ValheimModding | 2.12.1 | Modding library |
| [Additional Mod] | [Author] | [Version] | [Description] |

## Configuration

Details about any configuration options or settings users should know about.

## Compatibility

- Compatible with Valheim version X.X.X
- Known conflicts with [Mod Name]

## Credits

- [Mod Author 1] for [Mod Name 1]
- [Mod Author 2] for [Mod Name 2]
- All the amazing Valheim modders who made this possible

## License

This modpack is released under [Your License] license.
```

## Adding and Managing Mods

### Finding Mods for Your Modpack

1. Browse [Thunderstore's Valheim section](https://valheim.thunderstore.io/) to find mods
2. Test mods individually or in combinations to ensure compatibility
3. For each mod you want to include, note its dependency string:
   - Format: `AuthorName-ModName-Version`
   - Example: `denikson-BepInExPack_Valheim-5.4.2105`
   - This can be found on the mod's Thunderstore page

### Adding Mods to Your Modpack

1. Add each mod as a dependency in your `manifest.json`:

```json
"dependencies": [
  "denikson-BepInExPack_Valheim-5.4.2105",
  "ValheimModding-Jotunn-2.12.1",
  "AuthorName-ModName-Version",
  "AnotherAuthor-AnotherMod-Version"
]
```

2. Test your modpack thoroughly to ensure all mods work together

### Dependency Order and Management

The order of dependencies in your manifest.json is critical for proper mod loading:

1. **Core frameworks** (BepInEx, Jotunn) must be listed first
2. **Libraries and APIs** that other mods depend on should be next
3. **Mods that other mods depend on** should be listed before their dependents
4. **Independent mods** can be listed last

**Tips for Dependency Management:**
- Create a spreadsheet or document to track your mods and their dependencies
- Use a mod manager like r2modman to test combinations before adding to your modpack
- When updating your modpack, check for new versions of included mods
- Document any special load order requirements in your README.md

## Configuration Files and Customization

### Adding Mod Configurations

Proper configuration is key to creating a cohesive modpack experience:

1. Install the mods locally using a mod manager to generate default configs
2. Adjust settings to create your desired gameplay experience
3. Copy the configuration files to your modpack's `config/` directory
4. Follow the structure expected by the mods:

```
config/
├── BepInEx/
│   └── config/
│       ├── mod1.cfg
│       └── mod2.cfg
└── othermod/
    └── settings.json
```

**Important Notes:**
- Test all configurations thoroughly before including them
- Document any significant configuration changes in your README.md
- Consider creating a separate document explaining your configuration choices

### Custom Plugins and Scripts

To use custom plugins or scripts:

1. Place custom DLL files in the `plugins/` directory
2. Ensure they're compatible with your included mods
3. Document their functionality in your README.md
4. If they're created by others, ensure you have permission to distribute them
5. Provide proper attribution in your documentation

### Modpack Development Workflow

To develop a polished, functional modpack:

1. Ensure all mods work together without conflicts
2. Remove redundant or overlapping features
3. Document any known issues or quirks

## Functionality Testing

### Testing Locally


1. **Clean Test Environment:**
   - Create a fresh Valheim installation or profile
   - Install BepInEx manually or through a mod manager

2. **Installation Testing:**
   - Copy your modpack files to the appropriate directories
   - Verify all files are placed correctly

3. **Functionality Testing:**
   - Launch Valheim and check the console for errors
   - Verify all mods load correctly
   - Test each mod's functionality
   - Play through different game scenarios (new world, existing world)

4. **Compatibility Testing:**
   - Test with different Valheim versions
   - Check for conflicts between mods
   - Test in both singleplayer and multiplayer (if applicable)

5. **Performance Testing:**
   - Monitor FPS and performance
   - Test on different hardware configurations if possible
   - Identify and resolve any performance issues

### Using r2modman for Testing

[r2modman](https://thunderstore.io/package/ebkr/r2modman/) is an excellent tool for testing:

1. Create a new profile for testing
2. Install all your modpack's dependencies manually
3. Export the profile to see how the files are structured
4. Use this as a reference for your modpack structure

## Publishing and Maintenance

### Customizing the GitHub Actions Workflow

The `.github/workflows/publish.yml` file controls the automated publishing process:

```yaml
name: Publish to Thunderstore

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
          
      - name: Install tcli
        run: pip install tcli
        
      - name: Publish to Thunderstore
        env:
          THUNDERSTORE_API_TOKEN: ${{ secrets.THUNDERSTORE_TOKEN }}
        run: |
          # Extract version from tag (remove 'v' prefix if present)
          VERSION=${GITHUB_REF_NAME#v}
          
          # Publish using tcli
          tcli publish \
            --token $THUNDERSTORE_API_TOKEN \
            --package-name valheim-modpack-template \
            --package-version $VERSION \
            --namespace ${{ github.repository_owner }} \
            --community valheim \
            --categories Modpacks \
            --file .
```

**Customization Options:**
- Change `--package-name valheim-modpack-template` to your modpack name
- Modify `--categories Modpacks` to include additional categories
- Add additional build steps if required

### Publishing Process

1. **Prepare Your Release:**
   - Update version number in `manifest.json`
   - Update documentation with changes
   - Commit all changes to your repository
   ```bash
   git add .
   git commit -m "Prepare release v1.0.0"
   git push origin main
   ```

2. **Create a GitHub Release:**
   - Go to your repository on GitHub
   - Click "Releases" in the right sidebar
   - Click "Create a new release"
   - Tag version: `v1.0.0` (must match your manifest version)
   - Release title: "Version 1.0.0"
   - Description: Release notes and changes
   - Click "Publish release"

3. **Monitor the Publishing Process:**
   - Go to the "Actions" tab in your repository
   - Watch the workflow execution
   - Check for any errors in the logs
   - Verify your modpack appears on Thunderstore

### Maintaining

Regular maintenance may be required to keep the modpack up to date and functional:

1. **Update Included Mods:**
   - Check for updates to included mods
   - Test new versions for compatibility
   - Update dependency strings in manifest.json

2. **Version Management:**
   - Follow semantic versioning (MAJOR.MINOR.PATCH)
   - MAJOR: Breaking changes
   - MINOR: New features, backward compatible
   - PATCH: Bug fixes, backward compatible

3. **Documentation Updates:**
   - Keep your README.md current
   - Document changes in each release
   - Maintain a changelog

4. **Community Engagement:**
   - Respond to user feedback
   - Address bug reports
   - Consider feature requests

## Advanced Customization

### Custom Mod Patches and Integrations

1. **BepInEx Patchers:**
   - Create custom patchers to modify mod behavior
   - Place in the appropriate BepInEx directories
   - Document their purpose and functionality

2. **Configuration Overrides:**
   - Create custom configuration files that override defaults
   - Test thoroughly to ensure they work as expected
   - Document all overrides in your documentation

3. **Custom Scripts:**
   - Create scripts to enhance mod functionality
   - Place in the appropriate directories
   - Document their usage and requirements

### Managing Complex Dependencies

Modpacks with many mods can create complex dependency chains. To better manage these:

1. **Create a Dependency Map:**
   ```
   ModA → ModB → ModC
     ↓
   ModD → ModE
   ```

2. **Document in a `DEPENDENCIES.md` file:**
   ```markdown
   # Dependency Structure
   
   ## Core Dependencies
   - BepInExPack (required by all mods)
   - Jotunn (required by ModA, ModB, ModE)
   
   ## Mod Dependencies
   - ModA depends on: BepInExPack, Jotunn
   - ModB depends on: ModA, BepInExPack, Jotunn
   - ModC depends on: ModB
   - ModD depends on: BepInExPack
   - ModE depends on: ModD, Jotunn
   
   ## Known Conflicts
   - ModA conflicts with: ConflictingMod1
   - ModC conflicts with: ConflictingMod2, ConflictingMod3
   ```

3. **Version Compatibility Matrix:**
   ```markdown
   | Mod | Min Version | Max Version | Notes |
   |-----|-------------|-------------|-------|
   | ModA | 1.0.0 | 1.5.0 | Breaks with 2.0.0+ |
   | ModB | 2.3.0 | Current | Requires ModA 1.2.0+ |
   ```

### Version Update Strategy

Thunderstore does not support in-place updates for modpacks, a new version must be published each time a change is made:

1. **Create a Testing Branch:**
   ```bash
   git checkout -b update-v1.1.0
   ```

2. **Update Components:**
   - Update version number in `manifest.json`
   - Update mod dependencies to their latest compatible versions
   - Update configuration files as needed
   - Update documentation to reflect changes

3. **Testing Protocol:**
   - Test all components individually
   - Test the complete modpack in a clean environment
   - Test upgrade path from previous version
   - Test in multiplayer if applicable

4. **Release Process:**
   - Merge testing branch to main
   - Create detailed release notes
   - Create GitHub release
   - Monitor the publishing process

## Publishing Final Checklist

- [ ] **Manifest.json is correctly formatted**
  - [ ] Name is unique and follows naming conventions
  - [ ] Version number follows semantic versioning
  - [ ] Website URL points to your repository
  - [ ] Description is informative and well-formatted
  - [ ] All dependencies are correctly listed with proper versions

- [ ] **Icon.png meets requirements**
  - [ ] PNG format
  - [ ] Square dimensions (at least 256x256)
  - [ ] Visually representative of your modpack

- [ ] **Documentation is complete**
  - [ ] README.md contains all necessary information
  - [ ] Installation instructions are clear
  - [ ] All included mods are listed with proper attribution
  - [ ] Configuration options are documented

- [ ] **Configuration files are properly set up**
  - [ ] All necessary config files are included
  - [ ] Configs are correctly formatted
  - [ ] Configs are placed in the correct directories

- [ ] **GitHub Actions workflow is configured**
  - [ ] publish.yml is updated with your modpack name
  - [ ] THUNDERSTORE_TOKEN secret is set in repository settings

- [ ] **Testing is complete**
  - [ ] Modpack works in a clean installation
  - [ ] All mods function as expected
  - [ ] No conflicts between included mods
  - [ ] Performance is acceptable

- [ ] **License requirements are met**
  - [ ] You have permission to distribute all included content
  - [ ] Proper attribution is given to all mod authors
  - [ ] License information is included
