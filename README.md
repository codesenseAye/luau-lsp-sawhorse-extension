# Luau Language Server

An implementation of a language server for the [Luau](https://github.com/Roblox/luau) programming language.

## Getting Started

Install the extension from the VSCode Marketplace or OpenVSX Registry:

- VSCode Marketplace: https://marketplace.visualstudio.com/items?itemName=JohnnyMorganz.luau-lsp
- OpenVSX Registry: https://open-vsx.org/extension/JohnnyMorganz/luau-lsp

Alternatively, check out [Getting Started for Language Server Clients](https://github.com/JohnnyMorganz/luau-lsp/blob/main/editors/README.md)
to setup your own client for a different editor

### For General Users

The language server should be immediately usable for general Luau code after installation.
String require support is provided for module paths, using `require("module")`.
There are two options for resolving requires, which can be configured using `luau-lsp.require.mode`:
`relativeToWorkspaceRoot` (the default - equivalent to the [command-line REPL](https://github.com/Roblox/luau#usage) `luau`)
or `relativeToFile`.

Type definitions can be provided by configuring `luau-lsp.types.definitionFiles`, with corresponding
documentation added using `luau-lsp.types.documentationFiles`.

If you use Luau in a different environment and are interested in using the language server, or
looking for any specific features, please get in touch!

### For Rojo Users (requires `v7.3.0+`)

By default, the latest Roblox type definitions and documentation are preloaded out of the box.
This can be disabled by configuring `luau-lsp.platform.type`.

The language server uses Rojo-style sourcemaps to resolve DataModel instance trees for intellisense.
This is done by running `rojo sourcemap --watch default.project.json --output sourcemap.json`.
The server listens to changes of a `sourcemap.json` file present at the workspace root. It is recommended to add this
file to your `.gitignore`.

The following settings are configurable for sourcemap generation:

- `luau-lsp.sourcemap.enabled`: Whether sourcemap support is enabled (default: on)
- `luau-lsp.sourcemap.autogenerate`: Whether sourcemaps are automatically generated by the client. If disabled, the server will listen to manual changes to a `sourcemap.json` file (default: on)
- `luau-lsp.sourcemap.rojoProjectFile`: What project file to use (default: `default.project.json`)
- `luau-lsp.sourcemap.includeNonScripts`: Whether to include non script instances in the sourcemap. May be disabled for expensive DataModels (default: on)

If you do not use Rojo, you can still use the Luau Language Server, you just need to manually generate a `sourcemap.json`
file for your particular project layout.

> Note: in the diagnostics type checker, the types for DataModel (DM) instances will resolve to `any`. This is a current limitation to reduce false positives.
> However, autocomplete and hover intellisense will correctly resolve the DM type.
> To enable this mode for diagnostics, set `luau-lsp.diagnostics.strictDatamodelTypes` (off by default).
> [Read more](https://github.com/JohnnyMorganz/luau-lsp/issues/83#issuecomment-1192865024).

**A companion Studio plugin is available to provide DataModel information for Instances which are not part of your Rojo build / filetree: [Plugin Marketplace](https://www.roblox.com/library/10913122509/Luau-Language-Server-Companion)**

## Standalone

The tool can run standalone, similar to [`luau-analyze`](https://github.com/JohnnyMorganz/luau-analyze-rojo), to provide type and lint warnings in CI, with full Rojo resolution and API types support.
The entry point for the analysis tool is `luau-lsp analyze`.

Install the binary and run `luau-lsp --help` for more information.

## Supported Features

- [x] Diagnostics (incl. type errors)
- [x] Autocompletion
- [x] Hover
- [x] Signature Help
- [x] Go To Definition
- [x] Go To Type Definition
- [x] Find References
- [x] Document Link
- [x] Document Symbol
- [x] Color Provider
- [x] Rename
- [x] Semantic Tokens
- [x] Inlay Hints
- [x] Documentation Comments ([Moonwave Style](https://github.com/evaera/moonwave) - supporting both `--- comment` and `--[=[ comment ]=]`, but must be next to statement)
- [x] Code Actions
- [x] Workspace Symbols
- [x] Folding Range
- [x] Call Hierarchy

The following are extra features defined in the LSP specification, but most likely do not apply to Luau or are not necessary.
They can be investigated at a later time:

- [ ] Go To Declaration (do not apply)
- [ ] Go To Implementation (do not apply)
- [ ] Code Lens (not necessary)
- [ ] Document Highlight (not necessary - editor highlighting is sufficient)
- [ ] Selection Range (not necessary - editor selection is sufficient)
- [ ] Inline Value (applies for debuggers only)
- [ ] Moniker
- [ ] Formatting (see [stylua](https://github.com/JohnnyMorganz/StyLua))
- [ ] Type Hierarchy (Luau currently does not provide any [public] ways to define type hierarchies)

## Build From Source

```sh
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --target Luau.LanguageServer.CLI --config Release
```
