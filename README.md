# usbip

## Bringup driver
```
sudo apt install linux-tools-common
sudo apt install linux-tools-5.4.0-1015-raspi
sudo modprobe usbip_host
echo 'usbip_host' >> /etc/modules
```

## Bringup service
```
vi /lib/systemd/system/usbipd.service   
   
[Unit]
Description=usbip host daemon
After=network.target

[Service]
Type=forking
ExecStart=/usr/sbin/usbipd -D
ExecStartPost=/bin/sh -c "/usr/sbin/usbip bind --$(/usr/sbin/usbip list -p -l | grep '#usbid=10c4:8a2a#' | cut '-d#' -f1)"
ExecStop=/bin/sh -c "/usr/sbin/usbip unbind --$(/usr/sbin/usbip list -p -l | grep '#usbid=10c4:8a2a#' | cut '-d#' -f1); killall usbipd"

[Install]
WantedBy=multi-user.target
```
Now reload the service
```
# reload systemd, enable, then start the service
sudo systemctl --system daemon-reload
sudo systemctl enable usbipd.service
sudo systemctl start usbipd.service
```

## NodeJS

```
sudo apt install nodejs
sudo apt install npm
sudo apt install nvm
```
