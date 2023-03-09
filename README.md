# Install Nexus on OCP

* install new app

```
oc new-app sonatype/nexus3
```

* expose service

```
oc expose svc/nexus3
```

* edit route and add

```
[...]
spec:
  [...]
  tls:
    termination: edge
```

with command

```
oc patch route nexus3 -p "{\"spec\":{\"tls\":{\"termination\": \"edge\"}}}"
```

* set rediness probe

```
oc set probe deployment/nexus3 \
        --readiness \
        --failure-threshold 3 \
        --initial-delay-seconds 30 \
        --get-url=http://:8081
```

* set liveness probe

```
oc set probe deployment/nexus3 \
        --liveness \
        --failure-threshold 3 \
        --initial-delay-seconds 30 \
        -- echo ok
```

* create Maven-Settings config-map

```
oc create cm maven-settings --from-file=settings.xml=maven-settings.xml
```
