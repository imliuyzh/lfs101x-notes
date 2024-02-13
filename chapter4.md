# Graphical Interface

## X Window System

Loading the graphical desktop is one of the final steps in the boot process of a Linux desktop. Historically, this was known as the X Windows System, often just called X.

X is rather old software; it dates back to the mid-1980s and, as such, has certain deficiencies on modern systems (for example, with security), as it has been stretched rather far from its original purposes. A newer system, known as Wayland, is gradually superseding it and is the default display system for Fedora, RHEL, and other recent distributions. For the most part, it looks just like X to the user, although under the hood it is quite different.

## More About the Graphical Desktop

A desktop environment consists of a session manager, which starts and maintains the components of the graphical session, and the window manager, which controls the placement and movement of windows, window title-bars, and controls. Although these can be mixed, generally a set of utilities, session manager, and window manager are used together as a unit, and together provide a seamless desktop environment.

## Desktop Environments

GNOME is a popular desktop environment with an easy-to-use graphical user interface. It is bundled as the default desktop environment for most Linux distributions, including Red Hat Enterprise Linux (RHEL), Fedora, CentOS, SUSE Linux Enterprise, Ubuntu, and Debian. However, the look and feel can be quite different across distributions, even if they are all using GNOME.

Another common desktop environment very important in the history of Linux and also widely used is KDE, which has often been used in conjunction with SUSE and openSUSE. Other alternatives for a desktop environment include Unity (present on older Ubuntu but still based on GNOME), XFCE, and LXDE.

## `gnome-tweaks`

There are many settings which many users would like to modify which are not thereby accessible. Fortunately, `gnome-tweaks` exposes many more setting options. It also permits you to easily install extensions by external parties. Not all Linux distributions install this tool by default, but it is always available (older distributions used `gnome-tweak-tool`). Some recent distributions have taken most of the functionality out of this tool and placed it in a new one called `gnome-extensions-app`.
