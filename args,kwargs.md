# *args and **kwargs
## *args
It collects an arbitrary number of positional arguments into a tuple. It is used when you don't know beforehand how many arguments will be passed to a function.
## **kwargs 
It collects an arbitrary number of keyword arguments (name-value pairs) into a dictionary. It is used when you don't know which keyword arguments might be passed to a function.

# Note:
1) `*args` should be put before `**kwargs` in function arguments. Also while inputting while calling function, args arguments should be put before kwargs ones
2) Variable names should be put before args and kwargs in function as well as while calling.
3) The inputs of type '123' or 20 or true, etc are part of `*args`, while those of type data='10' or flag=True type are part of `**kwargs`.
4) args give output (10,'20',False) while kwargs give output {name='Anirudh', age=18, student=True}, a dictionary
5) `IMPORTANT:` `*args` can be replaced by `*some_args` name and `**kwargs` can be replaced by `**some_kwargs` name.
6) It is advised to put possible args and kwargs in docstring. 
# Examples
```python
def sum_all(*args): # a tuple is inputted
    """Sums all positional arguments."""
    return sum(args) 

def print_all(**kwargs): # a dictionary is inputted
    """Prints all keyword arguments and their values."""
    for key, value in kwargs.items():
        print(f"{key}: {value}")

class FlexibleDataContainer:
    def __init__(self, *args, **kwargs):
        self.data = {"args": args, "kwargs": kwargs}

    def display_data(self):
        print("Positional arguments:", self.data["args"]) 
        print("Keyword arguments:", self.data["kwargs"])

def flexible_decorator(func): # This function is flexible_function, automatically called using @
    def wrapper(*args, **kwargs):
        print("Hi all")
        result = func(*args, **kwargs) 
        print("Bye all! ")
        return result
    return wrapper

@flexible_decorator
def flexible_function(*args, **kwargs):
    print("Positional arguments:", args)
    print("Keyword arguments:", kwargs)

# Example usage
print(sum_all(1, 2, 3, 4, 5)) 
print_all(name="Anirudh", age=18, city="Bengaluru")

data_container = FlexibleDataContainer(10, 20, name="Ani", skill="Python") 
data_container.display_data()

flexible_function(1, 2, 3, name="AnirudhG07", job="Developer")
```
# OUTPUT
```bash
15
name: Anirudh
age: 18
city: Bengaluru
Positional arguments: (10, 20)
Keyword arguments: {'name': 'Ani', 'skill': 'Python'}
Hi all
Positional arguments: (1, 2, 3)
Keyword arguments: {'name': 'AnirudhG07', 'job': 'Developer'}
Bye all!
```
# More examples
## Pipeline and AI
Most AI's pipeline from HuggingFace has 
```python
# USING Codegen-2B-mono model from HuggingFace
from transformers import transformers

def generate_code(prompt):
    completion = pipeline( prompt, temperature=1, max_length=512, num_return_sequences=1)
    . . .
    return code
```
Here `*args` and `**kwargs` are used as we do not know how many of these the user will give. Of course some are compulsary like prompt.
In such cases the developer May set it as pipeline(prompt, *args, **kwargs) in source code. Here we do not have args, but kwargs are present.

## Log_info
```python
def log_info(*messages, **options):
    """
    Logs information to various outputs with optional configurations.

    Args:
        *messages: An arbitrary number of messages to log.
        **options: Keyword arguments for logging configuration:
            - `output`: Where to log messages (e.g., console, file)
            - `level`: Severity level of the log message (e.g., info, warning, error)

    Returns:
        None
    """
    default_output = "console"
    default_level = "info"

    output = options.get("output", default_output)
    level = options.get("level", default_level)

    for message in messages:
        print(f"[{level}] {message}")  # customize logging based on output and level

log_info("Starting application", "Initialization complete!", output="file", level="warning")
```
# OUTPUT
```bash
[warning] Starting application
[warning] Initialization complete!
```
