# Drumstick::RT Out of Tree Template Plugin (drumstick-rt-outoftree)

This project is a template/example for building a Drumstick::RT plugin, as an out-of-tree project.

Two plugins are included: `drumstick-rt-dummy-in` and `drumstick-rt-dummy-out`, that implement the `MIDIInput` and `MIDIOutput` interfaces respectively, but without functionality.

When installed, both plugins should be found and used by any Drumstick::RT based application, like [VMPK](https://vmpk.sourceforge.io) and [dmidiplayer](https://dmidiplayer.sourceforge.io).

External RT plugins may have their own configuration dialogs and be developed and distributed out of tree. The same rules and functions for configuration dialogs should apply to them.

The plugins including an internal configuration dialog should expose the property:

    Q_PROPERTY(bool isconfigurable ...)

And also a public slot with this name and signature:

    bool configure(QWidget *parent);

Use this template to write your own plugins and enjoy!
