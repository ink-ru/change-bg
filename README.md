# change-bg
## linux change desktop background in terminal (CLI)

You can use `dconf` to change background. Here is listing of `change-bg` bash script:

```shell
#!/bin/bash

WP="$(find ~+ -type f -exec mimetype {} + 2>/dev/null | awk -F': +' '{ if ($2 ~ /^image\//) print $1 }' | sort -R | tail -30 | shuf -n 1)"

dconf write /org/mate/desktop/background/picture-filename "'${WP}'"
```
You can find distro-specific key using GUI app - `dconf-editor`

But to use this script in CRON you need to set session environment variables. Command pgrep gnome-session doesn't work in Mint and other not Gnome desktops. To solve this problem you need to save environment variables of specific user by running command at system startup:

```shell
env > ~/cronenv && sed -i '/%s/d' ~/cronenv
```
You can find this code in `save-cronenv` file. You can add this script to startup aplications.

Now you have `cronenv` file (without substitutional vars - %s) in users home dir. Just restore them back in cron before running `dconf`:

```shell
*/1 7-21 * * * cd ~/Pictures && env $(cat ~/cronenv | xargs) /path/to/first/script/bg
Use crontab -e to edit cron jobs for current user. All works fine!
```
