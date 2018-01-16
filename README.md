Install [msys2](http://www.msys2.org/)
and open a "MSYS2 MinGW 64-Bit" shell.


Run the following commands to clone and start building:
```
pacman -Syuu
pacman -S git
git clone https://github.com/mkeeter/guile-mingw
cd guile-mingw
makepkg-mingw -s
```

It will make it almost all the way,
then fail when building documentation.
