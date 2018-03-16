---
title: Glossary
weight: 190
---

#### Allocation Manager
- _An implementation of the AllocationManager interface, the Allocation Manager provides a central access point for all allocations performed in the Domain._

#### Application
- _“Generically, an executable software program which may contain one or more modules. Within SCA, an application consists of one or more software modules which implement the Base Application Interfaces and which are identified within a Software Assembly Descriptor file. When loaded and executed, these modules create one or more components which comprise the application. ” Source: SCA v2.2.2._

#### Application Factory
- _“An instantiation of the ApplicationFactory interface is used to create an instance of an application. The domain manager creates an application factory for each Software Assembly Descriptor that is installed. ” Source: SCA v2.2.2._

#### Assembly Controller
- _“The assemblycontroller element of the Software Assembly Descriptor indicates the component that is the main resource controller for an application. ” Source: SCA v2.2.2._

#### Component
- _A processing module that implements the CF::Resource interface that is deployable by an Application Factory (refer to SCA v2.2.2 for a description of CF::Resource)._

#### Connection Manager
- _An implementation of the ConnectionManager interface, the Connection Manager provides a central access point for connections between Domain objects._

#### Console View
- _Displays a variety of console types depending on the type of development and the current set of user settings. The Console View is a part of Eclipse and the basic use is well documented by the Eclipse documentation: http://help.eclipse.org/. The REDHAWK IDE uses multiple Console Views for different purposes. There are also third party plug-ins within REDHAWK that have their own Console Views._

#### CORBA Name Browser
- _Maps names to specific CORBA Servants. The CORBA Name Browser can be used to examine the current contents of the Naming Service as well as perform basic manipulation of that context. The contents of the view will display all currently bound name contexts (folders) and objects. The CORBA Name Browser can be seen in Figure 14.3._

#### Device
- _“The SCA defines a Device interface class. This interface is an abstraction of a hardware device that defines the capabilities, attributes, and interfaces for that device.” Source: SCA v2.2.2. In the context of REDHAWK, a Device is is a software module that implements the Device interface class._

#### Device Manager
- _An implementation of the DeviceManager interface. The Device Manager is responsible for the creation of Devices and Services._

#### Domain
- _“A Domain defines a set of hardware devices and available applications under the control of a single Domain Manager.” Source: SCA v2.2.2._

#### Domain Manager
- _“An implementation of the DomainManager interface, a domain manager manages the complete set of available hardware devices and applications. It is responsible for the set-up and shut-down of applications and for allocating resources, devices, and non-CORBA components to hardware devices.” Source: SCA v2.2.2._

#### Editor
- _Depending on the type of file that is being edited, the appropriate editor is displayed in the editor area. For example, if a .TXT file is being edited, a text editor is displayed in the editor area. Source: <http://help.eclipse.org/>._

#### Error Log View
- _An Eclipse view that provides details of errors that have occurred with the running application._

#### Ethernet
- _the Local Area Network standard IEEE 802.3._

#### Event Channel
- _A software object that mediates the transfer of CORBA events between producers and consumers._

#### Event Channel Manager
- _An implementation of the EventChannelManager interface, the Event Channel Manager provides a central access point for the management of Event Channels in the Domain._

#### Event Service
- _Software that provides and manages Event Channels._

#### File Manager
- _An implementation of the FileManager interface, the File Manager provides a central access point for all of the file systems available in the Domain._

#### GPP
- _General Purpose Processor Device that manages a computing Node._

#### Hardware-Accelerated Component
- _A REDHAWK component that has the ability to access/maintain a designated portion of programmed hardware (i.e., a single modem in a multi-modem load)._

#### Linux
- _A free and open-source operating system that is Unix-like._

#### Local Area Network
- _A computer network that interconnects computers within a limited area, usually a single building or smaller, using a single type of physical network technology._

#### Message
- _Key/value pairs contained in a single instance of the CORBA::Any type and passed over the CORBA event API._

#### Naming Service
- _Software that provides a mapping between human-readable names of CORBA objects and the CORBA objects themselves._

#### NIC
- _The network interface card, usually interfacing with Ethernet._

#### Node
- _A single Device Manager, its associated Devices, and its associated Services._

#### Node Editor
- _The Node Editor is opened by double clicking a DCD file from the Project Explorer View. It presents all the content that can be found within the dcd.xml file in an editing environment designed for human use. The Node Editor contain an Overview, Devices, Diagram, and a raw XML tab which contains the DCD file content._

#### NUMA
- _Non-Uniform Memory Access._

#### Outline View
- _Displays an outline of a structured file that is currently open in the editor area. The Outline View is a part of Eclipse and the basic use is well documented by the Eclipse documentation: <http://help.eclipse.org>_

#### Persona
- _The load or programming files used to load functionality onto programmable hardware (i.e., bit files)._

#### Persona Device
- _A REDHAWK device that encapsulates a single load for specific programmable hardware and any corresponding ports/properties (defined in the REDHAWK XML descriptor files)._

#### Perspective
- _Each Workbench window contains one or more perspectives. A perspective defines the initial set and layout of views in the Workbench window. Within the window, each perspective shares the same set of editors. Each perspective provides a set of functionality aimed at accomplishing a specific type of task or works with specific types of resources. For example, the Java perspective combines views that you would commonly use while editing Java source files, while the Debug perspective contains the views that you would use while debugging Java programs. As you work in the Workbench, you will probably switch perspectives frequently. Perspectives control what appears in certain menus and toolbars. They define visible action sets, which you can change to customize a perspective. You can save a perspective that you build in this manner, making your own custom perspective that you can open again later. Source: <http://help.eclipse.org>_

#### Port
- _A CORBA object that produces or consumes data and/or commands. A Port is referred to as “uses” when it is a source/producer/output, and as a “provides” when it is a sink/consumer/input. A uses Port implements the CF::Port interface (see SCA v2.2.2 for a description of CF::Port)._

#### Problems View
- _As you work with resources in the workbench, various builders may automatically log problems, errors, or warnings in the Problems view. The Problems View is a part of Eclipse and the basic use is well documented by the Eclipse documentation: <http://help.eclipse.org>_

#### Programmable Device
- _A REDHAWK device that is the main proxy into physical hardware. It maintains which Persona (child) device should be loaded onto the hardware._

#### Programmable Hardware
- _Hardware that may be dynamically programmed to operate with specific functionality (Such as FPGAs and some microprocessors)._

#### Project Explorer View
- _Provides a hierarchical view of the resources in the Workbench. The Project Explorer View is a part of Eclipse and the basic use is well documented by the Eclipse documentation: <http://help.eclipse.org>_

#### Properties View
- _Displays property names and basic properties of a selected resource. The Properties View is a part of Eclipse and the basic use is well documented by the Eclipse documentation: <http://help.eclipse.org>_

#### Property
- _“An SCA Property is a variable that contains a value of a specific type. Configuration Properties are parameters to the configure and query operations of the PropertySet interface. Allocation Properties define the capabilities required of a Device by a Resource.” Source: SCA v2.2.2._

#### PyDev
- _PyDev is a third-party plug-in for Eclipse. It is an IDE used for programming in Python supporting code refactoring, graphical debugging, code analysis and many other features._

#### REDHAWK Explorer
- _The REDHAWK Explorer, an application built on the REDHAWK Core Framework, is used to navigate the contents of a REDHAWK Domain. It provides capabilities for viewing the contents of the Domain, configuring Domain resources, and launching Domain Waveforms._

#### REDHAWK Explorer View
- _Allows a user to navigate the contents of a REDHAWK Domain. It provides capabilities for viewing the contents of the Domain, configuring instantiated resources, and launching Applications in a target SDR environment. It also provides access to the Sandbox, which is an environment for running Components and Applications without Domain Manager and Device Manager._

#### Sandbox
- _A Python environment that can be used to easily run and interact with Components without having to create a Waveform or having to run a Domain Manager._

#### SCA Waveform
- _Deprecated. See Waveform._

#### Service
- _Software made available by the Device Manager at the Device Manager startup._

#### SoftPkg Editor
- _The SoftPkg Editor is opened by double clicking an SPD file from the Project Explorer View. It presents all the content that can be found within the spd.xml file in an editing environment designed for human use. If the SPD file references a PRF or SCD file, additional tabs are made available that represent these files in similar fashion._

#### System
- _The combination of hardware and (REDHAWK and non-REDHAWK) software necessary to accomplish a particular goal or set of goals._

#### Target SDR
- _The Target SDR refers to the REDHAWK resources which your workspace will be built and run against. It describes the platform that you are developing for. By default, points to the file location specified by the environment variable SDRROOT._

#### usesdevice relationship
- _A logical association between a Component and a specific Device that provides some capacity to that Component._

#### View
- _A view is a workbench part that can navigate a hierarchy of information or display properties for an object. Only one instance of any given view is open in a workbench page. When the user makes selections or other changes in a view, those changes are immediately reflected in the workbench. Views are often provided to support a corresponding editor. For example, an outline view shows a structured view of the information in an editor. A properties view shows the properties of an object that is currently being edited. Source: <http://help.eclipse.org>_

#### Waveform
- _A REDHAWK Waveform is analogous to an SCA Waveform Application. A REDHAWK Waveform is defined as a composition of Components, their interconnections, and their configuration overrides. This composition is defined in a Software Assembly Descriptor file._

#### Waveform Editor
- _The Waveform Editor is opened by double clicking an SAD file from the Project Explorer View. It presents all the content that can be found within the sad.xml file in an editing environment designed for human use. The Waveform Editor contains an Overview, Diagram, and a raw XML tab which contains the SAD file content._

#### Workbench
- _The term Workbench refers to the desktop development environment. The Workbench aims to achieve seamless tool integration and controlled openness by providing a common paradigm for the creation, management, and navigation of workspace resources. Source: <http://help.eclipse.org/>_

#### Workspace
- _In Eclipse, a workspace is a logical collection of projects that you are actively working on. Source: <http://help.eclipse.org>_
