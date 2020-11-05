# Raspberry Pi scripts for maintenance

### Prints disc space
`df -h`

### Lists all packages and sorts them by size

`dpkg-query -Wf '${Installed-Size}\t${Package}\n' | sort -nr | more`

### Packages for removal

`sudo apt-get purge wolfram-engine`

`sudo apt-get remove --purge libreoffice`

`sudo apt-get remove --purge libreoffice-core`

`sudo apt-get remove --purge libreoffice-common`

`sudo apt-get remove --purge scratch`

`sudo apt-get remove --purge scratch2`

`sudo apt-get remove --purge scratch3`

`sudo apt-get remove --purge minecraft-pi`

`sudo apt-get remove --purge sonic-pi`

### Cleaning after removing packages

`sudo apt-get clean`

`sudo apt-get autoremove`

### Find all directories with size larger than 1GB (or 1MB if you change G to M after the pipe) and print only files up to 4th depth level
`sudo du -xh -d 4 / | grep -P "G\t"`

### Find all files of Megabyte'ish/Gigabyte'ish size in current directory
`ls -lah | grep "M "`

`ls -lah | grep "G "`

### Rotating logs and configuring lor rotate

https://www.linode.com/docs/guides/use-logrotate-to-manage-log-files/

### Print all services and their status

`systemctl list-units --type=service`

### Print all cron jobs

`crontab -l`