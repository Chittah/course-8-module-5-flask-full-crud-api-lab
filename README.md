# Module Lab: Building Full CRUD RESTful APIs with Flask

## Learning Goals

- Implement RESTful API endpoints using Flask.
- Handle HTTP POST, PATCH, and DELETE methods to manage resource data.
- Accept and process JSON input using `request.get_json()`.
- Simulate persistent data using in-memory Python objects.
- Follow RESTful route conventions and return structured JSON responses.

## Introduction

In this lab, you will build a **Full CRUD API** to manage a list of events. The API will allow users to:

- Create new events using `POST`
- Update existing events using `PATCH`
- Delete events using `DELETE`

You’ll simulate database-like behavior with in-memory Python class objects and respond to all client requests with properly formatted JSON and appropriate status codes.

This lab reinforces essential backend development skills including route design, data mutation, error handling, and RESTful conventions.

## Setup Instructions

### Fork and Clone the Repository

1. Go to the provided GitHub repository link.
2. Fork the repository to your GitHub account.
3. Clone the forked repository to your local machine:

```bash
git clone <repo-url>
cd course-8-module-5-flask-full-crud-api-lab
```

### Install Dependencies

Ensure Python is installed:

```bash
python --version
```

Install Flask and dependencies using pipenv:

```bash
pipenv install
pipenv shell
```

Or with pip:

```bash
pip install flask
```

## Tasks

### Task 1: Define the Problem

You’re building a basic event management API. It should:

- Accept event creation via `POST /events`
- Allow updating event titles via `PATCH /events/<id>`
- Delete events using `DELETE /events/<id>`
- Respond with structured JSON and appropriate HTTP status codes

---

### Task 2: Determine the Design

The Flask API should be structured as follows:

- Use `@app.route()` with correct HTTP method decorators
- Accept input using `request.get_json()`
- Represent data using a custom `Event` class
- Store events in an in-memory list
- Use `jsonify()` for consistent JSON responses

---

### Task 3: Develop the Code

Create `app.py` and start with the following structure:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

# Event class
class Event:
    def __init__(self, id, title):
        self.id = id
        self.title = title

    def to_dict(self):
        return {"id": self.id, "title": self.title}

# In-memory data store
events = [
    Event(1, "Tech Meetup"),
    Event(2, "Python Workshop")
]

# TODO: POST /events - Create a new event from JSON input
# TODO: PATCH /events/<id> - Update the title of an event
# TODO: DELETE /events/<id> - Remove an event from the list

if __name__ == "__main__":
    app.run(debug=True)
```

---

### Task 4: Test the API

Start the Flask development server:

```bash
python app.py
```

Test your endpoints using Postman or curl:

- `POST http://localhost:5000/events`
  - Body: `{ "title": "Hackathon" }`
- `PATCH http://localhost:5000/events/1`
  - Body: `{ "title": "Hackathon 2025" }`
- `DELETE http://localhost:5000/events/2`

---

## Best Practices

- Use RESTful nouns in routes (e.g., `/events`)
- Validate incoming JSON and handle missing keys gracefully
- Use helper functions to reduce code repetition
- Return:
  - `201 Created` for successful POST
  - `200 OK` or `204 No Content` for PATCH and DELETE
  - `404 Not Found` if a resource doesn't exist
- Include inline comments to explain logic

---

## Considerations

**1. Input Validation**
- Ensure the `title` field is provided.
- Return a `400 Bad Request` if missing.

**2. Event Not Found**
- Return `404 Not Found` with a clear message when the event ID doesn't exist.

**3. Reusable Logic**
- Consider writing a helper function to look up events by ID.

**4. Scalability**
- While using a single file works here, separate concerns into modules as your API grows.

---

## Conclusion

After completing this lab, you will:

✅ Know how to handle incoming JSON with Flask  
✅ Build routes that implement full CRUD behavior  
✅ Simulate persistent resource changes in memory  
✅ Return proper HTTP status codes and structured responses  

This is a critical step in your backend developer journey. Next up: persistent databases!

########
# Building a Full CRUD RESTful API with Flask

## 1. Introduction

This project involved building a simple **Event Management REST API** using Flask. The API allows users to create, view, update, and delete events.

The application uses an **in-memory Python list** to simulate data persistence instead of a database. Each event contains an ID and a title.

The API implements the following operations:

| HTTP Method | Endpoint       | Purpose                   |
| ----------- | -------------- | ------------------------- |
| GET         | `/`            | Display a welcome message |
| GET         | `/events`      | Retrieve all events       |
| POST        | `/events`      | Create a new event        |
| PATCH       | `/events/<id>` | Update an event title     |
| DELETE      | `/events/<id>` | Delete an event           |

The API returns structured JSON responses using Flask's `jsonify()` function and uses appropriate HTTP status codes.

---

# 2. Step 1: Create a Feature Branch

Before writing any application code, a new Git feature branch was created.

The terminal was opened in VS Code and the following command was executed:

```bash
git checkout -b feature-crud-api
```

This created and switched to a new branch called:

```text
feature-crud-api
```

The branch was verified using:

```bash
git branch
```

The output showed:

```text
* feature-crud-api
  main
```

The asterisk indicates that the application was being developed on the `feature-crud-api` branch.

Using a feature branch keeps the new functionality separate from the main branch until it has been tested and is ready to be merged.

---

# 3. Step 2: Create the Flask Application

A new file called:

```text
app.py
```

was created in the project directory.

The Flask framework was imported using:

```python
from flask import Flask, jsonify, request
```

The Flask application was initialized using:

```python
app = Flask(__name__)
```

### Purpose of the imports

* `Flask` creates the web application.
* `jsonify()` converts Python data into JSON responses.
* `request` allows the application to access data sent by clients.

---

# 4. Creating the Event Model

An `Event` class was created to represent individual events.

```python
class Event:
    def __init__(self, id, title):
        self.id = id
        self.title = title

    def to_dict(self):
        return {"id": self.id, "title": self.title}
```

Each Event object contains:

* `id` — a unique identifier for the event.
* `title` — the name of the event.

The `to_dict()` method converts an Event object into a Python dictionary. This makes it possible to return the event as JSON using `jsonify()`.

---

# 5. Simulated Data Persistence

Since the project does not use a database, an in-memory list was used to store the events.

```python
events = [
    Event(1, "Tech Meetup"),
    Event(2, "Python Workshop")
]
```

Initially, the API contains two events:

| ID | Title           |
| -: | --------------- |
|  1 | Tech Meetup     |
|  2 | Python Workshop |

Because the data is stored in memory, it will be lost when the Flask application is stopped.

---

# 6. GET `/` — Welcome Endpoint

The root endpoint was created using the following route:

```python
@app.route("/")
def home():
    return jsonify({"message": "Welcome to the Event Management API"})
```

The endpoint can be accessed using:

```text
GET http://127.0.0.1:5000/
```

The API returns:

```json
{
  "message": "Welcome to the Event Management API"
}
```

This confirmed that the Flask application was running correctly.

---

# 7. GET `/events` — Retrieve Events

The GET endpoint was created to retrieve all events:

```python
@app.route("/events", methods=["GET"])
def get_events():
    return jsonify([event.to_dict() for event in events])
```

The endpoint is:

```text
GET http://127.0.0.1:5000/events
```

The initial response was:

```json
[
  {
    "id": 1,
    "title": "Tech Meetup"
  },
  {
    "id": 2,
    "title": "Python Workshop"
  }
]
```

This fulfilled the requirement that the API return an array containing the event data.

---

# 8. POST `/events` — Create a New Event

The POST endpoint was implemented to allow users to create new events.

```python
@app.route("/events", methods=["POST"])
def create_event():
    data = request.get_json()

    if not data or "title" not in data:
        return jsonify({"error": "Title is required"}), 400

    new_id = max([event.id for event in events], default=0) + 1

    new_event = Event(new_id, data["title"])
    events.append(new_event)

    return jsonify(new_event.to_dict()), 201
```

### Reading JSON input

The request body is read using:

```python
data = request.get_json()
```

The API expects a JSON body containing a title.

For example:

```json
{
  "title": "Flask Conference"
}
```

A new event is created and added to the `events` list.

The ID is automatically generated using:

```python
new_id = max([event.id for event in events], default=0) + 1
```

Therefore, the new event received ID `3`.

### POST request

The following request was sent:

```text
POST http://127.0.0.1:5000/events
```

with the JSON body:

```json
{
  "title": "Flask Conference"
}
```

The API returned:

```json
{
  "id": 3,
  "title": "Flask Conference"
}
```

The HTTP status was:

```text
201 Created
```

This confirmed that the POST functionality was working correctly.

---

# 9. Input Validation

The API validates the POST request to make sure that a title has been provided.

The following validation was implemented:

```python
if not data or "title" not in data:
    return jsonify({"error": "Title is required"}), 400
```

If a user sends an empty JSON object:

```json
{}
```

the API responds with:

```json
{
  "error": "Title is required"
}
```

and returns:

```text
400 Bad Request
```

This prevents invalid events from being added to the application.

---

# 10. PATCH `/events/<id>` — Update an Event

The PATCH endpoint allows an existing event's title to be changed.

```python
@app.route("/events/<int:id>", methods=["PATCH"])
def update_event(id):
    event = next((event for event in events if event.id == id), None)

    if event is None:
        return jsonify({"error": "Event not found"}), 404

    data = request.get_json()

    if not data or "title" not in data:
        return jsonify({"error": "Title is required"}), 400

    event.title = data["title"]

    return jsonify(event.to_dict()), 200
```

For example, event 1 initially had the title:

```text
Tech Meetup
```

A PATCH request was sent to:

```text
PATCH http://127.0.0.1:5000/events/1
```

with:

```json
{
  "title": "Advanced Tech Meetup"
}
```

The API returned:

```json
{
  "id": 1,
  "title": "Advanced Tech Meetup"
}
```

with a:

```text
200 OK
```

response.

The GET `/events` request was then used to confirm that the title had been updated.

---

# 11. Resource Not Found — 404 Error

The API also handles situations where a requested event does not exist.

For example:

```text
PATCH http://127.0.0.1:5000/events/99
```

If event 99 does not exist, the API returns:

```json
{
  "error": "Event not found"
}
```

with:

```text
404 Not Found
```

This provides a clear response to the client instead of allowing the application to fail unexpectedly.

---

# 12. DELETE `/events/<id>` — Delete an Event

The DELETE endpoint was implemented as follows:

```python
@app.route("/events/<int:id>", methods=["DELETE"])
def delete_event(id):
    event = next((event for event in events if event.id == id), None)

    if event is None:
        return jsonify({"error": "Event not found"}), 404

    events.remove(event)

    return jsonify({"message": "Event deleted successfully"}), 200
```

To test the endpoint, event 2 was selected.

The following request was sent:

```text
DELETE http://127.0.0.1:5000/events/2
```

The API returned:

```json
{
  "message": "Event deleted successfully"
}
```

with:

```text
200 OK
```

A subsequent GET request confirmed that event 2 had been removed from the list.

---

# 13. Testing the API Using Thunder Client

Thunder Client was used in VS Code to test the API endpoints.

Thunder Client makes it possible to send different HTTP requests such as:

* GET
* POST
* PATCH
* DELETE

and inspect the responses returned by the Flask application.

## GET Test

The following request was tested:

```text
GET http://127.0.0.1:5000/events
```

The API returned the event list successfully.

---

## POST Test

The method was changed to:

```text
POST
```

The URL was:

```text
http://127.0.0.1:5000/events
```

Under:

```text
Body → JSON
```

the following data was entered:

```json
{
  "title": "Flask Conference"
}
```

The response showed:

```json
{
  "id": 3,
  "title": "Flask Conference"
}
```

with:

```text
201 Created
```

This confirmed that a new event could be created.

---

## PATCH Test

The request method was changed to:

```text
PATCH
```

The URL was:

```text
http://127.0.0.1:5000/events/1
```

The JSON body was:

```json
{
  "title": "Advanced Tech Meetup"
}
```

The response was:

```json
{
  "id": 1,
  "title": "Advanced Tech Meetup"
}
```

with:

```text
200 OK
```

This confirmed that the event title could be updated.

---

## DELETE Test

The method was changed to:

```text
DELETE
```

The URL was:

```text
http://127.0.0.1:5000/events/2
```

No request body was required.

The API returned:

```json
{
  "message": "Event deleted successfully"
}
```

with:

```text
200 OK
```

The event was then confirmed to be absent from the list using GET `/events`.

---

# 14. Complete Application Code

The completed `app.py` file is:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)


# Simulated data
class Event:
    def __init__(self, id, title):
        self.id = id
        self.title = title

    def to_dict(self):
        return {"id": self.id, "title": self.title}


events = [
    Event(1, "Tech Meetup"),
    Event(2, "Python Workshop")
]


# GET / - Welcome message
@app.route("/")
def home():
    return jsonify({"message": "Welcome to the Event Management API"})


# GET /events - Get all events
@app.route("/events", methods=["GET"])
def get_events():
    return jsonify([event.to_dict() for event in events])


# POST /events - Create a new event
@app.route("/events", methods=["POST"])
def create_event():
    data = request.get_json()

    if not data or "title" not in data:
        return jsonify({"error": "Title is required"}), 400

    new_id = max([event.id for event in events], default=0) + 1

    new_event = Event(new_id, data["title"])
    events.append(new_event)

    return jsonify(new_event.to_dict()), 201


# PATCH /events/<id> - Update event title
@app.route("/events/<int:id>", methods=["PATCH"])
def update_event(id):
    event = next((event for event in events if event.id == id), None)

    if event is None:
        return jsonify({"error": "Event not found"}), 404

    data = request.get_json()

    if not data or "title" not in data:
        return jsonify({"error": "Title is required"}), 400

    event.title = data["title"]

    return jsonify(event.to_dict()), 200


# DELETE /events/<id> - Delete an event
@app.route("/events/<int:id>", methods=["DELETE"])
def delete_event(id):
    event = next((event for event in events if event.id == id), None)

    if event is None:
        return jsonify({"error": "Event not found"}), 404

    events.remove(event)

    return jsonify({"message": "Event deleted successfully"}), 200


if __name__ == "__main__":
    app.run(debug=True)
```

---

# 15. Step 3: Testing Results

The API was tested using different HTTP methods.

| Test             | Endpoint            | Result                   | Status |
| ---------------- | ------------------- | ------------------------ | ------ |
| Welcome          | GET `/`             | Welcome message returned | 200    |
| Retrieve events  | GET `/events`       | Event array returned     | 200    |
| Create event     | POST `/events`      | New event created        | 201    |
| Missing title    | POST `/events`      | Error message returned   | 400    |
| Update event     | PATCH `/events/1`   | Title updated            | 200    |
| Invalid event    | PATCH `/events/99`  | Event not found          | 404    |
| Delete event     | DELETE `/events/2`  | Event deleted            | 200    |
| Invalid deletion | DELETE `/events/99` | Event not found          | 404    |

The tests demonstrated that the API correctly handles successful requests as well as invalid input and missing resources.

---

# 16. Step 4: Git Commit

After testing the application, the changes were checked using:

```bash
git status
```

The application file was then staged:

```bash
git add app.py
```

The changes were committed using:

```bash
git commit -m "Build Flask CRUD event API"
```

This created a Git commit containing the completed CRUD functionality.

---

# 17. Push the Feature Branch

The feature branch was pushed to the remote repository using:

```bash
git push -u origin feature-crud-api
```

This uploaded the `feature-crud-api` branch to the remote repository.

---

# 18. Merge the Feature

After the feature branch had been pushed and the API tested successfully, a Pull Request/Merge Request was created to merge:

```text
feature-crud-api
```

into:

```text
main
```

After review, the feature branch could be merged into the main branch.

---

# 19. Challenges and Solutions

## Challenge 1: Invalid Input

### Problem

Users may send an empty request or omit the required `title` field.

### Solution

The application checks the JSON request:

```python
if not data or "title" not in data:
    return jsonify({"error": "Title is required"}), 400
```

### Benefit

This prevents invalid events from being created and provides a clear error message to the client.

---

## Challenge 2: Finding Events by ID

### Problem

The application needs to search for an event before updating or deleting it.

### Solution

A generator expression is used:

```python
event = next((event for event in events if event.id == id), None)
```

### Benefit

This allows the application to determine whether the requested event exists before performing an operation.

---

## Challenge 3: Resource Not Found

### Problem

A client may request an event ID that does not exist.

### Solution

The application returns:

```python
return jsonify({"error": "Event not found"}), 404
```

### Benefit

The client receives a meaningful error and the API follows RESTful HTTP conventions.

---

## Challenge 4: Data Persistence

### Problem

The application does not currently use a database.

### Solution

An in-memory Python list was used:

```python
events = [
    Event(1, "Tech Meetup"),
    Event(2, "Python Workshop")
]
```

### Trade-off

This approach is simple and suitable for learning and testing, but the data is lost when the application stops.

For a production application, the in-memory list could later be replaced with a database such as PostgreSQL or SQLite.

---

# 20. Conclusion

The Event Management API was successfully implemented using Flask.

The application supports the main CRUD operations:

* **Create** — POST `/events`
* **Read** — GET `/events`
* **Update** — PATCH `/events/<id>`
* **Delete** — DELETE `/events/<id>`

The application also includes JSON request handling using `request.get_json()`, JSON responses using `jsonify()`, input validation, resource-not-found handling, and appropriate HTTP status codes.

The completed application was tested using Thunder Client, and the feature was prepared for version control using a dedicated Git feature branch.

The implementation provides a foundation that can later be extended with additional functionality and a permanent database for production use.
#######