# A Simple HTTP Server to act as  http webhook for prometheus

A nodejs server running on port 8080 wwhich writes body received in HTTP to console.log.

This was used to write alerts on console.log for later use.

## Build and run in openshift

```
oc project monitoring
oc new-build  openshift/nodejs~https://github.com/mamoru1112/prometheus-file-webhook.git --name=file-webhook
oc new-app file-webhook
```

## Configure alertmanager 

Use this config map for alertmanager 


```
global:
  resolve_timeout: 5m

route:
  group_by: ['alertname']
  group_wait: 10s
  group_interval: 10s
  repeat_interval: 1h
  receiver: 'web.hook'
receivers:
- name: 'web.hook'
  webhook_configs:
  - url: 'http://file-webhook.monitoring.svc.cluster.local:8080/'
  
```

  
