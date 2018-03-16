---
title: "Auto-Generated Component Methods"
weight: 20
---

This section provides an overview of noteworthy methods provided in the auto-generated component files. In some cases, the names of the methods vary by language.

### `serviceFunction()`

The core functionality of a component resides in the `serviceFunction()` method in C++, the `process()` method in Python, and the `serviceFunction()` method in Java. The `serviceFunction()` is invoked on a recurring basis after `start()` is called on the component’s base class.

### `constructor()`

This is the component/device constructor. When this function is invoked, properties of kind property are initialized to their default or overloaded state.
