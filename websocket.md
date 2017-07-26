# Websocket

Here is an example.

```python
import asyncio
from mach9 import Mach9
from mach9.response import websocket

app = Mach9()

@app.get('/foo', websocket=True)
async def test(request):

    async def streaming(channel):
	# get data from websocket
        message = await request.stream.get()
        # send data
        await channel.send(b'foo')
        await channel.send(b'')
        await channel.send(b'bar')
    return websocket(streaming)

if __name__ == '__main__':
    app.run(host='127.0.0.1', port=8000)
```
