---
title: "Miscellaneous"
weight: 70
---

#### Saving and Loading Waveforms

The components making up a waveform can be loaded into the workspace by passing the path and name of the waveform’s SAD XML file to the `loadSADFile()` method. Note that *usesdevice* relationships are ignored when loading a SAD file onto the Sandbox.

```py
>>> sb.loadSADFile("/path/to/sad/file/waveform.sad.xml")
```

The instantiated components and their associated connections can also be saved as a waveform. To perform this operation, pass the desired waveform name to the `generateSADXML()` method.

This method returns an XML string representing the contents of the SAD file; this string may then be written to a file:

```py
>>> sadString = sb.generateSADXML("waveform_name")
>>> fp=open("/path/to/sad/file/waveform_name.sad.xml","w")
>>> fp.write(sadString)
>>> fp.close()
```

#### Debug Statements and Standard Out

Standard out and standard error can be redirected to a file:

```py
>>> sb.redirectSTDOUT("/path/to/file/file.txt")
```

Debug statements can be set explicitly.

To set the debug output status, pass `True` or `False` to the `setDEBUG()` method:

```py
>>> sb.setDEBUG(True)
```

To get the current state of the debug output, use the `getDEBUG()` method:

```py
>>> sb.getDEBUG()
True
```

#### Processing Components from the Command Line

To process individual components from the command line, use the `proc()` function. The following command is an example of the `proc()` function call with sample arguments:

```py
>>>sb.proc("my_component","input_file",sink="output_file",sourceFmt="16t",sinkFmt="8u",sampleRate=10000,execparams={"execprop1":5},configure={"prop2":4},providesPortName="input",usesPortName="output",timeout=10)
```

##### `proc()` Function Arguments
| **Name**            | **Description**                                                                                                                                |
| :------------------ | :--------------------------------------------------------------------------------------------------------------------------------              |
| `<first argument>`  | Specifies the name of the component to run (required).                                                                                         |
| `<second argument>` | Specifies the name of the input file (required).                                                                                               |
| `sink`              | Specifies the name of the output file (optional). If this argument is omitted, exit the `proc()` function with Ctrl+C.                         |
| `sourceFmt`         | Specifies the format for the input file (optional).                                                                                            |
|                     | \- `8/16/32/64` bit resolution                                                                                                                 |
|                     | \- `u/t`: unsigned/signed                                                                                                                      |
|                     | \- `c`: complex (real if omitted)                                                                                                              |
|                     | \- `r`: big endian (little endian if omitted)                                                                                                  |
| `sinkFmt`           | Specifies the format for the output file (optional).                                                                                           |
| `sampleRate`        | Specifies the sampling rate for the input file (optional).                                                                                     |
| `execparams`        | Specifies the dictionary describing properties (of kind property with command line enabled) to be passed as command-line arguments (optional). |
| `configure`         | Specifies the dictionary describing properties (of kind configure or property) to be over-ridden (optional).                                   |
| `providesPortName`  | Specifies the component input port name (optional).                                                                                            |
| `usesPortName`      | Specifies the component output port name (optional).                                                                                           |
| `timeout`           | Specifies how long the `proc()` function runs before exiting (optional).                                                                       |
