# labview_thread
Threading in LabVIEW using asynchronous calls on strictly typed VIs

Threading in LabVIEW is strictly typed, which mean the connector pane and terminal wiring types (i.e. Required, Recommended, Optional) affect the asynchronous call methods. This threading library caches the strictly typed VI references and type casts the specified terminal types used by the **start the asynchronous call** and **wait for asynchronous call** methods.

![VI Tree](/docs/imgs/vitree.png)

## Getting Started

Run the **Thread.lvlib:Demo.vi** to see how to **open**, **start** and **close** a strictly typed VI thread. 

![Demo](/docs/imgs/demo.png)

Create a new thread:
- Create a new VI
- Set the **VI Execution** to **Preallocated-Reentrant**
- Set the **Connector Pane** to **4-2-2-4 (Default)**
- *Optional* Add a string control for **Name** and/or **Data** if neccessary
- Wire the controls to the upper left terminals as shown below:

![Terminals](/docs/imgs/terminals.png)

Save the VI and wire the **VI Path** and **Thread Name** to the **Open.vi** as shown in the demo.

Note: The **Open.vi** creates a new VI Server reference but does not start the thread, use the **Start.vi** to run the VI and **Close.vi** to wait for the thread to finish before disposing the VI reference.

Additionally, each named thread is cached so that a thread can be fetched anywhere within the application using the **Get.vi**. This allows the user to set/get the VI properties using the generic VI reference returned by the **Generic VI.vi**. 

Use the **List.vi** to get a list of all thread names registered. 

Warning: Each thread name must be unique. The last thread added with the same name override any previously existing (uses LabVIEW Maps).

## Type Casting

LabVIEW's strictly typed VIs require the connector pane and terminal wiring types to be identical, this means that the connector pane type, rotation, terminal wiring of Required, Recommended or Optional must be identical as well. This library circumvents the strict VI typeness by casting to a variant and performing type cast check when the asynchronous call and wait methods are called. This accounts for the development environment option: **Front Panel > Connector pane terminals default to required**.

![Justification](/docs/imgs/justification.png)

Under the hood, connector pane terminal type cast checks are performed to call the appropriate async methods, as illustrated here:

![Type Cast](/docs/imgs/typecast.png)

Threading in LabVIEW is difficult because of its strict data typing and overly complicated threading options (Non-Reentrant, Reentrant, Async Forget, Async Collect, etc.). This library provides the basic means to use LabVIEW's asynchronous call and collect without needing a lot of unneccessary wrapper methods.
