# Raspberry Pi scripts for maintenance

### Prints disc space
`df -h`

### Lists all packages and sorts them by size

`dpkg-query -Wf '${Installed-Size}\t${Package}\n' | sort -nr | more`

### Packages for removal

`sudo apt-get remove --purge wolfram-engine`

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

### Print cron logs from syslog

`grep CRON /var/log/syslog`

`tail -f /var/log/syslog`

## Backup through ssh

### check where is your disc in `/dev`

`lsblk -p`

### copy whole disc content over ssh (gzipped)

`ssh pi@192.168.1.38 'sudo dd bs=4M if=/dev/mmcblk0 status=progress | gzip' | dd of=pi_os_backup.gz`

### additional info on backup restoring

https://www.raspberrypi.org/documentation/installation/installing-images/linux.md

### save ssh key

`ssh-copy-id -i ~/.ssh/klucz grizwold@192.168.1.38`
https://www.ssh.com/ssh/copy-id

### ssh access for headless

For headless setup, SSH can be enabled by placing a file named ssh, without any extension, onto the boot partition of the SD Card. When the Raspberry Pi boots, it looks for the ssh file. If it is found, SSH is enabled and the file is deleted. The content of the file does not matter; it could contain text, or nothing at all.

### prepare WIFI for headless

https://raspberrypi.stackexchange.com/questions/10251/prepare-sd-card-for-wifi-on-headless-pi

The Raspberry Pi Foundation's Raspberry Pi Imager now has an advanced options menu which is accessed by the keyboard shortcut Ctrl+Shift+X.
You can set hostname, allow SSH (including changing user password), configure wifi and set locale. Note that this tool also appears to have telemetry built in, which can be turned off from the GUI.

### Controlling RPi from Android 

https://play.google.com/store/apps/details?id=it.Ettore.raspcontroller&hl=en&gl=US

### Working with GPIO

https://www.ics.com/blog/gpio-programming-using-sysfs-interface

### Welcome screen

Custom bash welcome screen. Create file `/home/pi/.bash_profile`:

```
let upSeconds="$(/usr/bin/cut -d. -f1 /proc/uptime)"
let secs=$((${upSeconds}%60))
let mins=$((${upSeconds}/60%60))
let hours=$((${upSeconds}/3600%24))
let days=$((${upSeconds}/86400))
UPTIME=`printf "%d days, %02dh%02dm%02ds" "$days" "$hours" "$mins" "$secs"`

# get the load averages
read one five fifteen rest < /proc/loadavg

echo "$(tput setaf 2)
   .~~.   .~~.    `date +"%A, %e %B %Y, %r"`
  '. \ ' ' / .'   `uname -srmo`$(tput setaf 1)
   .~ .~~~..~.
  : .~.'~'.~. :   Uptime.............: ${UPTIME}
 ~ (   ) (   ) ~  Memory.............: `cat /proc/meminfo | grep MemFree | awk {'print int($2/1024)'}`MB (Free) / `cat /proc/meminfo | grep MemTotal | awk {'print int($2/1024)'}`MB (Total)
( : '~'.~.'~' : ) Free Disk Space....: `df -Pkh | grep -E '^/dev/root' | awk '{ print $4 }' | awk -F '.' '{ print $1 }'` on /dev/root
 ~ .~ (   ) ~. ~  Load Averages......: ${one}, ${five}, ${fifteen} (1, 5, 15 min)
  (  : '~' :  )   Running Processes..: `ps ax | wc -l | tr -d " "`
   '~ .~~~. ~'
       '~'
$(tput sgr0)"
```