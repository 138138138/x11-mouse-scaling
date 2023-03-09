# **Mouse Scaling Workarounds in Linux X11/Xorg**

# Introduction

One of the most troublesome thing in using Linux is scaling.

Most of scaling issues can be fixed using Wayland. However, not all apps and hardware support it.

In X11, if you scale the desktop to 200%, **you will find the mouse moving twice as slow.**

# Workarounds

Unfortunately, not all distros and desktop environment provide the same configurations to fix the problem.

The easiest one is Fedora.

https://bugzilla.redhat.com/show_bug.cgi?id=1413306

As state in the website, if you want to scale 200%, add this config.

```
sudo nano /etc/X11/xorg.conf.d/99-scalefix-fedora.conf
```

```
Section "InputClass"
    Identifier "scalefix"
    Option "DPIScaleFactor" "2.0" # ratio your dpi : 96dpi
EndSection
```

Where `2.0` is the scaling factor.

However, this only works in Fedora. In other distros, you will have to use the `TransformationMatrix` parameter. **Be aware this parameter occasionally causes the mouse to jump around.**

https://gitlab.freedesktop.org/xorg/xserver/-/issues/633

If you really cannot use Fedora but want X11 mouse scaling, use this config.

```
sudo nano /etc/X11/xorg.conf.d/99-scalefix-others.conf
```

```
Section "InputClass"
    Identifier "scalefix"
    Option "TransformationMatrix" "2.0 0 0 0 2.0 0 0 0 1"
EndSection
```

Where `2.0` is the scaling factor.

# Fractional Scaling Workarounds

If you use non-integer scaling, like 150% or 250%, you need to see which desktop environment you are using.

For **GNOME** and **GTK-based** desktops, use scaling factor of `2.0` if you scale at 125%, 150%, 175%, and `3.0` if you scale at 225%, 250%, 275%, etc.

This is because these desktops scale up the entire desktop to nearest integer, then scale down to fit your monitor.

For **KDE** desktop, set the actual scaling factor. If you scale at 150%, set it to `1.5`.

KDE desktop scales fractionally without the need to scale down.

# Conclusion

If you want proper mouse scaling in X11, use Fedora. This is the sad reality.

Or use Wayland if your hardware supports it.
