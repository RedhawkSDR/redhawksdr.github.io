---
title: "Creating and Running a Hello World Component"
weight: 60
---

Use the following procedure to create a simple component that prints `hello world` to the terminal upon startup.

1.  Create a new REDHAWK component Project:
  - **File > New > REDHAWK Component Project**

2.  Name the project: *HelloWorld*

3.  Click **Next**.

4.  Select:
  - **Prog. Lang: C++**

5.  Click **Next**.

6.  Click **Finish**.
  - If a dialog asks to switch to CPP perspective, click **No**.

7.  Generate Code:
  - In the editor tool bar, click **Generate All Component Implementations**

8.  In the `HelloWorld.cpp` file, add the following include to the beginning of the file:
    ```cpp
     #include <iostream>
    ```

9.  In the `HelloWorld.cpp` file, add the following code to the `serviceFunction()` method:
    ```cpp
     std::cout<<"Hello world"<<std::endl;
    ```

10. Compile the project:
  - **Project > Build Project**

11. Drag the project to the **Target SDR** section of the **REDHAWK Explorer**.

12. On a terminal, start a Python session.

13. Run the following commands:
  ```py
  >>> from ossie.utils import sb
  >>> hello_world = sb.launch("HelloWorld")
  >>> sb.start()
  ```
