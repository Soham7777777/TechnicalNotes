<h1 style="margin-bottom:0;">Marshmallow</h1><small>used for Schema Validation of request, loading and dumping of objects</small> <hr>

## Schema:

- It can be used **as base class** for defining new schema, It can define new schema from **dict** at runtime as well. 

## Dumping:

- **Schema.dump()** will return python object (dict/list) and **Schema.dumps()** will return JSON-encoded string.
- We can specify subset of fields to dump.

## Loading:

- **Schema.load()** will validate and deserialize the input dict and by default returns **dict** of `field_name : deserialized_value` or raises **ValidationError** with **dict** of validation errors. **Schema.loads()** will desrialize JSON-encoded string.

- ``` python
  from pprint import pprint
  
  user_data = {
      "created_at": "2014-08-11T05:26:03.869245",
      "email": "ken@yahoo.com",
      "name": "Ken",
  }
  schema = UserSchema()
  result = schema.load(user_data)
  pprint(result)
  # {'name': 'Ken',
  #  'email': 'ken@yahoo.com',
  #  'created_at': datetime.datetime(2014, 8, 11, 5, 26, 3, 869245)}
  ```

- **the `created_at` string was deserialized into `datetime.datetime` object.**

- To change the default behaviour of deserializing which is a dict of `field_name : deserialized_value`, we can use `@post_load` decorator which will deserialize in a custom way.

- ``` python
  from marshmallow import Schema, fields, post_load
  
  
  class UserSchema(Schema):
      name = fields.Str()
      email = fields.Email()
      created_at = fields.DateTime()
  
      @post_load
      def make_user(self, data, **kwargs):
          return User(**data)
  
  
  user_data = {"name": "Ronnie", "email": "ronnie@stones.com"}
  schema = UserSchema()
  result = schema.load(user_data)
  print(result)  # => <User(name='Ronnie')>
  ```

- **Now, `Schema.load()` returns instance of `User`.**

- When loading the data, if **ValidationError** occurres then `ValidationError.messages` will return **dict** of `field_name : error_message`, the fields that were correctly desrialized will be available in `ValidationError.valid_data`. For collection, the format will be like this: `index: ValidationError_dict` for example:

- ```json
  {
  	1: {'email': ['Not a valid email address.']},
      3: {'name': ['Missing data for required field.']}
  }
  ```

- A validator is a function or method that accepts a single argument, the value to validate. If validation fails, the callable should raise a `ValidationError` with a useful error message or return `False` (for a generic error message). Validators can be chanied togather with by creating iterable of validators.

- **Validation occurs on deserialization(loading) but not on serialization(dumping).**

- We can mark a field that must present by passing `required=True`.

- We can skip `required` validation by passing iterables of `field_name` to `partial` attribute when loading or pass `True` to ignore missing fields, for example:

- ```  python
  class UserSchema(Schema):
      name = fields.String(required=True)
      age = fields.Integer(required=True)
  
  
  result = UserSchema().load({"age": 42}, partial=("name",))
  # OR UserSchema(partial=('name',)).load({'age': 42})
  print(result)  # => {'age': 42} 
  # That means its ok if name field is not present.
  
  result = UserSchema().load({"age": 42}, partial=True)
  # ignore all missing fields.
  ```

- We can specify `load_default` and `dump_default`.

- By default, `ValidationError` will be raised if unkown field is encountered on loading. We can change this behaviour with 3 options: `RAISE`(default), `EXCLUDE`, `INCLUDE`.

- We can validate without deserializing.

- We can specify a field to either be dump_only(read only) or load_only(write only). **dump_only fields are considered unkown at loading time, thus if `unknown` set to `INCLUDE` then it will be loaded without validation.**

  - The field_name can be specified for loading and dumping through `data_key` argument. 