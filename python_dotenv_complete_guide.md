# Python `python-dotenv` Reference: `.env` Files, Dictionaries, and Environment Variables

**Purpose:** Learn how to store configuration outside Python source code
and read it with `python-dotenv`.

This guide explains the difference between:

-   `load_dotenv()` --- loads values from a `.env` file into the process
    environment (`os.environ`).
-   `dotenv_values()` --- reads values from a `.env` file and returns a
    Python dictionary.
-   `os.getenv()` and `os.environ` --- read environment variables from
    the current process.
-   `set_key()` and `unset_key()` --- modify entries in a `.env` file.

> Use environment variables to keep configuration separate from code. Do
> not commit real passwords, API keys, database credentials, or private
> tokens to Git.

------------------------------------------------------------------------

# 1. What is `python-dotenv`?

`python-dotenv` is a third-party Python package. It reads key/value
settings from a text file, commonly named `.env`.

For example, a project may need:

-   A database host and port.
-   A database username and password.
-   An API key.
-   A development/production mode.
-   A service URL.
-   A debug setting.

Instead of writing these values directly into Python source code, place
them in a `.env` file and load them when the application starts.

Install it:

``` bash
python -m pip install python-dotenv
```

Check installation:

``` bash
python -m pip show python-dotenv
```

Import name:

``` python
from dotenv import load_dotenv, dotenv_values
```

Important distinction:

-   Package name for installation: `python-dotenv`
-   Import module name: `dotenv`

Do not try `import python-dotenv`; hyphens are not valid in Python
import names.

------------------------------------------------------------------------

# 2. What is a `.env` file?

A `.env` file is a plain-text configuration file containing entries in
`KEY=VALUE` form.

Example project structure:

``` text
my_project/
│
├── .env
├── .env.example
├── .gitignore
├── main.py
└── requirements.txt
```

Example `.env` file:

``` dotenv
APP_NAME=MyPythonApp
APP_ENV=development
DEBUG=true
PORT=8000

DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=shop_db
DATABASE_USER=app_user
DATABASE_PASSWORD=replace_with_a_secret

API_BASE_URL=https://api.example.com
API_KEY=replace_with_your_api_key
```

Rules and useful notes:

1.  One setting per line is the usual format.
2.  The left side is the key; the right side is its value.
3.  Keys are conventionally written in uppercase with underscores.
4.  Values are read as text; they are not automatically converted to
    Python booleans, integers, lists, or dictionaries.
5.  Quotes can be used around values containing special characters or
    leading/trailing whitespace.
6.  Comments can be written on their own lines using `#`.
7.  Do not put spaces around the key name. `PORT=8000` is the
    conventional form.
8.  Avoid placing secrets in source code or example files.

For example:

``` dotenv
# Application settings
APP_ENV=development

# A value containing spaces
APP_NAME="My Shopping API"

# A URL
API_URL=https://example.com/api
```

------------------------------------------------------------------------

# 3. `load_dotenv()` --- load values into the environment

This is the common approach when you want to read `.env` values through
`os.getenv()` or `os.environ`.

Example `.env`:

``` dotenv
APP_NAME=MyPythonApp
DEBUG=true
PORT=8000
```

Python:

``` python
import os
from dotenv import load_dotenv

load_dotenv()

app_name = os.getenv("APP_NAME")
debug = os.getenv("DEBUG")
port = os.getenv("PORT")

print(app_name)
print(debug)
print(port)
```

Output:

``` text
MyPythonApp
true
8000
```

Notice that `debug` and `port` are strings:

``` python
print(type(debug))  # str
print(type(port))   # str
```

`load_dotenv()` reads the file and makes its values available in the
current process environment.

## 3.1 What does `load_dotenv()` return?

It returns a boolean:

-   `True`: a `.env` file was found and loaded.
-   `False`: no `.env` file was found/loaded.

Example:

``` python
from dotenv import load_dotenv

loaded = load_dotenv()

if loaded:
    print("A .env file was loaded.")
else:
    print("No .env file was found.")
```

Do not confuse this return value with the dictionary of settings.
`load_dotenv()` does not return all environment variables as a
dictionary.

## 3.2 Important `load_dotenv()` arguments

Common arguments include:

  -----------------------------------------------------------------------
  Argument                            Meaning
  ----------------------------------- -----------------------------------
  `dotenv_path`                       Path to the `.env` file

  `stream`                            A text stream from which to read
                                      configuration

  `verbose`                           Whether to provide additional
                                      information about finding/loading
                                      the file

  `override`                          Whether file values should replace
                                      existing environment variables

  `interpolate`                       Whether variable references such as
                                      `${NAME}` should be expanded

  `encoding`                          Text encoding used to read the file
  -----------------------------------------------------------------------

### `dotenv_path`

Specify the exact file:

``` python
from dotenv import load_dotenv

load_dotenv(dotenv_path=".env")
```

Windows path example:

``` python
load_dotenv(dotenv_path=r"E:\MY_PROJECT\.env")
```

Using a `Path` object:

``` python
from pathlib import Path
from dotenv import load_dotenv

env_file = Path(__file__).resolve().parent / ".env"

load_dotenv(dotenv_path=env_file)
```

This is useful because it builds the path relative to the Python file
rather than depending on the terminal's current working directory.

### `override`

By default, existing environment variables are not replaced by values
from the `.env` file.

``` python
load_dotenv(override=False)
```

This is the default behaviour.

With:

``` python
load_dotenv(override=True)
```

values from the `.env` file replace matching variables already present
in the process environment.

Example:

``` python
import os
from dotenv import load_dotenv

os.environ["APP_ENV"] = "production"

load_dotenv(override=False)
print(os.getenv("APP_ENV"))
```

If `.env` contains `APP_ENV=development`, the existing `production`
value remains when `override=False`.

With `override=True`, the file's value can replace it.

**Best practice:** Understand where configuration comes from. In
production, deployment/platform environment variables are often intended
to take precedence over a local `.env` file.

### `interpolate`

Variable interpolation lets one setting refer to another using `${NAME}`
syntax.

`.env`:

``` dotenv
DOMAIN=example.com
API_URL=https://${DOMAIN}/api
```

Python:

``` python
from dotenv import load_dotenv
import os

load_dotenv()

print(os.getenv("API_URL"))
```

The resulting value can be:

``` text
https://example.com/api
```

Use `${NAME}` for clear interpolation. Do not assume every shell
expansion syntax is supported identically.

### `encoding`

The default is UTF-8 in normal usage. You can specify it explicitly:

``` python
load_dotenv(encoding="utf-8")
```

------------------------------------------------------------------------

# 4. `dotenv_values()` --- read the `.env` file into a dictionary

This is the dictionary approach.

Instead of placing the values into `os.environ`, `dotenv_values()`
parses the file and returns a Python dictionary.

Example `.env`:

``` dotenv
APP_NAME=MyPythonApp
DEBUG=true
PORT=8000
DATABASE_HOST=localhost
```

Python:

``` python
from dotenv import dotenv_values

config = dotenv_values(".env")

print(config)
print(config["APP_NAME"])
print(config["PORT"])
```

The returned object behaves like a dictionary mapping keys to values.

Example result:

``` python
{
    "APP_NAME": "MyPythonApp",
    "DEBUG": "true",
    "PORT": "8000",
    "DATABASE_HOST": "localhost"
}
```

## 4.1 Access dictionary values

``` python
from dotenv import dotenv_values

config = dotenv_values(".env")

app_name = config["APP_NAME"]
port = config["PORT"]

print(app_name)
print(port)
```

Using square brackets:

``` python
config["APP_NAME"]
```

This raises `KeyError` if the key does not exist.

Use `.get()` when a key may be missing:

``` python
debug = config.get("DEBUG")
```

Provide a default:

``` python
debug = config.get("DEBUG", "false")
```

## 4.2 Dictionary methods you can use

Because `dotenv_values()` returns a dictionary-like mapping, common
dictionary operations are available:

  Operation                    Meaning
  ---------------------------- ------------------------------------------------
  `config[key]`                Retrieve a value; raises `KeyError` if missing
  `config.get(key)`            Retrieve a value or return `None` if missing
  `config.get(key, default)`   Retrieve a value or use a default
  `config.keys()`              View all keys
  `config.values()`            View all values
  `config.items()`             View key/value pairs
  `key in config`              Check whether a key exists
  `len(config)`                Count entries
  `dict(config)`               Make a regular dictionary copy

Example:

``` python
from dotenv import dotenv_values

config = dotenv_values(".env")

for key, value in config.items():
    print(key, "=", value)

if "DATABASE_HOST" in config:
    print("Database host is configured.")
```

**Security note:** Do not print all configuration values in production
logs. Some entries may contain passwords, tokens, or private keys.

## 4.3 Does `dotenv_values()` modify `os.environ`?

No. It parses the file and returns values without loading them into the
process environment.

``` python
import os
from dotenv import dotenv_values

config = dotenv_values(".env")

print(config.get("APP_NAME"))
print(os.getenv("APP_NAME"))
```

The first line reads from the dictionary. The second reads from the
process environment. The second can be `None` if the variable was not
separately set or loaded.

------------------------------------------------------------------------

# 5. `load_dotenv()` versus `dotenv_values()`

This is one of the most important differences to understand.

  -----------------------------------------------------------------------
  Feature                 `load_dotenv()`         `dotenv_values()`
  ----------------------- ----------------------- -----------------------
  Reads `.env`            Yes                     Yes

  Loads values into       Yes                     No
  `os.environ`                                    

  Returns all settings as No                      Yes
  a dictionary                                    

  Return value            Boolean                 Dictionary-like mapping

  Can be used with        Yes                     Not unless values are
  `os.getenv()`                                   separately loaded
  afterwards                                      

  Useful for application  Yes                     Yes
  configuration                                   

  Modifies process        Yes                     No
  environment                                     
  -----------------------------------------------------------------------

## 5.1 Approach A: environment-variable approach

``` python
import os
from dotenv import load_dotenv

load_dotenv()

database_host = os.getenv("DATABASE_HOST")
database_port = os.getenv("DATABASE_PORT")
```

Use this when the rest of your application expects configuration through
environment variables.

## 5.2 Approach B: dictionary approach

``` python
from dotenv import dotenv_values

config = dotenv_values(".env")

database_host = config.get("DATABASE_HOST")
database_port = config.get("DATABASE_PORT")
```

Use this when you want to keep the parsed configuration in a dictionary
without modifying the process environment.

## 5.3 Combining both approaches

You can combine a `.env` file with existing environment variables:

``` python
import os
from dotenv import dotenv_values

file_config = dotenv_values(".env")

config = {
    **file_config,
    **os.environ
}
```

Dictionary unpacking works from left to right. If a key exists in both
dictionaries, the later value wins. Therefore, values already in
`os.environ` take precedence over `.env` values in this example.

This is a common configuration pattern:

1.  Read default/local settings from `.env`.
2.  Allow deployment environment variables to override them.
3.  Use the resulting dictionary as application configuration.

------------------------------------------------------------------------

# 6. Values are strings: convert them to the correct type

A `.env` file is text. These values are not automatically converted:

``` dotenv
DEBUG=true
PORT=8000
RATE=2.5
```

When read, they are strings.

## 6.1 Convert to integer

``` python
port = int(config.get("PORT", "8000"))
```

## 6.2 Convert to float

``` python
rate = float(config.get("RATE", "2.5"))
```

## 6.3 Convert a boolean safely

Do not use `bool("false")`: it returns `True` because the string is
non-empty.

Use an explicit conversion:

``` python
def to_bool(value):
    return str(value).strip().lower() in {"1", "true", "yes", "on"}


debug = to_bool(config.get("DEBUG", "false"))
```

If the value is `"false"`, `"no"`, `"off"`, or `"0"`, the function
returns `False`.

For stricter validation, reject unknown values instead of silently
treating them as false:

``` python
def parse_bool(value):
    normalized = str(value).strip().lower()

    if normalized in {"1", "true", "yes", "on"}:
        return True

    if normalized in {"0", "false", "no", "off"}:
        return False

    raise ValueError(f"Invalid boolean value: {value!r}")
```

## 6.4 Convert a comma-separated list

`.env`:

``` dotenv
ALLOWED_HOSTS=localhost,127.0.0.1,example.com
```

Python:

``` python
allowed_hosts = [
    host.strip()
    for host in config.get("ALLOWED_HOSTS", "").split(",")
    if host.strip()
]
```

------------------------------------------------------------------------

# 7. Variable expansion and references

A `.env` file can refer to another variable:

``` dotenv
DOMAIN=example.com
API_URL=https://${DOMAIN}/api
```

With interpolation enabled, `python-dotenv` can resolve `${DOMAIN}`.

You can also build derived configuration in Python:

``` python
from dotenv import dotenv_values

config = dotenv_values(".env")

domain = config.get("DOMAIN", "example.com")
api_url = f"https://{domain}/api"
```

Prefer explicit Python construction when the value has
application-specific logic.

------------------------------------------------------------------------

# 8. Finding the `.env` file and project paths

A common beginner problem is that Python cannot find `.env`.

The file lookup can depend on the current working directory and how the
script is run. Make the location explicit when reliability matters.

Recommended pattern:

``` python
from pathlib import Path
from dotenv import load_dotenv

BASE_DIR = Path(__file__).resolve().parent
ENV_FILE = BASE_DIR / ".env"

loaded = load_dotenv(dotenv_path=ENV_FILE)

print("Environment file:", ENV_FILE)
print("Loaded:", loaded)
```

Meaning:

-   `__file__`: path of the current Python file.
-   `Path(__file__)`: represents that path as a `Path` object.
-   `.resolve()`: produces an absolute resolved path.
-   `.parent`: gets the containing directory.
-   `/ ".env"`: joins the directory with the filename.

If your `.env` file is in the project root but your Python file is in a
subdirectory, adjust the path, for example:

``` python
ENV_FILE = Path(__file__).resolve().parent.parent / ".env"
```

------------------------------------------------------------------------

# 9. Other useful `python-dotenv` functions

Besides `load_dotenv()` and `dotenv_values()`, the package provides
functions for editing and generating `.env` files.

## 9.1 `set_key()`

Adds or updates a key in a `.env` file.

``` python
from dotenv import set_key

set_key(
    ".env",
    "APP_ENV",
    "development"
)
```

Common arguments:

  -----------------------------------------------------------------------
  Argument                            Meaning
  ----------------------------------- -----------------------------------
  `dotenv_path`                       File to modify

  `key_to_set`                        Key name

  `value_to_set`                      Value to store

  `quote_mode`                        Controls whether values are quoted;
                                      common values include `"always"`,
                                      `"auto"`, and `"never"`

  `export`                            Whether to write the `export`
                                      prefix
  -----------------------------------------------------------------------

Example:

``` python
set_key(
    ".env",
    "APP_NAME",
    "My Python Application",
    quote_mode="auto"
)
```

`set_key()` modifies the file. Do not use it casually on a production
secrets file without understanding file permissions, backups, and
deployment behaviour.

## 9.2 `unset_key()`

Removes a key from a `.env` file.

``` python
from dotenv import unset_key

unset_key(".env", "OLD_SETTING")
```

Common arguments include the path, key to remove, and export handling.

## 9.3 `get_key()`

Reads a particular key from a `.env` file.

``` python
from dotenv import get_key

value = get_key(".env", "APP_NAME")
print(value)
```

For reading many values, `dotenv_values()` is usually more convenient.

## 9.4 `find_dotenv()`

Finds a `.env` file by searching from a starting location according to
the package's search rules.

``` python
from dotenv import find_dotenv, load_dotenv

env_path = find_dotenv()

if env_path:
    load_dotenv(env_path)
    print("Found:", env_path)
else:
    print("No .env file found.")
```

Useful arguments include:

-   `filename`: filename to search for; normally `.env`.
-   `raise_error_if_not_found`: raise an error if it cannot be found.
-   `usecwd`: begin searching from the current working directory rather
    than the default caller-based starting location.

## 9.5 `get_cli_string()`

Builds a command-line string for using the package's CLI integration. It
is a specialised helper, not normally needed in everyday application
configuration. Avoid using generated command strings with untrusted
values.

## 9.6 `load_dotenv()` from a stream

You can load configuration from an in-memory text stream instead of a
filesystem path.

``` python
from io import StringIO
from dotenv import load_dotenv

content = """
APP_NAME=Demo
PORT=8000
"""

load_dotenv(stream=StringIO(content))
```

This is useful for controlled tests or when configuration text is
supplied by another trusted component.

------------------------------------------------------------------------

# 10. `.env`, `.env.example`, and `.gitignore`

A safe project commonly has three related files.

## `.env`

Contains the real local values. Do not commit it.

``` dotenv
DATABASE_USER=app_user
DATABASE_PASSWORD=real_local_secret
API_KEY=real_api_key
```

## `.env.example`

Contains names and safe placeholders. It can be committed so another
developer knows which settings are required.

``` dotenv
DATABASE_USER=your_database_user
DATABASE_PASSWORD=your_database_password
API_KEY=your_api_key
```

## `.gitignore`

Add:

``` gitignore
.env
.env.*
!.env.example
```

This ignores `.env` and other `.env.*` files but allows `.env.example`.

Check the ignore rules before committing. If a secret was already
committed, adding `.env` to `.gitignore` does not erase the secret from
Git history. Rotate/revoke exposed credentials and clean the repository
history as appropriate.

------------------------------------------------------------------------

# 11. Complete practical example

Project structure:

``` text
dotenv_practice/
│
├── .env
├── .env.example
├── .gitignore
└── main.py
```

`.env`:

``` dotenv
APP_NAME=NetworkingStudy
APP_ENV=development
DEBUG=true
PORT=8080
API_BASE_URL=https://api.example.com
```

`main.py`:

``` python
import os
from pathlib import Path

from dotenv import load_dotenv


# Build the .env path relative to this Python file.
BASE_DIR = Path(__file__).resolve().parent
ENV_FILE = BASE_DIR / ".env"

# Load file settings into the current process environment.
loaded = load_dotenv(dotenv_path=ENV_FILE)

if not loaded:
    raise RuntimeError(f"Could not load environment file: {ENV_FILE}")


def parse_bool(value):
    normalized = str(value).strip().lower()

    if normalized in {"1", "true", "yes", "on"}:
        return True

    if normalized in {"0", "false", "no", "off"}:
        return False

    raise ValueError(f"Invalid boolean setting: {value!r}")


# Read and convert values.
APP_NAME = os.getenv("APP_NAME", "MyApp")
APP_ENV = os.getenv("APP_ENV", "development")
DEBUG = parse_bool(os.getenv("DEBUG", "false"))
PORT = int(os.getenv("PORT", "8000"))
API_BASE_URL = os.getenv("API_BASE_URL")

print("Application:", APP_NAME)
print("Environment:", APP_ENV)
print("Debug:", DEBUG)
print("Port:", PORT)
print("API URL:", API_BASE_URL)
```

Run:

``` bash
python main.py
```

This example demonstrates:

1.  Finding a `.env` file using `pathlib`.
2.  Loading it with `load_dotenv()`.
3.  Reading variables using `os.getenv()`.
4.  Applying defaults.
5.  Converting strings into booleans and integers.
6.  Validating a boolean value.

Do not print passwords or API keys in a real application.

------------------------------------------------------------------------

# 12. Testing with `dotenv_values()`

The dictionary approach can make configuration testing straightforward
because it does not modify the process environment.

``` python
from dotenv import dotenv_values

config = dotenv_values(".env")

required_keys = {
    "APP_NAME",
    "DATABASE_HOST",
    "DATABASE_USER"
}

missing = [
    key for key in required_keys
    if not config.get(key)
]

if missing:
    raise RuntimeError(
        f"Missing configuration keys: {', '.join(sorted(missing))}"
    )
```

This checks that required settings exist and are not empty.

You can also pass a different file path to test development and test
configurations separately.

------------------------------------------------------------------------

# 13. Common mistakes and how to fix them

  -----------------------------------------------------------------------------------
  Mistake                  Why it happens          Correction
  ------------------------ ----------------------- ----------------------------------
  `import python-dotenv`   Package name is         Use
                           confused with import    `from dotenv import load_dotenv`
                           name                    

  `os.getenv()` returns    File was not loaded or  Check path and key name
  `None`                   key is missing          

  `.env` not found         Working directory       Use an explicit `Path`
                           differs from            
                           expectation             

  Port is a string         `.env` values are text  Convert with `int()`

  `"false"` becomes `True` Non-empty strings are   Parse boolean strings explicitly
                           truthy in Python        

  Existing value does not  `override=False` is the Use `override=True` only when
  change                   default                 intended

  Dictionary lookup raises Key is missing          Use `config.get()` or validate
  `KeyError`                                       required keys

  Secret appears in Git    `.env` was              Ignore it, rotate exposed secrets,
                           tracked/committed       and clean history

  `dotenv_values()` did    It returns a dictionary Use the dictionary or call
  not affect `os.getenv()` without changing the    `load_dotenv()`
                           environment             

  Values contain           File syntax or quoting  Inspect the parsed value and
  unexpected spaces/quotes is misunderstood        correct the `.env` line
  -----------------------------------------------------------------------------------

------------------------------------------------------------------------

# 14. Quick reference

``` python
# Install:
# python -m pip install python-dotenv

# Import:
from dotenv import (
    load_dotenv,
    dotenv_values,
    find_dotenv,
    get_key,
    set_key,
    unset_key
)

import os
from pathlib import Path

# Load into os.environ:
load_dotenv()

# Load a specific file:
load_dotenv(dotenv_path=".env")

# Override existing process variables:
load_dotenv(override=True)

# Read a .env file into a dictionary:
config = dotenv_values(".env")

# Read from the dictionary:
config.get("APP_NAME")
config["APP_NAME"]
config.keys()
config.values()
config.items()

# Read from the process environment:
os.getenv("APP_NAME")
os.environ["APP_NAME"]
os.environ.get("APP_NAME")

# Set a process environment variable:
os.environ["APP_ENV"] = "development"

# Find a .env file:
find_dotenv()

# Read one key from a .env file:
get_key(".env", "APP_NAME")

# Set/update a key in a .env file:
set_key(".env", "APP_NAME", "MyApplication")

# Remove a key from a .env file:
unset_key(".env", "OLD_SETTING")

# Build a reliable path:
BASE_DIR = Path(__file__).resolve().parent
ENV_FILE = BASE_DIR / ".env"
load_dotenv(ENV_FILE)
```

------------------------------------------------------------------------

# 15. What you should remember

The most important concept is the difference between a file, a
dictionary, and the process environment.

``` text
.env file
   |
   |-------------------------------|
   |                               |
   v                               v
load_dotenv()                  dotenv_values()
   |                               |
   v                               v
os.environ                    Python dictionary
   |                               |
   v                               v
os.getenv("KEY")              config.get("KEY")
```

-   `load_dotenv()` loads `.env` values into the current process
    environment.
-   `dotenv_values()` reads `.env` and returns a dictionary.
-   `os.getenv()` reads one environment variable and can return a
    default.
-   `os.environ` is a mapping of the current process's environment
    variables.
-   `set_key()` updates a setting in a `.env` file.
-   `unset_key()` removes a setting from a `.env` file.
-   `get_key()` reads one setting from a `.env` file.
-   `.env` values are text until your Python code converts them.
-   `.env` is configuration, not encryption or a secure vault.

**Recommended practice:** Use `load_dotenv()` plus `os.getenv()` for a
conventional application setup. Use `dotenv_values()` when you
specifically want configuration as a dictionary or want to combine file
settings with other configuration sources.
