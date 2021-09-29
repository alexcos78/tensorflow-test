# tensorflow-test

keras-test.json
```
{
  "name": "Keras-test",
  "command": "cd $MESOS_SANDBOX && /bin/bash gpu_keras.sh",
  "shell": true,
  "retries": 2,
  "cpus": 4,
  "disk": 256,
  "mem": 4096,
  "gpus": 1,
  "uris": [
    "https://gist.github.com/alexcos78/054ab798e98a87ee425b74bee66905c5/raw/295e99ff6f09e046091f00850362c141f60493d1/gpu_keras.sh"
  ],
  "environmentVariables": [],
  "arguments": [],
  "runAsUser": "root",
  "container": {
    "type": "MESOS",
    "image": "tensorflow/tensorflow:latest-gpu"
  },
  "schedule": "R/2018-03-05T23:00:00.000Z/PT24H"
}
```

Run job on CHONOS
```
# curl -L -H 'Content-Type: application/json' -X POST -d @keras-test.json <MESOS_HOSTNAME>:4400/v1/scheduler/iso8601
```
