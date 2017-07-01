# Streaming

## Request Streaming

Mach9 allows you to get request data by stream, as below.

```python
from mach9 import Mach9
from mach9.views import CompositionView
from mach9.views import HTTPMethodView
from mach9.views import stream as stream_decorator
from mach9.blueprints import Blueprint
from mach9.response import stream, text

bp = Blueprint('blueprint_request_stream')
app = Mach9('request_stream')


class SimpleView(HTTPMethodView):

    @stream_decorator
    async def post(self, request):
        result = ''
        while True:
            body_chunk = await request.stream.receive()
            if body_chunk['more_content'] is False:
                break
            result += body_chunk['content'].decode('utf-8')
        return text(result)


@app.post('/stream', stream=True)
async def post(self, request):
    result = ''
    while True:
        body_chunk = await request.stream.receive()
        if body_chunk['more_content'] is False:
            break
        result += body_chunk['content'].decode('utf-8')
    return text(result)


@bp.put('/bp_stream', stream=True)
async def put(self, request):
    result = ''
    while True:
        body_chunk = await request.stream.receive()
        if body_chunk['more_content'] is False:
            break
        result += body_chunk['content'].decode('utf-8')
    return text(result)


async def post_handler(request):
    result = ''
    while True:
        body_chunk = await request.stream.receive()
        if body_chunk['more_content'] is False:
            break
        result += body_chunk['content'].decode('utf-8')
    return text(result)

app.blueprint(bp)
app.add_route(SimpleView.as_view(), '/method_view')
view = CompositionView()
view.add(['POST'], post_handler, stream=True)
app.add_route(view, '/composition_view')


if __name__ == '__main__':
    app.run(host='127.0.0.1', port=8000)
```

## Response Streaming

Mach9 allows you to stream content to the client with the `stream` method.

```python
from mach9 import Mach9
from mach9.response import stream

app = Mach9('response_stream')


@app.post('/post/<id>', stream=True)
async def post(request, id):
    assert isinstance(request.stream, BodyChannel)

    async def streaming(channel):
        while True:
            body_chunk = await request.stream.receive()
            if body_chunk['more_content'] is False:
                break
            await channel.send(body_chunk['content'])
    return stream(streaming)


@app.get('/get')
async def get(request):
    async def streaming(channel):
        await channel.send(b'foo')
        await channel.send(b'')
        await channel.send(b'bar')
    return stream(streaming)
```
