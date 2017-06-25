# Getting Started

Make sure you have both [pip](https://pip.pypa.io/en/stable/installing/) and at
least version 3.6 of Python before starting. Mach9 uses the new `async`/`await`
syntax, so earlier versions of python won't work.

1. Install Mach9: `python3 -m pip install mach9`
2. Create a file called `server.py` with the following code:

  ```python
  from mach9 import Mach9
  from mach9.response import text

  app = Mach9()


  @app.route("/")
  async def test(request):
      return text('Hello world!')

  app.run(host='127.0.0.1', port=8000, debug=True)
  ```
  
3. Run the server: `python3 server.py`
4. Open the address `http://0.0.0.0:8000` in your web browser. You should see
   the message *Hello world!*.

You now have a working Mach9 server!
