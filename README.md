# tensorflow-test with GPU

Scripts and procedure to test a GPU node as slave of a MESOS cluster.

Keras example used: MNIST digit classification

https://victorzhou.com/blog/keras-neural-network-tutorial/

## JSON to submit to CHRONOS

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
## Bash script to run on the container
gpu_keras.sh

https://gist.github.com/alexcos78/054ab798e98a87ee425b74bee66905c5/raw/295e99ff6f09e046091f00850362c141f60493d1/gpu_keras.sh


# Run job on CHONOS
```
# curl -L -H 'Content-Type: application/json' -X POST -d @keras-test.json <MESOS_HOSTNAME>:4400/v1/scheduler/iso8601
```

## Stdout
```
...
RUNNING
(60000, 784)
(60000,)
Epoch 1/5
1875/1875 [==============================] - 12s 4ms/step - loss: 0.3699 - accuracy: 0.8885
Epoch 2/5
1875/1875 [==============================] - 8s 4ms/step - loss: 0.1994 - accuracy: 0.9397
Epoch 3/5
1875/1875 [==============================] - 8s 4ms/step - loss: 0.1524 - accuracy: 0.9537
Epoch 4/5
1875/1875 [==============================] - 8s 4ms/step - loss: 0.1290 - accuracy: 0.9602
Epoch 5/5
1875/1875 [==============================] - 9s 5ms/step - loss: 0.1103 - accuracy: 0.9653
```

## Stderr
```
...
loning into 'tensorflow-test'...
2021-09-29 15:10:10.019081: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:10.660614: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:10.665266: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:10.671115: I tensorflow/core/platform/cpu_feature_guard.cc:142] This TensorFlow binary is optimized with oneAPI Deep Neural Network Library (oneDNN) to use the following CPU instructions in performance-critical operations:  AVX2 AVX512F FMA
To enable them in other operations, rebuild TensorFlow with the appropriate compiler flags.
2021-09-29 15:10:10.673359: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:10.677101: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:10.681032: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:14.809045: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:14.812862: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:14.816208: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:937] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2021-09-29 15:10:14.819592: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1510] Created device /job:localhost/replica:0/task:0/device:GPU:0 with 30998 MB memory:  -> device: 0, name: Tesla V100-SXM2-32GB, pci bus id: 0000:00:05.0, compute capability: 7.0
2021-09-29 15:10:16.750449: W tensorflow/core/framework/cpu_allocator_impl.cc:80] Allocation of 188160000 exceeds 10% of free system memory.
2021-09-29 15:10:17.106776: W tensorflow/core/framework/cpu_allocator_impl.cc:80] Allocation of 188160000 exceeds 10% of free system memory.
2021-09-29 15:10:17.457221: I tensorflow/compiler/mlir/mlir_graph_optimization_pass.cc:185] None of the MLIR Optimization Passes are enabled (registered 2)

real    1m4.870s
user    0m59.153s
sys     0m14.672s
I0929 15:11:04.474429  2971 executor.cpp:1041] Command exited with status 0 (pid: 2974)
I0929 15:11:05.479077  2973 process.cpp:935] Stopped the socket accept loop
```
