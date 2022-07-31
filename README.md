# markdown-custom README

## Installation
This extension is not listed on the vscode extension marketplace, so you'll want to build it yourself. To do that:
1. Clone this repo
1. Acquire `vsce` to package the extension:
    1. Have an npm
    2. `npm install -g vsce`
1. From the root of this repo, package:
    1. `vsce package`
    1. `vsce` may object to unfilled fields. These matter not.
1. In VS Code, open the Command Palette (Ctrl + Shift + P) and run the "Extensions: Install from VSIX..." to install the VSIX file created on the last step.
1. C-P -> `Reload Window` if necessary.
1. Clean up, if desired:
    > `npm uninstall -g vsce`

## Features

Our main feature: a bundle of pseudo-markdown delimeters which can have their own formatting.
Delimteters used: `@#%^&+=|`
1. @@here@@
1. ##look##
1. %%what%%
1. ^^can^^
1. &&be&&
1. ++done++
1. ==with==
1. ||them!||

(Insert a picture of the colored display above, as seen in the editor.)

Secondary: vscode provides a bundle of behaviors for parens (automatic creation of a closing pair, surround of selection) that we abuse for convenience of delimeters above, and a bit beyond. See `markdown-configuration.json` for details.

## Troubleshooting, workarounds, customization
1. Coloring does not work if your user (or workspace) settings override `"editor.tokenColorCustomizations"`; user-defined value for it prevails without merging.
    1. Workaround: take the color section defined in `package.json` and splice it into your user config.
    1. This also works if you wish for different hues.

## Release Notes

(If there's no release, are there any release notes?)
