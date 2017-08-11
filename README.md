These are the additional OCF startup scripts.

Just put them or what you need to use into /usr/lib/ocf/resource.d/heartbeat folder

haproxy resource:
-----------------

### Description

Manages haproxy daemon as an OCF resource in an High Availability setup.

### Usage

```
crm configure primitive haproxy ocf:heartbeat:haproxy params param1="value1" param2="value2" ... paramX="valueX" op monitor interval=1min
```

### Parameters

| Parameter | Description | Default value |
| --- | --- | --- |
| binpath | The HAProxy binary path. For example, "/usr/sbin/haproxy" | /usr/sbin/haproxy |
| conffile | The HAProxy daemon configuration file name with full path. For example, "/etc/haproxy/haproxy.cfg" | /etc/haproxy/haproxy.cfg |
| pidfile | The HAProxy daemon pid file with full path. For example, "/var/run/haproxy.pid" | /var/run/haproxy.pid" |
| statusurl | The HAProxy status URL to monitor. | |

### Notes

You need add a listen like this to the configuration file:
```
  listen health_check 127.0.0.1:6000
    mode health
```

Also you need to check if this listen works correctly (It should return: OK):
`#  wget -O- -q -L --no-proxy --bind-address=127.0.0.1 http://127.0.0.1:6000`

### Examples

```
crm configure primitive haproxy ocf:heartbeat:haproxy params conffile=/etc/haproxy.cfg op monitor interval=1min

crm configure primitive haproxy ocf:heartbeat:haproxy params binpath="/usr/local/sbin/haproxy" conffile=/etc/haproxy/haproxy.cfg op monitor interval=1min

crm configure primitive haproxy ocf:heartbeat:haproxy params pidfile=/var/run/ha/haproxy.pid op monitor interval=5min

crm configure primitive haproxy ocf:heartbeat:haproxy params statusurl="http://127.0.0.1:60000" op monitor interval=3seg
```
