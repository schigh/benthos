---
title: hdfs
type: output
---

```yaml
hdfs:
  directory: ""
  hosts:
  - localhost:9000
  max_in_flight: 1
  path: ${!count:files}-${!timestamp_unix_nano}.txt
  user: benthos_hdfs
```

Sends message parts as files to a HDFS directory. Each file is written
with the path specified with the 'path' field, in order to have a different path
for each object you should use function interpolations described
[here](/docs/configuration/interpolation#functions). When sending batched messages the
interpolations are performed per message part.

This output benefits from sending multiple messages in flight in parallel for
improved performance. You can tune the max number of in flight messages with the
field `max_in_flight`.

