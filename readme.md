# HTTP API Exercise

## Setup

### Make Sure You've Done The Initial Setup

Our [initial setup doc](../../../computer-setup/readme.md) has everything you need.

### Navigate To The Course Repo Directory

#### Navigate Via The File Browser First

A way friendly to those of us used to graphical interfaces is to use the file browser. Navigate using Windows' native filesystem window to:

api-class -> fundamentals-of-api-development -> exercises

You should see the "http-api" folder. *Don't double-click it*.  (Nothing  that bad will happen, you'll just have to go back if you do!)

#### Get There Via The Terminal

##### Use Anaconda's Terminal

Rather than Windows PowerShell, you'll want the **Anaconda PowerShell** for this one. It should be installed on your system.

It's like the regular PowerShell, but with some special Python-specific commands available to you.

It should be searchable on your system by pressing the Windows key and typing in "Anaconda".

##### Navigating To The Directory

- Type in `cd ` ("cd" with a space after it).
- Now drag the `http-api` folder from the windows file manager to the terminal. It should finish the command to be `cd ` and the path to your directory.
- Press enter.

You'll know you did it when you see the new path in your "prompt" (the area to the left of where you type in the terminal).

### Create And Activate Our Python Environment

This will give us the Flask code that enables us to run a server.

- Create the environment. Type `conda env create -f environment.yml` and press enter.
- Activate it. Type `conda activate api-class-flask-env` and press enter.

### Open The File In Your Editor

When opening files using Notepad's File->Open command, don't forget that `app.py` won't show up until you change "Text Files" to "All Files" in the drop-down menu.

## The Flask Server

### Running The Server

To run the server: `flask --debug run`

The `--debug` flag is optional but highly recommended during development—it will re-start the server when changes are made.

To run the server on a different port (if the default port 5000 is already in use): `flask --debug run --port [new port number]`

For example: `flask --debug run --port 5001`

### Hitting Up The Server

For GET requests, you can simply use your browser and hit up our endpoints. For example:

`http://127.0.0.1:5000/quotes`

For other HTTP verbs (such as POST), we recommend Postman, as it's the industry-standard tool. You can download it from app stores, command line repositories, or directly from [Postman's website](https://www.postman.com/downloads)

## Writing A Simple Route

With Flask, you declare which Python functions will be called from which endpoints. You do that with a Python decorator from Flask.

### Setup

Make sure this is near the top of your file:

```python
from flask import Flask, request

app = Flask(__name__)
```

You can skip importing `request` if only handling GET requests.

### A Simple Route

Let's write a simple route so that you can confirm everything works.

If you wanted the data "Hello, World!" to be returned when the user hit the endpoint `/hey`, you would write the following Python:

```python
@app.get("/hey")
def say_hi():
    return "Hello, World!"
```

The `@app.get` Python decorator is used by Flask to identify which endpoint (given as an argument to `@app.get`) will cause the function immediately following it to execute. Its return value is then sent back by the Flask server as a response to the client.

Now if you navigate to your local host  at http://127.0.0.1:5000/hey or http://localhost:5000/hey in Postman or your browser, you'll see the "Hello, World!" value returned. 

## Now Let's Write More Complex Routes!

### Exercise Layout

This exercise uses the same data and data files as the previous exercise.

### Specifications

See if you can create the following routes:

- `GET /characters` - returns all characters.
- `GET /quotes` - returns all quotes.
- `GET /characters/{id}` - returns the character with the matching `id` field. For example, `GET /characters/3`
- `GET /quotes/{id}` - returns the quote with the matching `id` field.
- `POST /characters` - adds a new character to the JSON file, returning the created character.
- `POST /quotes` - adds a new character to the JSON file, returning the created character.

### Tips

#### IDs

##### Getting The IDs

For getting a particular id, you can use a decorator like the following:

```python
@app.get("/characters/<string:id>")
def some_function_name(id):
```

Flask will pass in the part of the route after `/characters/` to the matching parameter in your function.

So a request to `GET /characters/4` will result in `"4"` passed in as the value for `id` in your function.

##### Converting IDs

One of the challenges in programming is dealing with data types. Data types, like strings, numbers, lists, and booleans (`true` and `false`) allow programming languages to perform functions specific to them—like grabbing a sub-string from a string (only a user's first name, for example) or performing a math operation, or checking if a condition is true or not (does the user have admin privileges?).

Most things typed in by a user are treated as strings by programming languages, including the ID part of the URL route. But to check the ID against the IDs in the data, which are numbers, you'll need to convert the ID passed into your route function.  You'll have to do **a bit** of research to see how, but you can either do it with a bit of Python code within your function, or with some changes to the Flask route in the decorator.
 
#### POST Bodies

The POST routes are tricky, but the data sent in the body of the request will be accessible on the imported `request` object. If you call `request.get_json()`, you will get the JSON that was sent in the request, converted to Python for you.

