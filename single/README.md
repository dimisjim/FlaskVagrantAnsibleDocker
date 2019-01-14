## Prerequisites

- vagrant 2.2.3
- vagrant-disksize plugin
- vagrant-hostmanager plugin
- virtualbox 5.2.22
- ansible 2.0

## Simple Flask application that outputs hostname and server hits.

Endpoints:
  - /add_user : Adds a user to a mysql database based on the current user and host the application is running.
  - /users : List all users from database.
