Electron.js has **two main processes**:

1.  **Main Process**
    
    -   Runs in a **Node.js environment**.
        
    -   Manages the application's lifecycle (window creation, menus, system events).
        
    -   Has **full access** to the filesystem, OS APIs, and native Electron modules.
        
    -   Can **spawn multiple Renderer Processes**.
        
    -   Uses `ipcMain` for communication with Renderer Processes.
        
2.  **Renderer Process**
    
    -   Runs inside a **Chromium browser** (like a web page).
        
    -   Handles the **UI** (HTML, CSS, JavaScript, React, etc.).
        
    -   Can communicate with the Main Process using **IPC (Inter-Process Communication)**.
        
    -   Runs in a **sandboxed** environment (limited access to system resources).
        

----------

### **Comparison of Main Process vs. Renderer Process**

| **Feature** | **Main Process** | **Renderer Process** |
|----------------------|---------------------------------|---------------------------------|
| Execution Context   | Runs in a **Node.js** environment | Runs in a **Chromium browser** |
| Responsibility      | Controls **app lifecycle**, windows, system access | Manages **UI** and frontend logic |
| Access to OS APIs   | ✅ Full access (Filesystem, Menus, Notifications) | ❌ No direct access (Requires `preload.js`) |
| Number of Instances | **Single** (1 per app) | **Multiple** (1 per window) |
| Communication       | Uses `ipcMain` for IPC | Uses `ipcRenderer` for IPC |
| Example Usage       | Creates windows, manages files, interacts with OS | Displays UI, handles user interactions |
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwODM2OTY5NDFdfQ==
-->