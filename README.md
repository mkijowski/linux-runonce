Create an `@reboot` entry in your crontab to run a script called `/usr/local/bin/runonce`.

Create a directory structure called `/etc/local/runonce.d/ran` using `mkdir -p`.

Create the script `/usr/local/bin/runonce` as follows:
```
#!/bin/sh
for file in /etc/local/runonce.d/*
do
    if [ ! -f "$file" ]
    then
        continue
    fi
    "$file"
    mv "$file" "/etc/local/runonce.d/ran/$file.$(date +%Y%m%dT%H%M%S)"
    logger -t runonce -p local3.info "$file"
done
```

Now place any script you want run at the next reboot (once only) in the directory `/etc/local/runonce.d` and `chown` and `chmod +x` it appropriately. Once it's been run, you'll find it moved to the `ran` subdirectory and the date and time appended to its name. There will also be an entry in your `syslog`.
