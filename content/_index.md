+++
title = "Recent Posts"
sort_by = "date"
template = "section.html"

[extra]
header = { title = "AccessKit" }
section_path = "blog/_index.md"
max_posts = 5
+++

## Accessibility infrastructure for UI toolkits

AccessKit makes it easier to implement accessibility, for screen readers and other assistive technologies, in toolkits that render their own user interface elements. It provides a cross-platform, cross-language abstraction over accessibility APIs, so toolkit developers only have to implement accessibility once.

## Getting started

AccessKit is written in Rust and has bindings for C and Python.

### Rust

The following Rust projects already integrate AccessKit:

* [Bevy](https://bevyengine.org/)
* [egui](https://github.com/emilk/egui)
* [Slint](https://slint.dev/)
* [Xilem](https://github.com/linebender/xilem)

If you are using the [winit](https://crates.io/crates/winit) cross-platform windowing library for Rust, you should use the [AccessKit winit adapter](https://crates.io/crates/accesskit_winit). Otherwise, you'll need to use one or more of the AccessKit platform adapters:

* [macOS adapter](https://crates.io/crates/accesskit_macos)
* [Unix adapter](https://crates.io/crates/accesskit_unix)
* [Windows adapter](https://crates.io/crates/accesskit_windows)

### C

The latest release of the AccessKit C bindings, including the header file, pre-built libraries, examples, and source code, can be found on the [GitHub releases page for the C bindings](https://github.com/AccessKit/accesskit-c/releases).

### Python

You can find the [AccessKit Python bindings on PyPI](https://pypi.org/project/accesskit/).
