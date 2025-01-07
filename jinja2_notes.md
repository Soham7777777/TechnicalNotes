# Jinja2 templating engine:

---

- Instances of `Environment` class are used to store configuration, global objects and load template.

  ```python
  from jinja2 import Environment, PackageLoader, select_autoescape
  
  env = Environment(
      loader=PackageLoader('temp_package'),
      autoescape=select_autoescape()
  )
  
  template = env.get_template('index.html')
  print(template.render(var='<h1> Hello again </h1>'))
  ```

- The autoescaping does its job when the template is rendered and variables contains html/css/js, if autoescaping is enabled than browser will show the `var` as it is written otherwise it will be interpreted by the browser and produces large text output.

- **Modifications on environments after the first template was loaded will lead to surprising effects and undefined behavior.**

  

  ## Designing Templates:

  ---

  - A Jinja template is simply a text file. Jinja can generate any text-based format (HTML, XML, CSV, LaTeX, etc.).  A Jinja template doesn’t need to have a specific extension: `.html`, `.xml`, or any other extension is just fine.

  - A template contains **variables** and/or **expressions**, which get replaced with values when a template is *rendered*; and **tags**, which control the logic of the template.  The template syntax is heavily inspired by Django and Python.

  - Template variables are defined by the context dictionary passed to the template.

  - If a variable or attribute does not exists: the default behavior is to evaluate to an empty string if printed or iterated over, and to fail for every other operation.

  - For the sake of convenience, `foo.bar` in Jinja does the following things on the Python layer:

    - check for an attribute called `bar` on `foo` (`getattr(foo, 'bar')`)
    - if there is not, check for an item `'bar'` in `foo` (`foo.__getitem__('bar')`)
    - if there is not, return an undefined object.

    `foo['bar']` works mostly the same with a small difference in sequence:

    - check for an item `'bar'` in `foo`. (`foo.__getitem__('bar')`)
    - if there is not, check for an attribute called `bar` on `foo`. (`getattr(foo, 'bar')`)
    - if there is not, return an undefined object.

    - This is important if an object has an item and attribute with the same name.  Additionally, the `attr()` filter only looks up attributes.

      

  - Variables can be modified by **filters**.  Filters are separated from the variable by a pipe symbol (`|`) and may have optional arguments in parentheses. For example `{{ name|striptags|title }}` removes all html tags and makes it title case.

  - Tests can be applied on variables which will return `True` or `False` based on conditon, for example:

    ```python
    {% if loop.index is divisibleby 3 %}
    {% if loop.index is divisibleby(3) %}
    ```

    ### Whitespace controll:

    - In the below template there is a single new line at the end of template (after </html>):
      ```jinja2
      <!DOCTYPE html>
      <html>
      <head>
          <title>{{ title }}</title>
      </head>
      <body>
          <h1>Hello, {{ username }}</h1>
      </body>
      </html>
      
      ```
    
    - jinja2 will strip that trailing new line, other white spaces in template are unchanged.
    
    - If the `trim_blocks` in `env` configuration is enabled than the first new line after tag (tag means: {% if ... %} or {% endif %} like that) will be removed. put `+` sign at the end of tag to manually disable it in template.
    
    - If the `lstrip_blocks` is enabled than white spaces will be removed which starts from the beggining of line and ends at the beggining of template tag. put `+` sign at the beggining of tag to manually disable it in template.
    
    - The `-` sign will strip all whitespaces before or after the tag by putting it at start or end of the block.
    
    - Just use tags with `-` sign like this to avoid unwanted whitespaces:
    
      ```jinja2
      {% block main -%}
      	Some default value
      {%- endblock %}
      ```
    
      ---
    
      

	### Template inheritance:

- `block` tags can be inside other blocks such as `if`, but they will always be executed regardless of if the `if` block is actually rendered.

- You can’t define multiple `{% block %}` tags with the same name in the same template.

- If you want to print a block multiple times user `self.block_name()`

  ```jinja
  <title>{% block title %}{% endblock %}</title>
  <h1>{{ self.title() }}</h1>
  {% block body %}{% endblock %}
  ```

- use `super()` to render content of parent block. chain multiple super like this `super.super()` to get desired block.

  ---

  

- Jinja allows you to put the name of the block after the end tag for better readability:

  ```jinja2
  {% block sidebar %}
      {% block inner_sidebar %}
          ...
      {% endblock inner_sidebar %}
  {% endblock sidebar %}
  ```

- Blocks can be nested for more complex layouts.  However, per default blocks may not access variables from outer scopes. you can explicitly specify that variables are available in a block by setting the block to “scoped” by adding the `scoped` modifier to a block declaration. When overriding a block, the `scoped` modifier does not have to be provided.

  ```jinja2
  {% for item in seq %}
      <li>{% block loop_item scoped %}{{ item }}{% endblock %}</li>
  {% endfor %}
  ```

- 
