+++
title = "Looking back; looking forward"
path = "looking-back-looking-forward"
authors = ["Matt Campbell"]
date = 2023-01-01T19:59:24Z
+++

a message from our founder

2022 has been a great year for AccessKit, thanks to continued funding from the Google Fonts team. As we celebrate the new year, I’d like to look back on what we’ve accomplished in 2022, and look forward to the work that still needs to be done to fulfill AccessKit’s potential.

## Progress in 2022

At this time last year, AccessKit was a proof-of-concept on Windows only, with support for only a handful of user interface control types, not including text edit controls, which are the most notoriously difficult type of control to make accessible. Now, with platform adapters for both Windows and macOS, robust support for both single-line and multi-line text edit controls, and enhanced support for other control types including sliders and steppers, AccessKit is ready to start making real applications accessible.

As evidence that AccessKit is ready for production, it is now integrated into [egui](https://github.com/emilk/egui), a popular Rust GUI toolkit. This makes egui, as far as we know, both the first pure-Rust GUI toolkit and the first immediate-mode GUI to implement platform accessibility APIs. AccessKit is enabled by default in [eframe](https://github.com/emilk/egui/tree/master/crates/eframe), the official ready-to-use application framework on top of egui. Other egui integrations, such as [bevy_egui](https://github.com/mvlabat/bevy_egui), will need to integrate AccessKit as well. Check out a video demonstration of AccessKit in an Egui application on Windows. 

<iframe title="YouTube video player" src="https://www.youtube.com/embed/ffbbQjTq5Pg" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

I’d like to thank [Emil Ernerfeldt](https://github.com/emilk), the primary developer of egui, for enthusiastically working with me to complete this integration. Also, thanks again to the Google Fonts team, for funding not only my work on AccessKit itself, but the integration in egui.

Speaking of that Google team, now that they’ve established a clear direction for their future research by starting the [xilem](https://github.com/linebender/xilem) and [glazier](https://github.com/linebender/glazier] projects, we’ve started the work on integrating AccessKit into these projects as well. AccessKit is already integrated into glazier, an abstraction over the various platform windowing APIs, similar to [winit](https://crates.io/crates/winit) but with an emphasis on deep integration with the platform for rich desktop applications. Unlike [AccessKit’s winit adapter](https://crates.io/crates/accesskit_winit), a bolted-on solution which requires some inelegant workarounds, the integration in glazier is direct and clean. We look forward to an equally clean, first-class integration of AccessKit in xilem, the new GUI toolkit being built on top of glazier.

Work on integrating AccessKit in other GUI toolkits is also underway. In particular, [Vizia](https://github.com/vizia/vizia) will soon have AccessKit support. This integration is being done primarily by Vizia’s main developer, [George Atkinson](https://github.com/geom3trik).

We even have a proof-of-concept integration of AccessKit into a non-Rust environment, specifically, the Unity game engine. You can check out this integration in action in our [modified version of an existing open-source Unity-based game](https://github.com/AccessKit/the-intercept). I presented this demo in [a talk at the NarraScope 2022 online conference](https://www.youtube.com/watch?v=m9UkOuXu6Z4), my first conference talk featuring AccessKit. As a result of this talk, at least one independent game developer is now interested in making their games accessible with AccessKit. This integration was particularly challenging because we have had no cooperation, at least so far, from the Unity developers. As a result, you may notice that in the demo, the window isn’t full-screen as it was in the original game, and it flickered once on startup. However, we believe we now have a solution to this technical problem. Thanks to [Arnold Loubriat](https://github.com/DataTriny), my main volunteer collaborator on AccessKit for more than a year, for doing most of the hard work on this project.

## Looking forward

As happy as I am with AccessKit’s progress in the past year, we’re not done yet. There’s much more work to do before AccessKit reaches its potential as universal, reusable accessibility infrastructure.

First, before it can be used to make the full range of desktop and mobile applications accessible, AccessKit needs to support more types of controls, including list views, drop-down lists, combo boxes, menus, scrollable areas, tabs, tables, and tree views. In some cases, this will be as simple (though tedious) as mapping specific properties in platform accessibility APIs to their counterparts in AccessKit. AccessKit itself already has many, if not all, of these properties, thanks to the schema that we adapted from Chromium’s cross-platform accessibility abstraction early in the project. But in some cases, we will probably need to go through some trial and error to successfully account for the different conventions around concepts such as focus, selection, and popups on different platforms.

While our support for text edit controls is robust within its current intended scope, it’s not yet comprehensive. In particular, AccessKit doesn’t yet expose formatting information, such as font name and size, style attributes such as bold, italic, and underline, colors, headings, and lists. Exposing some of these attributes will be straightforward, but we expect that the moderately more difficult part will be supporting text with a mix of different values for these attributes. We’d eventually also like to support hypertext, including embedded links and images, but this is a much more difficult project, which we will most likely postpone for a year or more.

As AccessKit is used to make more complex and data-rich user interfaces accessible, we may find it necessary to spend time on optimization. AccessKit’s current use of a single large data structure to represent all types of accessible UI elements is a known potential weakness in the design. However, we want to take our time to explore alternatives, and collect more data on the performance of AccessKit in real applications, before we commit to a different design. If and when we do make a major design change, we will do everything we can to ease the migration for existing users.

In the shorter term, we are eager to expand AccessKit to more platforms. As I write this, the platform adapter for GNOME and other Unix-based desktop environments that use the AT-SPI protocol, developed primarily by [Arnold Loubriat](https://github.com/DataTriny), is almost ready for production. You can check out its current state in the [pull request](https://github.com/AccessKit/accesskit/pull/198). We also plan to develop adapters for Android and iOS. Finally, and probably most difficult of all, we want to develop an adapter for the web platform, for applications and toolkits such as egui that use the HTML canvas element. The prioritization of these platforms is entirely dependent on future funding and volunteer contributions.

We are also excited about the potential of AccessKit to be reusable across programming languages, beyond Rust. We are already working on polishing our proof-of-concept Unity integration into a truly ready-to-use plugin. We also plan to integrate AccessKit into at least one project based on the Java Virtual Machine, which we’ll announce when it’s ready. Support for other languages is, again, dependent on future funding and volunteer contributions.

As AccessKit becomes more widely used, thorough and approachable documentation will become more important. We realize that most GUI developers don’t deeply understand accessibility as we do, and we need to make that understanding more widely available. Watch this site for more documentation, including introductory documentation for beginners, in the coming months.

## Conclusion

We believe AccessKit is gaining momentum, and we can’t wait to find out where it will go in the coming year. To keep this momentum going, we need funding to continue our work. If you’d like to directly fund my work on AccessKit, checkout [my new GitHub Sponsors profile](https://github.com/sponsors/mwcampbell). Of course, as an open-source project, AccessKit is also open to volunteer contributions, especially in GUI toolkit integrations, programming language support, and documentation. Let’s work together to make many more applications accessible in 2023!
