#YAOURT

1. Install 'base-devel' dan 'yajl' kalau belum dengan : `sudo pacman -S --needed base-devel yajl`
2. Buat folder untuk download dan kompile paket dari AUR misal di '/tmp/AUR' dengan : `mkdir -p ~/temp/AUR/ && cd ~/temp/AUR/`
3. Install 'package-query' dengan :
  ```
  wget https://aur.archlinux.org/cgit/aur.git/snapshot/package-query.tar.gz
  tar xfz package-query.tar.gz
  cd package-query  &&  makepkg
  sudo pacman -U package-query*.pkg.tar.xz
  ```

4. Install 'yaourt' dengan :
  ```
  wget https://aur.archlinux.org/cgit/aur.git/snapshot/yaourt.tar.gz
  tar xzf yaourt.tar.gz
  cd yaourt  &&  makepkg
  sudo pacman -U yaourt*.pkg.tar.xz
  ```

5. Yaourt siap dipakai


Referensi :
- https://astrofloyd.wordpress.com/2015/01/17/installing-yaourt-on-arch-linux/
- http://thestandardoutput.com/posts/fixing-package-query-to-pacman-dependency-in-arch/
