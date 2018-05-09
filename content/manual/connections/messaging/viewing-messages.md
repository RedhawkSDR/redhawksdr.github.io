---
title: "Viewing Messages"
weight: 30
---

Messages are events with their payload definition tied to structures in component properties. Viewing messages can be done with the same techniques that are used to view events.

To view events and messages sent to an EventChannel in a terminal window:

```bash
eventviewer <domain name> <event channel>
```

Help for the utility:

```bash
eventviewer --help
```

Example output:

```bash
eventviewer REDHAWK_DEV testchan
Receiving events. Press 'enter' key to exit
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
[{'id': 'foo', 'value': [{'id': 'some_string', 'value': 'some
string'}, {'id': 'some_float', 'value': 1.0}]}]
```
