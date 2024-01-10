# Others

### Git

https://git-scm.com/docs

```git
git status
git log
git add .
git diff -cached
git commit -m "Commit message"
git branch							# view current and local branches
git branch newBranch 				# create a new branch
git switch newBranch
git merge newBranch					# merge newBranch to the current branch
git branch -d branch				# ensures that the changes in the branch are already in the current branch
git branch -D branch				# delete
# remote
git remote add origin git@github.com:Hikipeko/p1-insta485-static.git
git fetch
git pull							# fetch + merge

```

**Init**

```shell
git init
git remote add origin git@github.com:Hikipeko/p2-insta485-serverside.git
git branch -M main
git push -u origin main
```

### Unzip

```shell
wget https://eecs485staff.github.io/p1-insta485-static/starter_files.tar.gz
tar -xvzf starter_files.tar.gz
mv starter_files/* .
rm -rf starter_files starter_files.tar.gz
tree
```



# Python

### Python Virtual Environment

**Create a virtual environment**

```shell
rm -rf env
python3 -m venv env
```

**Activate a virtual environment**

The `bin/activate` script adds `env/bin` to the `PATH` environment variable, making it the first place to look for commands.

```shell
source env/bin/activate
```

**Replicate a virtual environment**

A `requirements.txt` file lists the exact third party Python packages and their versions needed to replicate another virtual environment.

Install the package dependencies. 

```shell
pip install -r requirements.txt
```



### Python Debugging

**Install**

```shell
pip3 install pdbpp
```

Add `breakpoints()` to your file.

Sticky mode

Tab completion

**Commands**

`p` print variable

`n` execute next line of code

`q` quit

```shell
import pdb			# import pdb
breakpoint()		# create breakpoint
python3 -m pdb 		# start pdb from command line
where				# display stack trace and line number
next				# execute current line and move to the next
step 				# step into function at currentline
locals				# show values of local variables
sticky				# enter the function
```

**pytest**

```shell
pytest tests/		# test entire test suite
pytest tests/test_*.py
pytest tests/test_file.py::test_name
pytest -v tests/	# verbose
pytest -s tests/	# print
pytest --pdb tests	# enter pdb on failure
pytest --trace tests/	# break at the top of a test
```





### Python Package

https://docs.python.org/3/tutorial/modules.html#packages

https://flask.palletsprojects.com/en/2.2.x/patterns/packages/

平常我们习惯了使用 pip 来安装一些第三方模块，这个安装过程之所以简单，是因为模块开发者为我们默默地为我们做了所有繁杂的工作，而这个过程就是 `打包`。

**eggs & wheels**

Egg 格式是由 setuptools 在 2004 年引入，而 Wheel 格式是由 PEP427 在 2012 年定义。Wheel 的出现是为了替代 Egg，它的本质是一个zip包，其现在被认为是 Python 的二进制包的标准格式。

**setup.py编写**

#### Modules

A module is a file containing python definitions and statements.

A module can contain executable statements and funciton definitions. These statements are executed only the first time the module  name is encounted in an import statement.

```python
import fib
print(fib(4))

# make module a script that can be executed by python
if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))

# dir() is used to find out the names (variables, modules, functinos) defined by a module
import sys
dir(sys)
# submodue
import sound.effects.echo
sound.effects.echo.echofilter()
from sound.effects import echo()
echo.echofilter()
from sound.effects.echo import echofilter()
echofilter()

# Intra-package References
from ..filters import equalizer
```

#### Packages

A way of structuring Python's module namespace by using "dotted moduel names".

Useful to pack up the modules.

The `__init__.py` files are used to initialize modules, and are required to make Python treat directories as packages.



### Python App Setup

https://chriswarrick.com/blog/2014/09/15/python-apps-the-right-way-entry_points-and-scripts/

https://docs.python.org/3/distutils/setupscript.html

The `setup.py` file provided with the starter files describes how your package should be installed.

**File structure**

```shell
entry_point_project/
	my_project/
		__init__.py
		__main__.py
	setup.py
	...

python -m my_project		# execute
```

**1. Create a main file**

The `main()` function must not take any arguments.

**2. Adjust `setup.py` accordingly**

```python
from setuptools import setup
setup(
    name="my_project",
    version="0.1.0",
    packages=["my_project"],
    entry_points={
        "console_scripts": [
            "my_project = my_project.__main__:main"
        ]
    },
)
```

**Install python project locally**

`pip install -e .`

Install a project in editable mode (i.e. setuptools "develop mode") from a local project path or a VCS url.

The project is now in env/bin, and can be executed by entering the filename.



### Python click library

https://click.palletsprojects.com/en/8.1.x/options/

```python
import click

@click.command()
@click.option('--count', default=1, help='Number of greetings.')
@click.option('--name', prompt='Your name',
              help='The person to greet.')
def hello(count, name):
    """Simple program that greets NAME for a total of COUNT times."""
    for x in range(count):
        click.echo('Hello %s!' % name)

if __name__ == '__main__':
    hello()
```

A function becomes a Click command line tool by decorating it through [`click.command()`](https://click.palletsprojects.com/en/7.x/api/#click.command).

#### Parameters

Arguments can do less than options. The following features are only available for options:

- automatic prompting for missing input
- act as flags (boolean or otherwise)
- option values can be pulled from environment variables, arguments can not
- options are fully documented in the help page, arguments are not ([this is intentional](https://click.palletsprojects.com/en/8.1.x/documentation/#documenting-arguments) as arguments might be too specific to be automatically documented)

#### Options

##### Name

Options have a name that will be used as the Python argument name when calling the decorated function.

A name is chosen in the following order

1. If a name is not prefixed, it is used as the Python argument name and not treated as an option name on the command line.
2. If there is at least one name prefixed with two dashes, the first one given is used as the name.
3. The first name prefixed with one dash is used otherwise.

string > --string > -s

##### Basic value options

By default, options are not required, however to make an option required, simply pass in required=True as an argument to the decorator.f

```python
@click.option('--n', required=True, type=int)
# How to use a Python reserved word such as `from` as a parameter
@click.command()
@click.option('--from', '-f', 'from_')
@click.option('--to', '-t')
def reserved_param_name(from_, to):
    click.echo(f"from {from_} to {to}")
```

For single option boolean flags, the default remains hidden if the default value is False.

```python
@click.command()
@click.option('--n', default=1, show_default=True)
@click.option("--gr", is_flag=True, show_default=True, default=False, help="Greet the world.")
@click.option("--br", is_flag=True, show_default=True, default=True, help="Add a thematic break")
def dots(n, gr, br):
    if gr:
        click.echo('Hello world!')
    click.echo('.' * n)
    if br:
        click.echo('-' * n)
```

##### Multi value options

```python
@click.command()
@click.option('--pos', nargs=2, type=float)
def findme(pos):
    a, b = pos
    click.echo(f"{a} / {b}")
```

##### Tuples as multi value options

```python
@click.command()
@click.option('--item', type=(str, int))
def putitem(item):
    name, id = item
    click.echo(f"name={name} id={id}")
```



### Python Decorators

装饰器让你在一个函数的前后去执行代码。

原理：

https://www.runoob.com/w3cnote/python-func-decorators.html

蓝本规范:

```python
from functools import wraps

def decorator_name(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        if not can_run:
            return "Function will not run"
        return f(*args, **kwargs)
    return decorated
 
@decorator_name # equivalent to run func = decorator_name(func)
def func():
    return("Function is running")
 
can_run = True
print(func())
# Output: Function is running
 
can_run = False
print(func())
# Output: Function will not run
```

**Flask**

```python
from functools import wraps
 
def requires_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        auth = request.authorization
        if not auth or not check_auth(auth.username, auth.password):
            authenticate()
        return f(*args, **kwargs)
    return decorated
```



### Python logging

https://docs.python.org/3/library/logging.html

logger: expose the interface that application code directly uses.

Handler: send the log record to the appropriate destination.

Filter: determine which log records to output.

Formatter: specify layout of log records.

```python
logger = logging.getLogger(__name__)
```



### Python pathlib

https://docs.python.org/3/library/pathlib.html

```python
from pathlib import Path
# listing subdirectories
p = Path('.')
subdirs = [x for x in p.iterdir() if x.is_dir()]
# navigating inside a directory tree
q = p / 'init.d' / 'reboot'
q.resolve()
# open a file
with q.open() as f:
    l = f.readline()
# methods
Path.exists()
Path.is_dir()
Path.is_file()
Path.iterdir()
Path.mkdir(mode=0o777, parents=False, exist_ok=False)
Path.open(mode='r', buffering=-1, encoding=None, errors=None, newline=None)
Path.read_bytes()
Path.read_text(encoding=None, errors=None)
Path.touch(mode=0o666, exist_ok=True)
Path.write_bytes(data)						# open, write, and close
Path.write_text(data, encoding=None, errors=None, newline=None)
```





# Front-end

### Browser Developer Tools

Run a static page server.

```shell
python3 -m http.server 8000
```

**View source**

chrome `Ctrl + u`

**Developer tools**

`Ctrl + Shift + I`

**Console**

See errors generated while rendering a page.

Anything sent to `console.log()` in JavaScript will appear in the console.

**Elements AKA DOM**

Use the Elements tab to link rendered parts of a page to the underlying HTML and CSS. This is the Document Object Model (DOM).

**Cookies**

Windows/Linux `Control`+`Shift`+`C` –> `Application` –> `Cookies`

**Private browsing**

Private browsing mode creates a temporary session with separate cookies, browser history, etc. When you close all your private browsing tabs, the temporary information is removed.

**JavaScript debugger**

### HTML

Hyper Text Markup Language, the standard markup language for creating Web pages.

```html
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>

<h1>My First Heading</h1>
<p>My first paragraph.</p>

</body>
</html>
```

**Example Explained**

- The `<!DOCTYPE html>` declaration defines that this document is an HTML5 document
- The `<html>` element is the root element of an HTML page
- The `<head>` element contains meta information about the HTML page
- The `<title>` element specifies a title for the HTML page (which is shown in the browser's title bar or in the page's tab)
- The `<body>` element defines the document's body, and is a container for all the visible contents, such as headings, paragraphs, images, hyperlinks, tables, lists, etc.
- The `<h1>` element defines a large heading
- The `<p>` element defines a paragraph

**HTML element**

Defined by a start tag, some content, and an end tag.

`<br>` doesn't have end tag, defines a line break.

**HTML headings**

`<h1>` to `<h6>`

**Links**

`<a href="https://www.w3schools.com">This is a link</a>`

**Image**

`<img src="https://static.zerochan.net/Matikane.Tannhauser.full.3297803.png" alt="W3Schools.com" width="104" height="142">`

**Attributes**

All HTML elements can have attributes, providing additional information about elements.

The required `alt` attribute for the `<img>` tag specifies an alternate text for an image, if the image for some reason cannot be displayed.

The `style` attribute is used to add styles to an element, such as color, font, size, and more.

`<p style="color:red;">This is a red paragraph.</p>`

You should always include the `lang` attribute inside the `<html>` tag, to declare the language of the Web page. This is meant to assist search engines and browsers.

`<html lang="en-US">`

The `title` attribute defines some extra information about an element.

The value of the title attribute will be displayed as a tooltip when you mouse over the element

`<p title="I'm a tooltip">This is a paragraph.</p>`

#### Heading

Search engines use the headings to index the structure and content of your web pages.

**Bigger heading**

`<h1 style="font-size:60px;">Heading 1</h1>`

#### Paragraphs

With HTML, you cannot change the display by adding extra spaces or extra lines in your HTML code.

The `<hr>` tag defines a thematic break in an HTML page, and is most often displayed as a horizontal rule.

The HTML `<pre>` element defines preformatted text.

The text inside a `<pre>` element is displayed in a fixed-width font (usually Courier), and it preserves both spaces and line breaks.

#### Styles

The HTML `style` attribute has the following syntax:

`<tagname style="property:value;">`

**Background color**

`<body style="background-color:rgb(229, 186, 143);">`

**Text color**

<p style="color:red;">This is a paragraph.</p>

**Fonts**

<p style="font-family:courier;">This is a paragraph.</p>

**Font Size**

<p style="font-size:90%;">This is a paragraph.</p>

**Text alignment**

<p style="text-align:center;">Centered paragraph.</p>

#### Text Formatting

Formatting elements were designed to display special types of text:

- `<b>` - Bold text
- `<strong>` - Important text
- `<i>` - Italic text
- `<em>` - Emphasized text
- `<mark>` - Marked text
- `<small>` - Smaller text
- `<del>` - Deleted text
- `<ins>` - Inserted text
- `<sub>` - Subscript text
- `<sup>` - Superscript text

<b>This text is bold</b>

<strong>This text is important!</strong>

<i>This text is italic</i>

<em>This text is emphasized</em>

<small>This is some smaller text.</small>

<p>Do not forget to buy <mark>milk</mark> today.</p>

<p>My favorite color is <del>blue</del> red.</p>

<p>My favorite color is <del>blue</del> <ins>red</ins>.</p>

<p>This is <sub>subscripted</sub> text.</p>

<p>This is <sup>superscripted</sup> text.</p>

**Quotations**

The HTML `<blockquote>` element defines a section that is quoted from another source.

<p>Here is a quote from WWF's website:</p><blockquote cite="http://www.worldwildlife.org/who/index.html">For 50 years, WWF has been protecting the future of nature.The world's leading conservation organization,WWF works in 100 countries and is supported by1.2 million members in the United States andclose to 5 million globally.</blockquote>

The HTML `<q>` tag defines a short quotation.

<p>WWF's goal is to: <q>Build a future where people live in harmony with nature.</q></p>

The HTML `<abbr>` tag defines an abbreviation or an acronym, like "HTML", "CSS", "Mr.", "Dr.", "ASAP", "ATM".

<p>The <abbr title="World Health Organization">WHO</abbr> was founded in 1948.</p>

The HTML `<address>` tag defines the contact information for the author/owner of a document or an article.

<address>
Written by John Doe.<br>
Visit us at:<br>
Example.com<br>
Box 564, Disneyland<br>
USA
</address>

The HTML `<cite>` tag defines the title of a creative work.

<p><cite>The Scream</cite> by Edvard Munch. Painted in 1893.</p>

The HTML `<bdo>` tag is used to override the current text direction:

<bdo dir="rtl">This text will be written from right to left</bdo>

#### Comments

<!-- Write your comments here -->

#### Color

background, text, border

**values**

rgb(229,186,143)

#ff6347

hsl(9, 100%, 64%)

rgba(255, 99, 71, 0.5)

#### CSS

Cascading Style Sheets is used to format the layout of a webpage.

CSS can be added to HTML documents in 3 ways:

- **Inline** - by using the `style` attribute inside HTML elements
- **Internal** - by using a `<style>` element in the `<head>` section
- **External** - by using a `<link>` element to link to an external CSS file

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
<h1>This is a heading</h1>
<p>This is a paragraph.</p>
</body>
</html>
```

```css
body {
  background-color: powderblue;
}
h1 {
  color: blue;
}
p {
  color: red;
}
```

#### Links

The `target` attribute specifies where to open the linked document.

- `_self` - Default. Opens the document in the same window/tab as it was clicked
- `_blank` - Opens the document in a new window or tab
- `_parent` - Opens the document in the parent frame
- `_top` - Opens the document in the full body of the window

```html
<a href="https://www.w3schools.com/" target="_blank">Visit W3Schools!</a>
```

**Use image as link**

<a href="google.com"><img src="https://static.zerochan.net/Matikane.Tannhauser.full.3297803.png-" style="width:150px;"></a>

**Link to Email**

Use `mailto:` inside the `href` attribute to create a link that opens the user's email program (to let them send a new email).

<a href="mailto:someone@example.com">Send email</a>

**Button as a link**

<button onclick="document.location='default.asp'">HTML Tutorial</button>

#### HTML Forms

<form action="/action_page.php">
  <label for="fname">First name:</label><br>
  <input type="text" id="fname" name="fname" value="John"><br>
  <label for="lname">Last name:</label><br>
  <input type="text" id="lname" name="lname" value="Doe"><br><br>
  <input type="submit" value="Submit">
</form> 

If you click the "Submit" button, the form-data will be sent to a page called "/action_page.php".

**Input Element**

text, radio, checkbox, submit (a button for submitting the form), button

**Label**

The `for` attribute of the `<label>` tag should be equal to the `id` attribute of the `<input>` element to bind them together.

Notice that each input field must have a `name` attribute to be submitted.

If the `name` attribute is omitted, the value of the input field will not be sent at all.

**Action Attribute**

The `action` attribute defines the action to be performed when the form is submitted.

Usually, the form data is sent to a file on the server when the user clicks on the submit button.

In the example below, the form data is sent to a file called "action_page.php". This file contains a server-side script that handles the form data.



### CSS

Cascading Style Sheets

**Syntax**

h1 {color: blue; font-size: 12px;}

Selector {Property: Value}

**Selector**

id Selector: use the id attribute of an HTML element to select a specific element.

class Selector: selects HTML elements with a specific class attribute.

`<p class="center">`

`p.center`

Universal Selector *

Grouping Selector: h1, h2, p {}

**Priority**

Inline > External & internal style sheet

**Comment**

`/* ... */ `

**Background**

The `background-color` property specifies the background color of an element.

The `background-image` property specifies an image to use as the background of an element.

The `background-attachment` property specifies whether the background image should scroll or be fixed (will not scroll with the rest of the page):

```css
p {
  background-image: url("paper.gif");
}
body {
  background-image: url("gradient_bg.png");
  background-repeat: repeat-x;
  background-position: right top;
  background-attachment: fixed/scroll;
}
body {
  background: #ffffff url("img_tree.png") no-repeat right top;
}
```

#### Border

**Style**

The `border-style` property specifies what kind of border to display.

The following values are allowed:

- `dotted` - Defines a dotted border
- `dashed` - Defines a dashed border
- `solid` - Defines a solid border
- `double` - Defines a double border
- `groove` - Defines a 3D grooved border. The effect depends on the border-color value
- `ridge` - Defines a 3D ridged border. The effect depends on the border-color value
- `inset` - Defines a 3D inset border. The effect depends on the border-color value
- `outset` - Defines a 3D outset border. The effect depends on the border-color value
- `none` - Defines no border
- `hidden` - Defines a hidden border

**Width**

```css
p.three {
  border-style: solid;
  border-width: 25px 10px 4px 35px; /* 25px top, 10px right, 4px bottom and 35px left */
}
```

**Color**

The `border-color` property is used to set the color of the four borders.

**Sides**

```css
p {
  border-top-style: dotted;
  border-right-style: solid;
  border-bottom-style: dotted;
  border-left-style: solid;
}
p {
  border-style: dotted solid;
}
```

**Shorthand**

```css
p {
  border: 5px solid red;
}
p {
  border-left: 6px solid red;
}
```

**Rounded Borders**

```css
p {
  border: 2px solid red;
  border-radius: 5px;
}
```

#### Margins

The CSS `margin` properties are used to create space around elements, outside of any defined borders.

```css
p {
  margin: 25px 50px 75px 100px;
}
```

#### Two columns

```css
.column {
    float: left;
    width: 50%;
}

/* Clear floats after the columns */
.row:after {
    content: "";
    display: table;
    clear: both;
}
```

### Bootstrap

**What**

Bootstrap is a free front-end framework for faster and easier web development.

Bootstrap includes HTML and CSS based design templates for typography, forms, buttons, tables, navigation, modals, image carousels and many other, as well as optional JavaScript plugins

**Why**

Easy to use, responsive features, mobile-first approach, browser compatibility.

CDN: content delivery network

```html
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-A3rJD856KowSb7dwlZdYEkO39Gagi7vIsF0jrRAoQmDKKtQBHUuLZ9AsSv4jD4Xa" crossorigin="anonymous"></script>
</head>
```

**Container**





### Jinja

https://jinja.palletsprojects.com/en/3.1.x/templates/

#### Basics

The simplest way to configure Jinja to load templates for your application is to use [`PackageLoader`](https://jinja.palletsprojects.com/en/3.1.x/api/#jinja2.PackageLoader).

```jinja2
from jinja2 import Environment, PackageLoader, select_autoescape
env = Environment(
    loader=PackageLoader("yourapp"),
    autoescape=select_autoescape()
)
```

This will create a template environment with a loader that looks up templates in the `templates` folder inside the `yourapp` Python package.

There are a few kinds of delimiters. The default Jinja delimiters are configured as follows:

- `{% ... %}` for [Statements](https://jinja.palletsprojects.com/en/3.0.x/templates/#list-of-control-structures)
- `{{ ... }}` for [Expressions](https://jinja.palletsprojects.com/en/3.0.x/templates/#expressions) to print to the template output
- `{# ... #}` for [Comments](https://jinja.palletsprojects.com/en/3.0.x/templates/#comments) not included in the template output

#### Variables

The following lines do the same thing:

```
{{ foo.bar }}
{{ foo['bar'] }}
```

#### Template Inheritance

The most powerful part of Jinja is template inheritance. Template inheritance allows you to build a base “skeleton” template that contains all the common elements of your site and defines **blocks** that child templates can override.

```jinja2
<!DOCTYPE html>
<html lang="en">
<head>
    {% block head %}
    <link rel="stylesheet" href="style.css" />
    <title>{% block title %}{% endblock %} - My Webpage</title>
    {% endblock %}
</head>
<body>
    <div id="content">{% block content %}{% endblock %}</div>
    <div id="footer">
        {% block footer %}
        &copy; Copyright 2008 by <a href="http://domain.invalid/">you</a>.
        {% endblock %}
    </div>
</body>
</html>
```

In this example, the `{% block %}` tags define four blocks that child templates can fill in. All the block tag does is tell the template engine that a child template may override those placeholders in the template.

A child template might look like this:

```jinja2
{% extends "base.html" %}
{% block title %}Index{% endblock %}
{% block head %}
    {{ super() }}
    <style type="text/css">
        .important { color: #336699; }
    </style>
{% endblock %}
{% block content %}
    <h1>Index</h1>
    <p class="important">
      Welcome to my awesome homepage.
    </p>
{% endblock %}
```

The `{% extends %}` tag is the key here. It tells the template engine that this template “extends” another template. 

It’s possible to render the contents of the parent block by calling `super()`. This gives back the results of the parent block.

### REST API

https://www.liaoxuefeng.com/wiki/1022910821149312/1105000713418592#0

A REST API sends JSON data over HTTP.

REST就是一种设计API的模式。最常用的数据格式是JSON。由于JSON能直接被JavaScript读取，所以，以JSON格式编写的REST风格的API具有简单、易读、易用的特点。

编写API有什么好处呢？由于API就是把Web App的功能全部封装了，所以，通过API操作数据，可以极大地把前端和后端的代码隔离，使得后端代码易于测试，前端代码编写更简单。

**Characteristics**

- **Client-Server**: There should be a separation between the server that offers a service, and the client that consumes it.
- **Stateless**: Each request from a client must contain all the information required by the server to carry out the request. In other words, the server cannot store information provided by the client in one request and use it in another request.
- **Cacheable**: The server must indicate to the client if requests can be cached or not.
- **Layered System**: Communication between a client and a server should be standardized in such a way that allows intermediaries to respond to requests instead of the end server, without the client having to do anything different.
- **Uniform Interface**: The method of communication between a client and a server must be uniform.
- **Code on demand**: Servers can provide executable code or scripts for clients to execute in their context. This constraint is the only one that is optional.

#### REST API Tools

GitHub REST API

https://api.github.com/users/awdeorio

**curl**

`curl https://api.github.com/users/awdeorio`

**httpie**

`http https://api.github.com/users/awdeorio`

**Basic Access Authentication**

A method for an HTTP user agent to provide username and password when making a request.

`http -a awdeorio:password "http://localhost:8000/api/v1/posts/1/"`

**POST requests**

HTTPie implicitly uses JSON as the content type.

`http -a awdeorio:password POST "http://localhost:8000/api/v1/comments/?postid=3" text='Comments'`

**Sessions**

For the REST APIs that require a login and session cookies.

Login using HTTPie to fill out the login form and save the cookie in session.json.

```shell
http \
  --session=./session.json \
  --form POST \
  "http://localhost:8000/accounts/" \
  username=awdeorio \
  password=password \
  operation=login
```

Then access API with the session cookie.

```shell
http \
	--session=./session.json \
	"http://localhost:8000/api/v1/posts/1/"
```



### Testing

**React/JS Debugging**

https://eecs485staff.github.io/p3-insta485-clientside/setup_react_js_debugging.html

**End to End Testing**

https://eecs485staff.github.io/p3-insta485-clientside/setup_endtoend_testing.html



### JavaScript

JavaScript is a *loosely typed* and *dynamic* language. Variables in JavaScript are not directly associated with any particular value type, and any variable can be assigned (and re-assigned) values of all types:

```js
let foo = 42;    // foo is now a number
foo     = 'bar'; // foo is now a string
foo     = true;  // foo is now a boolean
```

#### Primitive Values

Immutable

Boolean: true / false

Null: null

Undefined: a variable that has not been assigned a value has the value `undefined`

Number: like double, 64-bit

BigInt: represent integers with arbitrary precision, created by appending `n` to the end of an integer or by calling the constructor.

```js
> const x = BigInt(Number.MAX_SAFE_INTEGER);
9007199254740991n
> x + 1n === x + 2n; // 9007199254740992n === 9007199254740993n
false
```

NaN NaN !== NaN

String

Symbol: built-in object whose constructor returns a `symbol` [primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)  that's guaranteed to be unique.

**Variable**

Declared using `let`, `const`, `var`

**`let`** allows you to declare block-level variables. The declared variable is available from the *block* it is enclosed in.

**`const`** allows you to declare variables whose values are never intended to change. The variable is available from the *block* it is declared in.

Only functions have a scope. So if a variable is defined using `var` in a compound statement (for example inside an `if` control structure), it will be visible to the entire function.

**&& ||**

The `&&` and `||` operators use short-circuit logic, which means whether they will execute their second operand is dependent on the first. 

```js
const name = o && o.getName();
const name = cachedName || (cachedName = getName());
const allowed = (age > 18) ? 'yes' : 'no';
```

#### Objects

In computer science, an object is a value in memory which is possibly referenced by an [identifier](https://developer.mozilla.org/en-US/docs/Glossary/Identifier).

**Properties**

In JavaScript, objects can be seen as a collection of properties (dictionary).

Property values can be values of any type, including other objects, which enables building complex data structures. Properties are identified using *key* values.

```js
obj.details.color; // orange
obj['details']['size']; // 12
```

#### Function

```js
function avg(...args) {
  let sum = 0;
  for (const item of args) {
    sum += item;
  }
  return sum / args.length;
}

avg(2, 3, 4, 5); // 3.5
```

**Anonymous function**

```js
let avg = function() {
  let sum = 0;
  for (const item of arguments) {
    sum += item;
  }
  return sum / arguments.length;
};
```

#### Custom objects

```js
function makePerson(first, last) {
  return {
    first: first,
    last: last,
    fullName() {
      return this.first + ' ' + this.last;
    },
    fullNameReversed() {
      return this.last + ', ' + this.first;
    }
  };
}

function Person(first, last) {
  this.first = first;
  this.last = last;
}
Person.prototype.fullName = function() {
  return this.first + ' ' + this.last;
};
Person.prototype.fullNameReversed = function() {
  return this.last + ', ' + this.first;
};
```

#### Closures

```js
function makeAdder(a) {
  return function(b) {
    return a + b;
  };
}
const add5 = makeAdder(5);
const add20 = makeAdder(20);
add5(6); // ?
add20(7); // ?
```



### React

https://reactjs.org/tutorial/tutorial.html

#### What is React?

React is a declarative, efficient, and flexible JavaScript library for building user interfaces. It lets you compose complex UIs from small and isolated pieces of code called “components”.

```js
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />
```

Here, ShoppingList is a **React component class**, or **React component type**. A component takes in parameters, called `props` (short for “properties”), and returns a hierarchy of views to display via the `render` method.

Most React developers use a special syntax called “JSX” which makes these structures easier to write.

**Immutability**

Immutability makes complex features much easier to implement.

Avoiding direct data mutation lets us keep previous versions of the game’s history intact, and reuse them later.

**Function components**

A simpler way to write components that only contain a `render` method and don’t have their own state. 

```js
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

#### Hello World

```react
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<h1>Hello, world!</h1>);
```

#### JSX

A syntax extension to JavaScript.

JSX comes with the full power of JavaScript. You can put *any* JavaScript expressions within braces inside JSX. Each React element is a JavaScript object that you can store in a variable or pass around in your program.

```react
const element = <h1>Hello, world!</h1>;
```

By default, React DOM [escapes](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that’s not explicitly written in your application. Everything is converted to a string before being rendered. This helps prevent [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks.

#### Rendering Elements

```html
<div id="root"></div>
```

We call this a “root” DOM node because everything inside it will be managed by React DOM.

Applications built with just React usually have a single root DOM node.

To render a React element, first pass the DOM element to [`ReactDOM.createRoot()`](https://reactjs.org/docs/react-dom-client.html#createroot), then pass the React element to `root.render()`:

```react
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
const element = <h1>Hello, world</h1>;
root.render(element);
```

**Updating the rendered elements**

React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object). Once you create an element, you can’t change its children or attributes. An element is like a single frame in a movie.

**React only updates what's necessary**

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

In our experience, thinking about how the UI should look at any given moment, rather than how to change it over time, eliminates a whole class of bugs.

#### Components and Props

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen.

```react
const element = <Welcome name="Sara" />;
```

When React sees an element representing a user-defined component, it passes JSX attributes and children to this component as a single object. We call this object “props”.

**All React components must act like pure functions with respect to their props.**

#### State and Lifecycle

State is similar to props, but it is private and fully controlled by the component.

**Converting a function to a Class**

1. Create an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), with the same name, that extends `React.Component`.
2. Add a single empty method to it called `render()`.
3. Move the body of the function into the `render()` method.
4. Replace `props` with `this.props` in the `render()` body.
5. Delete the remaining empty function declaration.

```react
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

```

We want to [set up a timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) whenever the `Clock` is rendered to the DOM for the first time. This is called “mounting” in React.

We also want to [clear that timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) whenever the DOM produced by the `Clock` is removed. This is called “unmounting” in React.

While `this.props` is set up by React itself and `this.state` has a special meaning, you are free to add additional fields to the class manually if you need to store something that doesn’t participate in the data flow (like a timer ID).

Thanks to the `setState()` call, React knows the state has changed, and calls the `render()` method again to learn what should be on the screen. 

**Using state correctly**

Use this.setState() to update state.

To fix it, use a second form of `setState()` that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

```react
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));

  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

The merging is shallow, so `this.setState({comments})` leaves `this.state.posts` intact, but completely replaces `this.state.comments`.

This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.

#### Handling Events

With JSX you pass a function as the event handler, rather than a string.

```react
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

When you define a component using an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), a common pattern is for an event handler to be a method on the class.

If calling `bind` annoys you, there are two ways you can get around this. You can use [public class fields syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields#public_instance_fields) to correctly bind callbacks:

```react
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  handleClick = () => {
    console.log('this is:', this);
  };
  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

**Passing arguments to event handlers**

```react
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

#### Conditional Rendering

```react
function WarningBanner(props) {
  if (!props.warn) {    return null;  }
  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```

#### Lists and Keys

```react
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>  <li key={number.toString()>{number}</li>);
<ul>{listItems}</ul>
```

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity.

It’s strongly recommended that you assign proper keys whenever you build dynamic lists.

Keys only make sense in the context of the surrounding array.

A good rule of thumb is that elements inside the `map()` call need keys.

#### Forms

We can combine the two by making the React state be the “single source of truth”. Then the React component that renders a form also controls what happens in that form on subsequent user input. An input form element whose value is controlled by React in this way is called a “controlled component”.

```react
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {    this.setState({value: event.target.value});  }
  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

#### Lifting State Up



**Passing data through props**

 Passing props is how information flows in React apps, from parents to children.

```javascript
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;  }
}

class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```

To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead.

#### Composition vs Inheritance

**Containment**

Some components don’t know their children ahead of time. This is especially common for components like `Sidebar` or `Dialog` that represent generic “boxes”.

We recommend that such components use the special `children` prop to pass children elements directly into their output:

```react
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}    
    </div>
  );
}
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

Anything inside the `<FancyBorder>` JSX tag gets passed into the `FancyBorder` component as a `children` prop.

While this is less common, sometimes you might need multiple “holes” in a component. In such cases you may come up with your own convention instead of using `children`:

```react
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}      </div>
      <div className="SplitPane-right">
        {props.right}      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />      }
      right={
        <Chat />      } />
  );
}
```

**Specialization**

```react
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}      
      </h1>
      <p className="Dialog-message">
        {props.message}      
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog      
        title="Welcome"      
        message="Thank you for visiting our spacecraft!" />  
  );
}
```

#### Thinking in React

1. Break the UI into a component hierarchy.
2. Build a static version in React.
3. Identify the minimal representation of UI state. (don't repeat yourself)
4. Identify where your state should live. (onw-way data flow down the component hierarchy)
5. Add inverse data flow



# Back-end



### Flask

https://flask.palletsprojects.com/en/2.2.x/quickstart/

**Hello world**

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

We use the [`route()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.Flask.route) decorator to tell Flask what URL should trigger our function.

The function returns the message we want to display in the user’s browser. The default content type is HTML, so HTML in the string will be rendered by the browser.

Before you can do that you need to tell your terminal the application to work with by exporting the `FLASK_APP` environment variable:

```bash
# for debug
export FLASK_ENV=development
export FLASK_APP=insta485
# This tells your operating system to listen on all public IPs.
flask run --host 0.0.0.0 --port 8000
```

**HTML escaping**

When returning HTML (the default response type in Flask), any user-provided values rendered in the output must be escaped to protect from injection attacks. HTML templates rendered with Jinja, introduced later, will do this automatically.

```python
from markupsafe import escape

@app.route("/<name>")
def hello(name):
    return f"Hello, {escape(name)}!"
```

**Routing**

Use the [`route()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.Flask.route) decorator to bind a function to a URL.

You can add variable sections to a URL by marking sections with `<variable_name>`. Your function then receives the `<variable_name>` as a keyword argument.

```python
from markupsafe import escape

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return f'User {escape(username)}'

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return f'Post {post_id}'

@app.route('/path/<path:subpath>')
def show_subpath(subpath):
    # show the subpath after /path/
    return f'Subpath {escape(subpath)}'
```

Types: string(default), int, float, path, uuid

**URL building**

To build a URL to a specific function, use the [`url_for()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.url_for) function.

The generated paths are always absolute, avoiding unexpected behavior of relative paths in browsers.

```python
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
   
/
/login
/login?next=/
/user/John%20Doe
```

**HTTP methods**

Web applications use different HTTP methods when accessing URLs. You should familiarize yourself with the HTTP methods as you work with Flask. By default, a route only answers to `GET` requests.

```python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```

**Static files**

To generate URLs for static files, use the special `'static'` endpoint name:

```
url_for('static', filename='style.css')
```

The file has to be stored on the filesystem as `static/style.css`.

**Rendering templates**

Flask configures the [Jinja2](https://palletsprojects.com/p/jinja/) template engine for you automatically.

To render a template you can use the [`render_template()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.render_template) method. All you have to do is provide the name of the template and the variables you want to pass to the template engine as keyword arguments. Here’s a simple example of how to render a template:

```python
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```

Flask will look for templates in the `templates` folder. So if your application is a module, this folder is next to that module, if it’s a package it’s actually inside your package.

Inside templates you also have access to the [`config`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.Flask.config), [`request`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.request), [`session`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.session) and [`g`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.g) [1](https://flask.palletsprojects.com/en/2.1.x/quickstart/#id3) objects as well as the [`url_for()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.url_for) and [`get_flashed_messages()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.get_flashed_messages) functions.

**Accessing request data**

For web applications it’s crucial to react to the data a client sends to the server. In Flask this information is provided by the global [`request`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.request) object.

```python
@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'],
                       request.form['password']):
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password'
    # the code below is executed if the request method
    # was GET or the credentials were invalid
    return render_template('login.html', error=error)
```

**File uploads**

You can handle uploaded files with Flask easily. Just make sure not to forget to set the `enctype="multipart/form-data"` attribute on your HTML form, otherwise the browser will not transmit your files at all.

```python
from flask import request

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/uploaded_file.txt')
    ...
```

**Redirects and Errors** 

To redirect a user to another endpoint, use the [`redirect()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.redirect) function; to abort a request early with an error code, use the [`abort()`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.abort) function:

```python
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()
```

**Sessions**

In addition to the request object there is also a second object called [`session`](https://flask.palletsprojects.com/en/2.1.x/api/#flask.session) which allows you to store information specific to a user from one request to the next.

```python
from flask import session

# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    if 'username' in session:
        return f'Logged in as {session["username"]}'
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form method="post">
            <p><input type=text name=username>
            <p><input type=submit value=Login>
        </form>
    '''

@app.route('/logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))
```

**Query string**
A query string is a part of a [uniform resource locator](https://en.wikipedia.org/wiki/Uniform_resource_locator) (URL) that assigns values to specified parameters.

```url
/comments/?target=/
```

**Interact with DB**

https://flask.palletsprojects.com/en/2.2.x/tutorial/database/



### Sqlite3

https://docs.python.org/3/library/sqlite3.html

Once a [`Connection`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection) has been established, create a [`Cursor`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor) object and call its [`execute()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.execute) method to perform SQL commands:

```python
cur = con.cursor()

# Create table
cur.execute('''CREATE TABLE stocks
               (date text, trans text, symbol text, qty real, price real)''')

# Insert a row of data
cur.execute("INSERT INTO stocks VALUES ('2006-01-05','BUY','RHAT',100,35.14)")

# Save (commit) the changes
con.commit()

# We can also close the connection if we are done with it.
# Just be sure any changes have been committed or they will be lost.
con.close()
```

SQL operations usually need to use values from Python variables. However, beware of using Python’s string operations to assemble queries, as they are vulnerable to SQL injection attacks.

Instead, use the DB-API’s parameter substitution. To insert a variable into a query string, use a placeholder in the string, and substitute the actual values into the query by providing them as a [`tuple`](https://docs.python.org/3/library/stdtypes.html#tuple) of values to the second argument of the cursor’s [`execute()`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.execute) method.

```python
import sqlite3

con = sqlite3.connect(":memory:")
cur = con.cursor()
cur.execute("create table lang (name, first_appeared)")

# This is the qmark style:
cur.execute("insert into lang values (?, ?)", ("C", 1972))

# The qmark style used with executemany():
lang_list = [
    ("Fortran", 1957),
    ("Python", 1991),
    ("Go", 2009),
]
cur.executemany("insert into lang values (?, ?)", lang_list)

# And this is the named style:
cur.execute("select * from lang where first_appeared=:year", {"year": 1972})
print(cur.fetchall())

con.close()
```

[Why do you need to create a cursor when querying a sqlite database?](https://stackoverflow.com/questions/6318126/why-do-you-need-to-create-a-cursor-when-querying-a-sqlite-database)

### fetch API

https://davidwalsh.name/fetch

https://developer.mozilla.org/en-US/docs/Web/API/fetch

**Basic `fetch` Usage**

```js
// url (required), options (optional)
fetch('https://davidwalsh.name/some/url', {
	method: 'get'
}).then(function(response) {
	
}).catch(function(err) {
	// Error :(
});

// Chaining for more "advanced" handling
fetch('https://davidwalsh.name/some/url').then(function(response) {
	return //...
}).then(function(returnedValue) {
	// ...
}).catch(function(err) {
	// Error :(
});
```

**Response**

The `fetch`'s `then` method is provided a `Response` instance but you can also manually create `Response` objects yourself -- another situation you may encounter when using service workers. With a `Response` you can configure:



### Promise API

https://davidwalsh.name/promises

https://www.liaoxuefeng.com/wiki/1022910821149312/1023024413276544

I promise I’ll do **__ when the previous task of __** is done.

A new Promise is created with the `new` keyword and the promise provides `resolve` and `reject` functions to the provided callback:

```js
var p = new Promise(function(resolve, reject) {
	// Do an async task async task and then...
	if(/* good condition */) {
		resolve('Success!');
	}
	else {
		reject('Failure!');
	}
});

p.then(function(result) { 
	/* do something with the result */
}).catch(function() {
	/* error :( */
}).finally(function() {
   /* executes regardless or success for failure */ 
});
```

可见Promise最大的好处是在异步执行的流程中，把执行代码和处理结果的代码清晰地分离了：

resolve & reject参数为输出内容

```js
new Promise(function (resolve, reject) {
    log('start new Promise...');
    var timeOut = Math.random() * 2;
    log('set timeout to: ' + timeOut + ' seconds.');
    setTimeout(function () {
        if (timeOut < 1) {
            log('call resolve()...');
            resolve('200 OK');
        }
        else {
            log('call reject()...');
            reject('timeout in ' + timeOut + ' seconds.');
        }
    }, timeOut * 1000);
}).then(function (r) {
    log('Done: ' + r);
}).catch(function (reason) {
    log('Failed: ' + reason);
});
```

要串行执行这样的异步任务，不用Promise需要写一层一层的嵌套代码。有了Promise，我们只需要简单地写：

```
job1.then(job2).then(job3).catch(handleError);
```

其中，`job1`、`job2`和`job3`都是Promise对象。





