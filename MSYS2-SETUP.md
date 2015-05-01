# Install MSYS2

Use installer and then update packages to latest following instructions at:

http://sourceforge.net/p/msys2/wiki/MSYS2%20installation/

You can use any of the three shells to install packages but using the mingw
applications(like gitk) or building mingw software will require using a mingw
2/64 shell so the PATH is setup correctly.

# Install git and gitk

$ pacman -S git

Tk is needed for gitk

$ pacman -S mingw-w64-x86_64-tk mingw-w64-i686-tk

# Install Config files

copy ssh keys to ~/.ssh

clone dotfiles repo and run install-msys2.sh to copy files into ~/

# Install package Dependencies

Install MSYS2 python3 package

$ pacman -S python3

Install gcc

pacman -S mingw-w64-i686-gcc mingw-w64-x86_64-gcc

Install all flavours of pkgconfig

$ pacman -S pkg-config mingw-w64-i686-pkgconf mingw-w64-x86_64-pkgconf

Install libsndfile library

$ pacman -S mingw-w64-i686-libsndfile mingw-w64-x86_64-libsndfile

Install Gtk+2

$ pacman -S mingw-w64-i686-gtk2 mingw-w64-x86_64-gtk2

Install Gtk+3

$ pacman -S mingw-w64-i686-gtk3 mingw-w64-x86_64-gtk3

Install Gtkmm2

$ pacman -S mingw-w64-i686-gtkmm mingw-w64-x86_64-gtkmm

Install Gtkmm3

$ pacman -S mingw-w64-i686-boost mingw-w64-x86_64-boost
