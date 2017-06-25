# Custom Protocols

*Note: this is advanced usage, and most readers will not need such functionality.*

You can change the behavior of Mach9's protocol by specifying a custom
protocol, which should be a subclass
of
[asyncio.protocol](https://docs.python.org/3/library/asyncio-protocol.html#protocol-classes).
This protocol can then be passed as the keyword argument `protocol` to the `mach9.run` method.

The constructor of the custom protocol class receives the following keyword
arguments from Mach9.

- `loop`: an `asyncio`-compatible event loop.
- `connections`: a `set` to store protocol objects. When Mach9 receives
  `SIGINT` or `SIGTERM`, it executes `protocol.close_if_idle` for all protocol
  objects stored in this set.
- `signal`: a `mach9.server.Signal` object with the `stopped` attribute. When
  Mach9 receives `SIGINT` or `SIGTERM`, `signal.stopped` is assigned `True`.
- `request_handler`: a coroutine that takes a `mach9.request.Request` object
  and a `response` callback as arguments.
- `error_handler`: a `mach9.exceptions.Handler` which is called when exceptions
  are raised.
- `request_timeout`: the number of seconds before a request times out.
- `request_max_size`: an integer specifying the maximum size of a request, in bytes.

## Example

An error occurs in the default protocol if a handler function does not return
an `HTTPResponse` object.

By overriding the `write_response` protocol method, if a handler returns a
string it will be converted to an `HTTPResponse object`.

```python
from mach9 import Mach9
from mach9.server import HttpProtocol
from mach9.response import text

app = Mach9(__name__)


class CustomHttpProtocol(HttpProtocol):

    def __init__(self, *, loop, request_handler, error_handler,
                 signal, connections, request_timeout, request_max_size):
        super().__init__(
            loop=loop, request_handler=request_handler,
            error_handler=error_handler, signal=signal,
            connections=connections, request_timeout=request_timeout,
            request_max_size=request_max_size)

    def write_response(self, response):
        if isinstance(response, str):
            response = text(response)
        self.transport.write(
            response.output(self.request.version)
        )
        self.transport.close()


@app.route('/')
async def string(request):
    return 'string'


@app.route('/1')
async def response(request):
    return text('response')

app.run(host='0.0.0.0', port=8000, protocol=CustomHttpProtocol)
```
