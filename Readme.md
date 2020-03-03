# EZProxy service file for systemd start/stop
## ezproxy.service

My installation of EZProxy is in the directory /usr/local/ezproxy/ and I link the latest ezproxy version like this: 
ezproxy -> ezproxy.6_5_2.bin

I am using Ubuntu Linux 18 LTS.

### Installation:

1. Edit this file to /usr/local/ezproxy/ezproxy.service 
2. ln -s /usr/local/ezproxy/ezproxy.service /lib/systemd/system/ezproxy.service 

3. systemctl daemon-reload 
4. systemctl enable ezproxy.service
5. systemctl start ezproxy.service  

or stop

1. systemctl stop ezproxy.service

or status:

1. systemctl status ezproxy.service

When changing booting procedure (maybe from init.d --> systemd), don't forget to to disable the othere one...

Here is my ezproxy.service file:


```
[Unit]
Description=EZProxy
Documentation=https://help.oclc.org/Library_Management/EZproxy
PartOf=Network.target
After=Network.target nss-lookup.target local-fs.target

[Service]
Type=simple
WorkingDirectory=/usr/local/ezproxy
ExecStartPre=/sbin/sysctl -w net.ipv4.ip_local_port_range="10000 65535"
ExecStartPre=/sbin/sysctl -w net.ipv4.tcp_tw_reuse=1
ExecStartPre=/sbin/sysctl -w net.ipv4.tcp_fin_timeout=15
ExecStart=/usr/local/ezproxy/ezproxy start
ExecStop=/usr/local/ezproxy/ezproxy stop
ExecReload=/usr/local/ezproxy/ezproxy restart
KillMode=none
Restart=on-failure
RestartSec=3
LimitNOFILE=60000
LimitNPROC=infinity
LimitSTACK=infinity

[Install]
WantedBy=multi-user.target
Alias=ezproxy.service

# Installation:
#
# Edit this file to /usr/local/ezproxy/ezproxy.service 
# ln -s /usr/local/ezproxy/ezproxy.service /lib/systemd/system/ezproxy.service 
#
# systemctl daemon-reload 
# systemctl enable ezproxy.service
# systemctl start ezproxy.service  
```


