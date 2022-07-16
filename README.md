# Drumstick::RT Out-of-Tree Template Plugin (drumstick-rt-outoftree)

This project is a template/example for building [Drumstick::RT](https://github.com/pedrolcl/drumstick) plugins, as an out-of-tree [^1] project.

There are already a number of [in-tree Drumstick::RT plugins](https://github.com/pedrolcl/drumstick/tree/master/library/rt-backends), the Dummy ones between them. Nothing forbids to build and distribute other independent plugins, and this project may be a template for that projects.

The rules for creating and deploying Drumstick::RT plugins are the same as other [Qt Plugins](https://doc.qt.io/qt-6/plugins-howto.html#the-low-level-api-extending-qt-applications).
[Deployment rules](https://doc.qt.io/qt-6/deployment-plugins.html) mandate that plugins must share the same build and runtime Qt library major version with the applications using the plugins, and not be built with a higher minor version.

A plugin may implement MIDI OUT or MIDI IN functionality for applications. To do so, the plugins must extend either the [MIDIOutput](https://github.com/pedrolcl/drumstick/blob/master/library/include/drumstick/rtmidioutput.h) or the [MIDIInput](https://github.com/pedrolcl/drumstick/blob/master/library/include/drumstick/rtmidiinput.h) interfaces.

You may want to read also the [documentation](https://drumstick.sourceforge.io/docs/index.html) about [Drumstick](https://drumstick.sourceforge.io/) and some of the utilities and examples.

Two plugins are included here: `drumstick-rt-dummy-in` and `drumstick-rt-dummy-out`, that implement the `MIDIInput` and `MIDIOutput` interfaces respectively, but without functionality.

When installed, both plugins should be found and used by any Drumstick::RT based application, like [VMPK](https://vmpk.sourceforge.io) and [dmidiplayer](https://dmidiplayer.sourceforge.io).

External RT plugins may have their own configuration dialogs and be developed and distributed out of tree. The same rules and functions for configuration dialogs should apply to them.

The plugins including an internal configuration dialog should expose the property:

    Q_PROPERTY(bool isconfigurable ...)

And also a public slot with this name and signature:

    bool configure(QWidget *parent);

Use this template to write your own plugins and enjoy!

[^1] Located outside of a source tree. Antonym: in-tree‎. [Source](https://www.wordsense.eu/out-of-tree/). This software engineering term was originated in the Linux kernel development community, [probably as a pun](https://www.collinsdictionary.com/dictionary/english/out-of-your-tree).
