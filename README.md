# boot-grub
howto

# Afficher menu GRUB avec UUIDs d'un système EN ligne
Celui qui sera affiché au prochain boot
## Version brute
```sh
awk '/^(menuentry|submenu)/' /boot/grub/grub.cfg
```
## Version lisible
```sh
# Nota : ne gère PAS les formats FS suivants (car le format des UUIDs différents) : NTFS, LVM2
gawk '/^(menuentry|submenu)/ { printf '\
' gensub( /[^\x27]+\x27([^\x27]+)\x27.+/ , "\\1" , "g" ) ; '\
' uuid = gensub( '\
'  /.+'\
'([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}).*/ '\
' , "\\1" , "g" ) ; '\
' if ( uuid != $0 ) { printf " [" uuid "]" } printf "\n" }' \
  /boot/grub/grub.cfg
```
