---
title: "The Connection Process"
weight: 10
---

All connections take a client-server pattern. All calls are made from the client to the server. It is the role of the server to provide a set of functions that can be called by the client. It is the role of the client to understand what interfaces the server provides and to invoke (use) them. This is the basis for the uses/provides nomenclature for ports.

All uses ports implement the `CF::Port` interface. `CF::Port` is an interface that is part of the REDHAWK Core Framework; it contains only two methods: `connectPort()` and `disconnectPort()`. To connect a uses port to a provides port, an external entity needs to invoke the `connectPort()` function on the uses port, where the arguments are a CORBA pointer to the provides port and a string that identifies that connection. To sever a connection, an external entity needs to invoke the `disconnectPort()` function on the uses port, where its only argument is the string ID used to establish the connection. In the case of an application, the connections are established/destroyed by an object in the Domain Manager process space based on the waveform’s XML file. In the case of the Sandbox, the Sandbox makes the correct calls to establish and destroy a connection based on user input.

All provides ports must implement an interface described in IDL. This interface implements the methods that the uses port invokes after a connection has been made. When a uses port is given a pointer to a provides port, it essentially casts this generic pointer to the interface that it believes the provides port to implement. If this casting process fails, an exception is raised during the `connectPort()` call.
