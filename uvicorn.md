# Uvicorn

You put following code in server.py

```python
from mach9 import Mach9
from mach9.response import text

app = Mach9()


@app.route('/')
async def test(request):
    return text('Hello world!')

if __name__ == '__main__':
    app.run(host='127.0.0.1', port=8000)
```

Run server

```
uvicorn server:app
```
