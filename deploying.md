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

## Running via command

If you like using command line arguments, you can launch a Mach9 server by
executing the module. For example, if you initialized Mach9 as `app` in a file
named `server.py`, you could run the server like so:

`python -m mach9 server.app --host=0.0.0.0 --port=1337 --workers=4`

With this way of running mach9, it is not necessary to invoke `app.run` in your
Python file. If you do, make sure you wrap it so that it only executes when
directly run by the interpreter.

```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=1337, workers=4)
```

## Asynchronous support
This is suitable if you *need* to share the mach9 process with other applications, in particular the `loop`.
However be advised that this method does not support using multiple processes, and is not the preferred way
to run the app in general.

Here is an incomplete example (please see `run_async.py` in examples for something more practical):

```python
server = app.create_server(host="0.0.0.0", port=8000)
loop = asyncio.get_event_loop()
task = asyncio.ensure_future(server)
loop.run_forever()
```
