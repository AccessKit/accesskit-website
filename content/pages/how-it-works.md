+++
title = "How it Works"
template = "info-page.html"
path = "how-it-works"
insert_anchor_links = "left"

[extra]
header = { title = "How it Works" }
+++

## How does AccessKit Work?

Under the hood, AccessKit contains various components that work in tandem to provide a level accessibility experience, irrespective of the platform used.

### Data Schema

The heart of AccessKit is a data schema that defines all the data required to render an accessible UI for screen readers and other assistive technologies. The schema represents a tree structure, in which each node or branch is either a single UI element, or an element cluster such as a window, pain, or document. Each node has an integer ID, a role (e.g. button, edit, etc.) and a variety of optional attributes. The schema also defines actions that can be requested by assistive technologies, such as moving the keyboard focus, invoking a button, or selecting text. The schema is based largely on Chromium’s cross-platform accessibility abstraction.

The canonical definition of the schema is in the Rust programming language. Rust was chosen for its combination of efficiency and an expressive type system. Representations of the schema in other programming languages and schema definition languages (such as JSON Schema or Protocol Buffers) can be generated from the Rust code.

#### Serialization

When both the toolkit and the platform adapter (see below) are written in Rust or another language that can efficiently access Rust data structures (such as C++), the data defined in the schema can be passed back and forth with no serialization overhead. In other cases, serialization will be required to minimize the overhead of language interoperability. Because the schema supports Serde, each language binding can choose its own serialization format.

#### Known Issues

- The schema doesn’t yet define any events. While some events in platform accessibility APIs, such as focus change and property changes, can be implied from tree updates, other 
  events, such as ad-hoc announcements meant for screen reader users, cannot.
- The in-memory representation of a node has not yet been optimized.

[Access the schema here](https://github.com/AccessKit/accesskit/tree/main/common)

### Platform Adapters

These are the libraries that implement the accessibility API’s across various platforms. This cuts down significantly on manual legwork when designing your app, as individual accessibility implementations will not need to be manually created for each platform.

The interaction between the provider (toolkit or application) and the platform adapter is also inspired by Chromium. Because Chromium has a multi-process architecture and does not allow synchronous IPC from the browser process to the sandboxed renderer processes, the browser process cannot pull accessibility information from the renderer on demand. Instead, the renderer process pushes data to the browser process. The renderer process initially pushes a complete accessibility tree, then it pushes incremental updates. The browser process only needs to send a request to the renderer process when an assistive technology requests one of the actions mentioned above. In AccessKit, the platform adapter is like the Chromium browser process, and the UI toolkit is like the Chromium renderer process, except that both components run in the same process and communicate through normal function calls rather than IPC.

One notable consequence of this design is that only the platform adapter needs to retain a complete accessibility tree in memory. That means that this design is suitable for immediate-mode GUI toolkits, as long as they can provide a stable ID for each UI element.

The platform adapters are written primarily in Rust. We’ve chosen Rust for its combination of reliability and efficiency, including safe concurrency, which is especially important in modern software. Some future adapters may need to be partially written in another language, such as Java or Kotlin for the Android adapter.

#### Windows

The Windows adapter currently implements the UI Automation API.

It doesn’t yet support all possible widget types, but it can now be used to make real, non-trivial applications accessible. In particular, it supports both single-line and multi-line text edit controls, but not yet rich text.

[The Windows Platform Adapter can be found here](https://github.com/AccessKit/accesskit/tree/main/platforms/windows)

#### Mac OS

The Mac OS platform exposes an AccessKit accessibility tree through the Cocoa NS accessibility protocol. It is roughly at feature parity with the Windows adapter, including support for text edit controls.

[Access the Mac OS Platform Adapter here](https://github.com/AccessKit/accesskit/tree/main/platforms/macos)

#### Unix

The Unix adapter, which implements the D-Bus-based AT-SPI protocol, is available in [the AccessKit Unix crate](https://crates.io/crates/accesskit_unix), in [the platforms/unix directory](https://github.com/AccessKit/accesskit/tree/main/platforms/unix). This adapter doesn’t yet fully support text edit controls. It is also not yet usable with the Orca screen reader, due to a keyboard input handling issue that we are working with the appropriate GNOME development teams to solve.

### Adapters for cross-platform windowing layers

#### Winit

This is a platform adapter for the popular [Winit Rust GUI library](https://lib.rs/crates/winit).

Accessibility trees are exposed to the accessibility API of the platform in question.

[Access the Winit platform adapter here](https://github.com/AccessKit/accesskit/tree/main/platforms/winit)

#### Glazier

AccessKit is also directly integrated into the [glazier](https://github.com/linebender/glazier) crate. Contrary to the Winit adapter, AccessKit has been fully integrated in Glazier from the ground up.

### Planned Adapters

The following adapters are planned:

- Android
- iOS
- web (accomplished through the creation of a hidden HTML DOM)

### Consumer Library

Some of the code required by the platform adapters is platform-independent. This code is in [the AccessKit Consumer Crate](https://crates.io/crates/accesskit_consumer), in [the consumer directory](https://github.com/AccessKit/accesskit/tree/main/consumer). In addition to platform adapters, this library may also be useful for implementing embedded assistive technologies, such as a screen reader running directly inside an application, for platforms that don’t yet have an AccessKit platform adapter, or for devices that don’t have platform support for accessibility at all, such as game consoles and appliances.

### Language Bindings

UI toolkit developers who merely want to use AccessKit should not be required to use Rust directly. In addition to a direct Rust API, AccessKit provides a C API covering both the core data structures and all platform adapters. This C API can be used from a variety of languages. The Rust source for the C bindings is in the [bindings/c directory](https://github.com/AccessKit/accesskit/tree/main/bindings/c). The AccessKit project also provides a pre-built package, including a header file, both dynamic and static libraries, and sample code, for the C API, so toolkit developers won’t need to deal with Rust at all. The latest pre-built package can be found [here](https://github.com/AccessKit/accesskit-c/releases).
