# Goldmark Alert Callouts Extension

[![Go Reference](https://pkg.go.dev/badge/github.com/ZMT-Creative/gm-alert-callouts.svg)](https://pkg.go.dev/github.com/ZMT-Creative/gm-alert-callouts)
[![Go version](https://img.shields.io/github/go-mod/go-version/ZMT-Creative/gm-alert-callouts)](https://github.com/ZMT-Creative/gm-alert-callouts)
[![License](https://img.shields.io/github/license/ZMT-Creative/gm-alert-callouts)](./LICENSE.md)
[![GitHub Release](https://img.shields.io/github/v/release/ZMT-Creative/gm-alert-callouts?sort=semver&display_name=release)](https://github.com/ZMT-Creative/gm-alert-callouts/releases/latest)

A [Goldmark](https://github.com/yuin/goldmark) extension that provides support for GitHub-style alerts and Obsidian-style callouts with customizable icons and folding functionality.

## Breaking Changes

> [!WARNING]
>
> **The changes in this release are significant.** While every attempt has been made to create wrappers
> and helpers to keep things running smoothly over previous releases, there is a very real chance some things
> that worked for you in previous versions might work differently or not at all. Please test thoroughly!

## Features

- **GitHub Alerts**: Full support for GitHub's five standard alert types (`[!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]`, `[!CAUTION]`)
- **Obsidian Callouts**: Compatible with Obsidian-style callouts including folding (`+` for open, `-` for closed)
- **Customizable Icons**: Support for custom icon maps
- **Built-in IconSets**: Built-in icon sets Strict GFM alerts (just the five standard GFM Alerts), Hybrid (GFM, Alias and Obsidian-like Callouts) and Strict Obsidian callouts
- **Structured HTML**: Generates semantic HTML with CSS classes for easy styling
- **Nested Content**: Supports complex content including lists, code blocks, and other Markdown elements within alerts
- **Unicode Support**: You can name your custom alerts using Unicode characters (*experimental*) and create custom iconsets using kinds and aliases that use Unicode letters and numbers

## Installation

```bash
go get github.com/ZMT-Creative/gm-alert-callouts
```

## Quick Start

### Basic Usage

```go
package main

import (
    "bytes"
    "fmt"

    "github.com/yuin/goldmark"
    alertcallouts "github.com/ZMT-Creative/gm-alert-callouts"
)

func main() {
    // Create Goldmark with the extension using built-in GFM icons
    md := goldmark.New(
        goldmark.WithExtensions(
            alertcallouts.NewAlertCallouts(
                alertcallouts.UseGFMStrictIcons(),
                alertcallouts.WithFolding(true),
            ),
        ),
    )

    source := `# Example
> [!NOTE]
> This is a note with an icon.

> [!WARNING]-
> This is a closed warning callout.`

    var buf bytes.Buffer
    if err := md.Convert([]byte(source), &buf); err != nil {
        panic(err)
    }

    fmt.Println(buf.String())
}
```

### Pre-configured Extension

For convenience, a pre-configured extension is available:

```go
import (
    "github.com/yuin/goldmark"
    "github.com/yuin/goldmark/extension"
    alertcallouts "github.com/ZMT-Creative/gm-alert-callouts"
)

// Combining with other extensions
md := goldmark.New(
    goldmark.WithExtensions(
        extension.GFM,                 // GitHub Flavored Markdown
        extension.Footnote,            // Footnotes
        alertcallouts.AlertCallouts,   // Alert callouts
    ),
)
```

This pre-configured extension includes:

- GFM + Aliases icon set (adds aliases like `notes` -> `note` and `tips` -> `tip`)
- Folding functionality enabled
- Ready-to-use configuration

> [!IMPORTANT]
>
> When adding custom icons and icon sets in `gm-alert-callouts` you are **only** inserting the icon
> code into the HTML output. You still need to create the necessary CSS styling to format the final
> alert/callout style. An example of a CSS style file for the `Hybrid` built-in icon set can be
> found in the `examples/assets/css/alertcallouts-hybrid.css` file. This should provide a starting point
> for customizing the styling for your project.

## Supported Markdown Syntax

### Basic Alerts

```markdown
> [!NOTE]
> This is a note alert.

> [!TIP]
> This is a helpful tip.

> [!IMPORTANT]
> This is important information.

> [!WARNING]
> This is a warning message.

> [!CAUTION]
> This is a caution message.
```

### Foldable Callouts

When folding is enabled, use `+` (default open) or `-` (default closed):

```markdown
> [!TIP]+
> This callout is open by default and can be collapsed.

> [!WARNING]-
> This callout is closed by default and can be expanded.
```

### Multi-line Content

```markdown
> [!NOTE]
> This alert contains multiple paragraphs.
>
> - List item one
> - List item two
>
> And even code blocks:
> ```go
> fmt.Println("Hello, World!")
> ```
```

## Configuration Options

The extension supports functional options for flexible configuration:

- **Icon Sets**: `UseGFMStrictIcons()`, `UseHybridIcons()`, and `UseObsidianIcons()`
- **Custom Icons**: `WithIcon()`, `WithIcons()`
- **Functionality**:
  - `WithFolding()` (enable/disable collapsible callouts)
  - `WithCustomAlerts()` (enable/disable custom alerts - will use the default `note` or `info` icon for the set)
  - `WithAllowNOICON()` (enable/disable the use of the `noicon-` and `noicon_` prefixes to tag alert names to not display the icon)

For detailed configuration options and examples, see the [API Reference](docs/FEATURES.md#configuration-options).

## HTML Output

The extension generates semantic HTML with CSS classes for styling:

- **Basic alerts**: Uses `<div>` elements with `callout` and `callout-{type}` classes
- **Foldable callouts**: Uses `<details>` and `<summary>` elements for collapsible functionality
- **Structured content**: Separate containers for title, icon, and content areas

For complete HTML structure details and CSS class reference, see the [API Reference](docs/FEATURES.md#html-output-structure).

## Documentation

- **[API Reference](docs/FEATURES.md)** - Detailed API documentation and usage examples
- **[Icon Customization](docs/ICONMAPS.md)** - Guide to creating custom icons and icon maps

> [!NOTE]
>
> This extension and its documentation are still under active development. We have tried to be thorough
> about updating the documentation to reflect the changes to the codebase. However, it is certainly
> possible possible that some of the information in the documenation is inaccurate or just out-of-date. Always
> refer to the source code when in doubt about a feature or functionality.

## Requirements

- Go 1.23.0 or later
- [Goldmark](https://github.com/yuin/goldmark) v1.4.6 or later

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Project Lineage

Portions of this software are based on the work of others, used under their respective MIT
Licenses. In keeping with the requirements of the MIT License, here are the license notices for
these authors:

- [Adam Chovanec](docs/LICENSE-chovanec.md)
- [Thiago Okada](docs/LICENSE-thiagokokada.md)
