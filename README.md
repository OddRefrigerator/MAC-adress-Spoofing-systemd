# MAC-adress-Spoofing-systemd
Mac address spoofing systemd service units

[Unit]
Description=MAC Address Change %I
Wants=network-pre.target
Before=network-pre.target
BindsTo=sys-subsystem-net-devices-%i.device
After=sys-subsystem-net-devices-%i.device
[Service]
Type=oneshot
ExecStart=/usr/bin/ip link set dev %i address A8:1A:F1:43:F1:7B
ExecStart=/usr/bin/ip link set dev %i up
[Install]
WantedBy=multi-user.target

[Unit]
Description=macchanger on %I
Wants=network-pre.target
Before=network-pre.target
BindsTo=sys-subsystem-net-devices-%i.device
After=sys-subsystem-net-devices-%i.device
[Service]
ExecStart=/usr/bin/macchanger -e %I
Type=oneshot
[Install]
WantedBy=multi-user.target

[Unit]
Description=Set wlan0 MAC address
Before=dhcpcd.service
[Service]
Type=exec
ExecStart=/bin/macchanger -m xx:xx:xx:xx:xx:xx wlan0
[Install]
WantedBy=networking.service

