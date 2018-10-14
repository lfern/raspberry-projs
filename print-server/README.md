# Raspberry Pi Print Server

First this
```bash
sudo apt-get update
sudo apt-get install cups samba
sudo usermod -a -G lpadmin pi
```
Then this:
```bash
sudo cupsctl --remote-any
sudo /etc/init.d/cups restart
```
Or this:
```bash
sudo nano /etc/cups/cupsd.conf
sudo /etc/init.d/cups restart
```

Change/add the following lines to the configuration file.

```config
# Only listen for connections from the local machine.

#Listen localhost:631

#CHANGED TO LISTEN TO LOCAL LAN

Port 631

# Restrict access to the server...
<Location />
  Order allow,deny
  Allow @Local
</Location>


# Restrict access to the admin pages...
<Location /admin>
  Order allow,deny
  Allow @Local
</Location>


# Restrict access to configuration files...
<Location /admin/conf>
  AuthType Default
  Require user @SYSTEM
  Order allow,deny
  Allow @Local
</Location>
```



http://192.168.1.105:631



```bash
sudo apt-get install samba
sudo nano /etc/samba/smb.conf
```

Modify [printers] section for CUPS printing.

```config
[printers]
comment = All Printers
browseable = no
path = /var/spool/samba
printable = yes
guest ok = yes
read only = yes
create mask = 0700
```

Modify [print$] section for windows:
```config
[print$]
comment = Printer Drivers
path = /var/lib/samba/printers
browseable = yes
read only = no
guest ok = no
```

```bash
sudo /etc/init.d/samba restart
```
