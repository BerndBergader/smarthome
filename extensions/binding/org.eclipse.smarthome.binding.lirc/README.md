# LIRC Binding

This binding integrates infrared transceivers through [LIRC](http://www.lirc.org) or [WinLIRC](http://winlirc.sourceforge.net).

A list of remote configuration files for LIRC is available [here](http://lirc-remotes.sourceforge.net/remotes-table.html).


## Supported Things

This binding supports LIRC and WinLIRC as bridges for accessing the configured remotes.

LIRC must be started with TCP enabled. On systemd based systems, TCP can be enabled by editing the file
`/usr/lib/systemd/system/lirc.service` and adding `--listen` to the end of the `ExecStart` line. An example
systemd service file for LIRC is shown below.

```ini
[Unit]
Description=Linux Infrared Remote Control
After=network.target

[Service]
RuntimeDirectory=lirc
ExecStart=/usr/sbin/lircd --nodaemon --driver=default --device=/dev/lirc0 --listen

[Install]
WantedBy=multi-user.target
```
By default, LIRC will listen on IP address 0.0.0.0 (any available IP address) and port 8765. If you would
rather run LIRC on a specific port or IP address, you can use `--listen=192.168.1.100:9001` instead.


## Discovery

Discovery of the LIRC bridge is not supported. However, remotes will be automatically discovered once
a bridge is configured.

## Thing Configuration

```xtend
Bridge lirc:bridge:local [ host="192.168.1.120", port="9001" ] {
    Thing remote RCA_RS2640 [ remote="RCA_RS2640" ]
}
```
Bridge:
* **host**: IP address or hostname of the LIRC server. Defaults to localhost
* **port**: The port number the LIRC server is listening on. Defaults to 8765

Remote:
* **remote**: The name of the remote as known by LIRC


## Channels

This binding currently supports following channels:

| Channel Type ID | Item Type    | Description  |
|-----------------|------------------------|--------------|
| event | Trigger | Triggers when a button is pressed. |
| transmit | String | Used to transmit IR commands to LIRC. |