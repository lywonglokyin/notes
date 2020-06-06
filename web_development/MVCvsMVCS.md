# MVC

## Model

Model is usually the independent part of the MVC model. Its main purpose is to access data. Usually, `View` would be an observer of `Model`.

## View

The appearance. HTML, CSS, ...

## Controller

Controls the flow of the requests.

A typical flow:
1) User send a request. Controller accepts the request, then ask Model for data.
2) Model ask database for data (e.g. MongoDB). Then returns it to controller.
3) Controller send the received data to View.
4) View would then handles presentation, for example, using dynamically rendered html, and sending back to controller.
5) Controller then return the presentation to users.

A working example:
1) A user send a request for list of cats. Controller accepts the request, and ask Model for list of cats.
2) Model perform query to database (e.g. `SELECT * FROM cats`), and return the results.
3) If the query is success, controller will pass the data to `View.cats`, else it calls `View.error`.
4) View renders the html (e.g. `<html>... <h1>Cat 1</h1>...`), and return to controller.
5) The controller returns the page to user.

---

# MVCS

## Service

An extra service layer is added for MVCS model.

My understanding is that it add an extra layer between Controller and Model, such that there is a more clear cut of usage of database (Service can held a bundle of Models of different databases).