

Copy files to NAS
```BASH
sudo rsync --ignore-existing -czraP -t --progress --log-file=/home/rsync.progress.log /mnt/e/_zaloha\ NAS/Mamča lantean@dakara:/mnt/HomeArchive/tmp/
```

restart SCALE UI
```BASH
sudo systemctl restart middlewared
```