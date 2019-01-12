## Prerequisites

- vagrant 2.2.3
- agrant-disksize plugin
- virtualbox 5.2.22

## Simple Flask application that outputs hostname and server hits.

python3 /opt/flask/initdb.py && python3 /opt/flask/app.py

Endpoints:
  - /add_user : Adds a user to a mysql database based on the current user and host the application is running.
  - /users : List all users from database.
