# Deploying

Deploying Mach9 is made simple by the inbuilt webserver. After defining an
instance of `mach9.Mach9`, we can call the `run` method with the following
keyword arguments:

- `host` *(default `"127.0.0.1"`)*: Address to host the server on.
- `port` *(default `8000`)*: Port to host the server on.
- `debug` *(default `False`)*: Enables debug output (slows server).
- `ssl` *(default `None`)*: `SSLContext` for SSL encryption of worker(s).
- `sock` *(default `None`)*: Socket for the server to accept connections from.
- `workers` *(default `1`)*: Number of worker processes to spawn.

## Workers

By default, Mach9 listens in the main process using only one CPU core. To crank
up the juice, just specify the number of workers in the `run` arguments.

```python
app.run(host='0.0.0.0', port=1337, workers=4)
```

Mach9 will automatically spin up multiple processes and route traffic between
them. We recommend as many workers as you have available cores.
