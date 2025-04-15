## **Inter-Process Communication (IPC) in Electron.js**

### **What is IPC?**

Inter-Process Communication (**IPC**) in Electron **enables communication** between the **Main Process** and the **Renderer Process**. Since the Renderer Process runs in a separate environment (Chromium), it **cannot** directly access Node.js modules. IPC helps bridge this gap.

----------

## **How IPC Works in Electron?**

1.  The **Renderer Process** sends a message to the **Main Process** using `ipcRenderer`.
    
2.  The **Main Process** listens for the event using `ipcMain`.
    
3.  The **Main Process** processes the request and optionally sends a response.
    
4.  The **Renderer Process** receives and processes the response.
    

----------

## **Types of IPC Communication in Electron**

| **IPC Type** | **Sender** | **Receiver** | **Communication Type** | **When to Use?** |
|-------------|------------|-------------|------------------------|------------------|
| `send` + `on` | `ipcRenderer.send(event, data?)` | `ipcMain.on(event, callback)` | **One-way (fire-and-forget)** | When Renderer needs to trigger an action in the Main Process but doesn’t need a response. |
| `send` + `once` | `ipcRenderer.send(event, data?)` | `ipcMain.once(event, callback)` | **One-time event listener** | When Renderer needs to trigger an action only **once**. |
| `invoke` + `handle` | `ipcRenderer.invoke(event, data?)` | `ipcMain.handle(event, callback)` | **Request-Response (async)** | When Renderer needs a **response** from the Main Process (e.g., fetching files, system info). |
| `webContents.send` + `on` | `mainWindow.webContents.send(event, data?)` | `ipcRenderer.on(event, callback)` | **Main Process to Renderer (one-way push)** | When the Main Process needs to **send real-time updates** (e.g., CPU usage, notifications). |

----------

## **1️⃣ One-Way Communication (`send` + `on`)**

Used when the **Renderer Process sends data** to the **Main Process** without expecting a response.

### **Example**

#### **Renderer Process (`renderer.js`)**

js
```js
// Send a message to the Main Process 
 window.electron.send("log-message", "User clicked a button!");
``` 

#### **Main Process (`main.js`)**


```js
const { ipcMain } = require("electron");

ipcMain.on("log-message", (event, message) => { console.log("Received message:", message);
});
``` 

✔ **Use Case:** Logging, triggering actions, saving files.

----------

## **2️⃣ Request-Response Communication (`invoke` + `handle`)**

Used when the **Renderer Process needs a response** from the **Main Process**.

### **Example**

#### **Renderer Process (`renderer.js`)**

js

```js
// Request system info from the Main Process  
window.electron.invoke("get-system-info").
then((info) => { console.log("System Info:", info);
});
``` 

#### **Main Process (`main.js`)**

js

```js
const { ipcMain } = require("electron"); const os = require("os");

ipcMain.handle("get-system-info", async () => { return { cpu: os.cpus()[0].model, memory: os.totalmem() };
});
``` 

✔ **Use Case:** Fetching system information, reading/writing files.

----------

## **3️⃣ Sending Data from Main Process to Renderer (`webContents.send`)**

Used when the **Main Process needs to send data** to the **Renderer Process** (e.g., real-time updates).

### **Example**

#### **Main Process (`main.js`)**

js

```js
setInterval(() => {
    mainWindow.webContents.send("statistics", { cpuUsage: Math.random() });
}, 2000);
``` 

#### **Renderer Process (`renderer.js`)**

js

```js
window.electron.on("statistics", (stats) => { console.log("Live CPU Usage:", stats.cpuUsage);
});
``` 

✔ **Use Case:** Live updates, notifications.

----------

## **IPC Security Best Practices**

1.  **Use `contextBridge`**
    
    -   Expose only necessary functions to the Renderer using `contextBridge.exposeInMainWorld()`.
        
2.  **Avoid using `remote` API**
    
    -   `remote` is deprecated due to security risks. Use `ipcRenderer.invoke()` instead.
        
3.  **Validate Input Data**
    
    -   Never trust input from the Renderer Process. Always sanitize and validate data.
        

----------

## **Conclusion**

-   IPC allows communication **between the Main Process and Renderer Process**.
    
-   Use **`.send()` for fire-and-forget**, **`.invoke()` for request-response**, and **`webContents.send()` for real-time updates**.
    
-   Always follow **security best practices** to prevent vulnerabilities.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI1OTI3MTk5NF19
-->