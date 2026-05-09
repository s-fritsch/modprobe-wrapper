# modprobe-wrapper

A modprobe wrapper with a whitelist for module auto-loading.

## Summary

modprobe-wrapper is a very simple wrapper for modprobe that implements a
whitelist of modules that are allowed to be auto-loaded by the linux kernel.
It is mainly useful for servers with a relatively static configuration for
attack surface reduction.

## Usage

Write the path to modprobe-wrapper into `/proc/sys/kernel/modprobe` late in the
boot process. This means that modules that are loaded during boot need not be
present in the whitelist.

Put the aliases you want to allow into into `/etc/modprobe-wrapper/whitelist`
(one per line, no trailing whitespace).

Put aliases you want to be denied quietly without a log message into
`/etc/modprobe-wrapper/blacklist.quiet`.  This can be useful if software
triggers some modprobe event again and again for a module that is not present
in your kernel

When you install or update software of if something does not work as expected
on your server, always check syslog for modprobe-wrapper's log messages.
Failed module auto-loading may make software fail in very strange ways with
misleading error messages.

Compared to `/proc/sys/kernel/modules_disabled`, modprobe-wrapper gives you
some logging and allows to adapt the whitelist without reboot.

## Caveats

* Explicit calls to modprobe or insmod bypass the whitelist check. This may be a feature.

* The whitelist needs to contain the module alias (e.g.
  `net-pf-16-proto-4-type-2`) that triggers the load. The log message also only
  contains this alias. Use modinfo to check what the actual module name is.
