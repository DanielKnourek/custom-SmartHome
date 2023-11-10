# Permissions for SMB share

## ~/HomeArchive/HomeArchiveData

```BASH
sudo -i
cd /mnt/HomeArchive/HomeArchiveData

install -d -m 0770 -o daniel -g noFamily Daniel/
install -d -m 0770 -o vaclav -g noFamily Táta/
install -d -m 0770 -o vasek -g noFamily Vašek/
install -d -m 0770 -o blanka -g noFamily Mamča/

install -d -m 0770 -o root -g Family Sdilene/
install -d -m 0770 -o root -g Family Media/
install -d -m 0770 -o root -g Family Media/Filmy\&Pořady
install -d -m 0770 -o root -g Family Media/Hudba
install -d -m 0770 -o root -g Family Media/Muzeum
```

```sh
# permission v2
lantean@dakara:~$ ls -la /mnt/HomeArchive/HomeArchiveData/
total 282
d---r-x---  8 root   Family    8 Oct 12  2022 .
drwxr-xr-x  3 root   root      3 Aug 25  2022 ..
drwxr-x--- 25 daniel noFamily 43 Jun 18 17:00 Daniel
drwxr-x--- 10 blanka noFamily 13 Oct 12  2022 Mamča
d---r-x---  6 root   Family    8 Sep  2  2022 Media
drwxrwx---  8 root   Family   19 Aug 28 04:03 Sdilene
drwxr-x--- 12 vaclav noFamily 12 Sep  1 22:03 Táta
drwxr-x--- 22 vasek  noFamily 30 Aug 25  2022 Vašek
```
