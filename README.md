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
1. In VS Code, open the Command Palette (Ctrl + Shift + P) and run `Extensions: Install from VSIX...` to install the VSIX file created on the last step.
1. C-P -> `Reload Window` if necessary.
1. Clean up, if desired:
    > `npm uninstall -g vsce`

## Features
Our main feature: a bundle of pseudo-markdown delimiters which can have their own formatting.  
Delimiters used: `@#%^&+=|`.  
1. @@here@@
1. ##look##
1. %%what%%
1. ^^can^^
1. &&be&&
1. ++done++
1. ==with==
1. ||them!||

(TODO: Insert a picture of the colored display above, as seen in the editor.)

Secondary: vscode provides a bundle of behaviors for parens (automatic creation of a closing pair, surround of selection, tracking of pairs). We add some more paren-pairs to track inside MD files, and also abuse these features for convenience of delimiters above. See `markdown-configuration.json` for details.

## Troubleshooting, workarounds, customization
1. Coloring does not work if your user (or workspace) settings override `"editor.tokenColorCustomizations"`; user-defined value for it prevails without merging.
    1. Workaround: take the color section defined in `package.json` and splice it into your user config.
    1. This also works if you want to tinker with hues.

## How does this thing work?
(This omits a bunch of details; treat this as useful-model rather than literal-truth.)  
Coloring between delimiters:  
1. VSCode continuously-ish scans Markdown files with a bunch of regexes, to figure out its structure — headers hierarchy, ordinary-MD-formatting sections, LaTeX-formatting sections, etc.
1. We can inject additional regexes into the bunch! `syntaxes/markdown.tmLanguage.json` does this.
1. For any capture of the regex, so-called `scopes` are assigned to the match and submatches.
    1. You can use `Developer: Inspect Editor Tokens and Scopes` vscode command to look at those. (Without this extension, or in non-markdown files, too!)
    1. Names for our novel scopes are assigned by our `markdown.tmLanguage.json`, and can be semi-arbitrary. We follow vague conventions observed in already-present scope names.
1. Elsewhere, VSCode permits one to configure `editor.tokenColorCustomizations`, setting text and background colors (and who knows what else) per-scope!
    1. We attempt to inject a config section with scope->color assignments in `package.json`. This doesn't work if user overrides these in user-config, unfortunately; see workaround above if you want both our colors and your own customizations.

Paren-matching:  
1. VSCode has language configurations; among other things, these allow one to set:
    1. which symbols are paren-pairs — i.e. their pairing should be tracked, and unpaired `(` or `]` highlighted
    1. which open-paren symbol should be autocompleted with a close-paren symbol — type `(`, and `)` is inserted immediately after; type `{`, get `}`
        1. (This actually works for entire strings, not just single characters; type `##`, and `##` gets inserted after.)
    1. which symbols should, when typed on highlited segment, instead of deleting it, wrap it in an open-closed pair — higlight `asdf`, type `(`, get `(asdf)` instead of `(`.
1. These configurations can be contributed by extensions! Our config is in `markdown-configuration.json`, and override of the default markdown config is in `package.json` ~> `contributes.languages.0`.

## Release Notes

### [0.1.1] — 2024-01-25
More brackets: ⌜⌝, ⌞⌟.

### [0.1.0] — 2023-07-14
Added: more brackets for code to track: ⟦⟧, ⟨⟩, ⟪⟫, ⦃⦄, ⦅⦆, ⦇⦈, 【】, 〖〗.

### [0.0.1] — ???
Initial.
Delimiters in use: `@#%^&+=|`
