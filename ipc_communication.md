


| **Method**  | **Used In** | **Communication** | **Type** | **Use Case** |
|------------|------------|-------------------|----------|-------------|
| `ipcRenderer.on(event, callback)`  | Renderer  | Listens for an event sent by the Main Process  | **Asynchronous** (Event-based)  | When the Main Process needs to continuously send data to the Renderer (e.g., real-time updates) |
| `ipcRenderer.send(event, data?)`  | Renderer  | Sends data to the Main Process (fire-and-forget)  | **Asynchronous** (One-way)  | When the Renderer needs to **trigger** something in the Main Process but **doesn’t need a response** (e.g., logging, notifications) |
| `ipcRenderer.invoke(event, data?)`  | Renderer  | Sends a request to the Main Process and waits for a response  | **Asynchronous** (Request-Response)  | When the Renderer needs a response from the Main Process (e.g., fetching data from files, getting system information) |
| `ipcMain.handle(event, callback)`  | Main  | Listens for `.invoke()` calls and responds  | **Asynchronous** (Request-Response)  | When the Main Process needs to return a value to the Renderer (e.g., reading a file and sending its contents back) |

<!--stackedit_data:
eyJoaXN0b3J5IjpbODAyMTYwMDI3XX0=
-->