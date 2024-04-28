# Basics of Flask

---

- minimal application:

  - ```python
    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route("/")
    def hello_world():
        return "<p>Hello, World!</p>"
    ```

- We then use the [`route()`](https://flask.palletsprojects.com/en/3.0.x/api/#flask.Flask.route) decorator to tell Flask what URL should trigger our function.

- run the command with `python3 -m flask --app file_name run` or we can write `app.run(debug=True)` in out main python code.

- As a shortcut, if the file is named `app.py` or `wsgi.py`, you don’t have to use `--app`. See [Command Line Interface](https://flask.palletsprojects.com/en/3.0.x/cli/) for more details.


## **Configurations**:
---

- `app = Flask(__name__, instance_relative_config=True)` : The instance_relative_config parameter is used to tell flask to load configuration files relative to instance folder instead of application root folder. We can load instance resources as described [here](https://flask.palletsprojects.com/en/3.0.x/config/#instance-folders:~:text=Flask.open_instance_resource().).


## **Handling Errors**:
---

- If an uncaught error occures while handling the requests then flask will show a default "500 Internal server Error" will be raised.

- There are HttpErrors provided by werkzeug and they are by default handled with simple error pages.

- We can either override those exceptions or subclass tham or create custom excpetions.

- For custom exceptions or for custom error responses, we need to register error handlers for that perticular error. 

- in a view function the default format is like this:
	- return HTML, status_code

- here the default status code is 200 used. read [this](https://flask.palletsprojects.com/en/3.0.x/quickstart/#about-responses) for more info.

## **Application lifecycle:**
---
```python
from flask import Flask

app = Flask(__name__)
app.config.from_mapping(
    SECRET_KEY="dev",
)
app.config.from_prefixed_env()

@app.route("/")
def index():
    return "Hello, World!"
```

### Setup phase:

- The above code is called setup phase for application, setup phase is the code that executes before app starts running.

- In flask, all setup must be done before running app, there must not be setup code remain to execute when app is running. The setup phase includes following things:

	- Adding routes, view functions, and other request handlers with @app.route, @app.errorhandler, @app.before_request, etc.

	- Registering blueprints.

	- Loading configuration with app.config.

	- Setting up the Jinja template environment with app.jinja_env.

	- Setting a session interface, instead of the default itsdangerous cookie.

	- Setting a JSON provider with app.json, instead of the default provider.

	- Creating and initializing Flask extensions.

### Serving phase:

- Flask implements the WSGI interface which is specified by PEP3333. It includes 2 parts of WSGI, 1.) the server/gateway and 2.) the application/framework

- Browser or other client makes HTTP request.

- WSGI server receives request.

- WSGI server converts HTTP data to WSGI environ dict.

- WSGI server calls WSGI application with the environ.

- Flask, the WSGI application, does all its internal processing to route the request to a view function, handle errors, etc.

- Flask translates View function return into WSGI response data, passes it to WSGI server.

- WSGI server creates and send an HTTP response.

- Client receives the HTTP response.

- Continue reading from : [Application Structure and Lifecycle](https://flask.palletsprojects.com/en/3.0.x/lifecycle/#middleware)

- After that read : [The Application Context](https://flask.palletsprojects.com/en/3.0.x/appcontext/)

- After that read : [The Request Context](https://flask.palletsprojects.com/en/3.0.x/reqcontext/)
