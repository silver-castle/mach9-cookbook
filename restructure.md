# Restructrurable

Mach9 is restructrurable framework.  
Most Mach9's components are independent.  
You can replace Mach9's components.  
(You can replace most Mach9's components by putting them in constructor of Mach9.  
Following code is constructor of Mach9.)  

```python
class Mach9:

    def __init__(self, name=None, router=None, error_handler=None,
                 load_env=True, request_class=None, protocol=None,
                 serve=None, serve_multiple=None, log=None, netlog=None,
                 log_config=LOGGING, update_current_time=None,
                 get_current_time=None, composition_view=None,
                 body_channel_class=None, reply_channel_class=None)
```

You can use Mach9's components to making your framework.  
Mach9 has following components.  

* Blueprints
* Config
* Request
* Response
* Channel
* Protocol
* Router
* Server
* Signal
* Timer
* ErrorHandler
* Exceptions
* View
* Log

## Blueprints

Blueprints provides [Blueprints functions](blueprints.md).  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/blueprints.py).

## Config

Config provides [Config functions](config.md).  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/config.py).

## Request

Request provides [Request functions](request.md).  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/request.py).

## Response

Response provides [Response functions](response.md).  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/response.py).

## Channel

Channel is BodyChannel and ReplyChannel.  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/http.py).

## Protocol

Protocol should be a subclass of [asyncio.protocol](https://docs.python.org/3/library/asyncio-protocol.html#protocol-classes).  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/http.py).

## Router

Router provides [Routing functions](https://github.com/silver-castle/mach9-cookbook/blob/master/routing.md).   
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/router.py).

## Server

Server executes application.  
Server has `serve()` and `serve_multiple()`.  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/server.py).

## Signal

Signal saves server stopped or not.  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/signal.py).

## Timer

Timer provides time counter.  
Timer has `update_current_time()` and `get_current_time()`.  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/timer.py).

## ErrorHandler

Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/handlers.py).

## View

View provides [Class-Based Views functions](class_based_views.md).  
View has HTTPMethodView and CompositionView.  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/views.py).

## Exceptions

Exceptions provides [Exceptions functions](exceptions.md).  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/exceptions.py).

## Log

Log provides [Logging functions](logging.md).  
Implementation example is [here](https://github.com/silver-castle/mach9/blob/master/mach9/log.py).
