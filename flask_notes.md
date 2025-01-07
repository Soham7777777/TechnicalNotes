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



## Notes on Routing:

---

- rule: The string that defines the **template of requested path**. For example (/users/, /home, /employee/\<int:id\>)

- endpoint: **The name given to a rule.** By default view function name is the endpoint name if `endpoint` argument isn't passed to `add_url_rule`(`route` is shortcut for `add_url_rule`). This name can be used with functions like `url_for` in order to point to a perticular rule. An endpoint cannot be registered on multiple view functions but multiple endpoints can be registered on a single view function.

- Thus, there is **one-to-many** relationship between view_function and endpoint.

- ```   python
  @app.get('/b', endpoint='bar')
  @app.get('/', endpoint='foo')
  def home():
      return 'hello world'
  
  @app.get('/f')
  def func():
      return redirect(url_for('foo'))
  ```
  
- When using `add_url_rule`, The `view_func` parameter doesn't have to be passed, just passing `endpoint` can also work if there is a function registered with that endpoint by `app.endpoint` decorator.

- ```python
  # First approach: adding rule and specifying its view_func
  def index():
      return 'Hello wrold'
  
  app.add_url_rule('/', view_func=index)
  
  # Second approach: adding rule and specifying endpoint
  app.add_url_rule('/', endpoint='foo')
  
  @app.endpoint('foo')
  def my_view():
      return 'Hello guys'
  # app.add_url_rule('/bar', endpoint='baaz') # if there is no view function registered with given endpoint the key error occures.
  
  # app.add_url_rule('/f') # error occures, either endpoint or view_func has to be passed.
  ```

- 3 ways to define rules for routing system:

  - `flask.Flask.route()` decorator
  - `flask.Flask.add_url_rule()` function
  - `flask.Flask.url_map`

- The idea is to keep each URL unique so the following rules apply:

  - If a rule ends with a slash and is requested without a slash by the user, the user is automatically redirected to the same page with a trailing slash attached.
  - If a rule does not end with a trailing slash and the user requests the page with a trailing slash, a 404 not found is raised.

- ```python
  @app.route('/users/', defaults={'page': 1})
  @app.route('/users/page/<int:page>')
  def show_users(page):
      pass
  ```

- In the above example multiple routes are registered for one view function. The first one does not have any variable part but it required to have `defaults` argument since function has `page` as parameter.

- **If a URL contains a default value, it will be redirected to its simpler form with a 301 redirect. In the above example, `/users/page/1` will be redirected to `/users/`.  If your route handles `GET` and `POST` requests, make sure the default route only handles `GET`, as redirects can’t preserve form data.**

- The `route` decorator is shortcut fo `add_url_rule`: `app.add_url_rule("/", view_func=index)` is equvivalent to register index function with `@app.route('/')`. 

- Build URL:  `url_for` function generates a URL to the given endpoint with the given values. values to use for the variable parts of the rule. Unknown keys are appended as query string arguments, like `?a=b&c=d`. `url_for` requires an active request or application context.

- ```python
  from flask import url_for
  
  @app.route('/')
  def index():
      return 'index'
  
  @app.route('/login')
  def login():
      return 'login'
  
  @app.route('/user/<username>')
  def profile(username):
      return f'{username}\'s profile'
  
  with app.test_request_context():
      print(url_for('index'))
      print(url_for('login'))
      print(url_for('login', next='/'))
      print(url_for('profile', username='John Doe'))
      
  # output:
  # /
  # /login
  # /login?next=/
  # /user/John%20Doe
  ```

  

## Static endpoint:

---

- Flask by default provides an `static` endpoint which is associated with the rule `/static/<path:filename>`. The default behaviour is to serve content from static directory with given path as `filename` parameter. so for exmple url path `/static/docs/index.html` will serve `docs/index.html` from inside static directory.
- The default endpoint name is directory name from the path `static_folder`. The  path for static content can also be directly changed using `static_url_path`.  These are given when instantiating the `Flask`.



## Blueprint:

---

- A `Blueprint` object works similarly to a `Flask` application object, but it is not actually an application.  Rather it is a *blueprint* of how to construct or extend an application. A blueprint in Flask is not a pluggable app because it is not actually an application – it’s a set of operations which can be registered on an application, even multiple times. 
- Blueprints instead provide separation at the Flask level, share application config, and can change an application object as necessary with being registered. The downside is that you cannot unregister a blueprint once an application was created without having to destroy the whole application object.
- The basic concept of blueprints is that they record operations to execute when registered on an application. 

- **Important read [this](https://flask.palletsprojects.com/en/2.3.x/patterns/urlprocessors/) page for understanding `url_defaults()` and `url_value_preprocessor()`**.
