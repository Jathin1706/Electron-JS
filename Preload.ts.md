## 🔧 What is `preload.js` used for?

The **preload script** runs **before** the web page (React/Vite/etc.) is loaded, and it can:

### ✅ 1. **Expose Safe APIs** to the Renderer (frontend)

You can use `contextBridge` to selectively expose **safe functions** to the browser side (without giving full Node.js access):

ts

```ts
// preload.js or preload.ts  const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electronAPI', { openFile: () => ipcRenderer.send('dialog:openFile'),
});
``` 

And in your React frontend:

js

`window.electronAPI.openFile();` 

----------

### ✅ 2. **Enable Secure IPC (Inter-Process Communication)**

The preload script allows **controlled messaging** between the frontend and backend (`main.js`) via `ipcRenderer`.

----------

### ✅ 3. **Protect Against Security Risks**

Without a preload script, enabling Node.js directly in the renderer (`nodeIntegration: true`) can expose your app to **XSS and remote code execution vulnerabilities**.

Preload provides a **sandboxed, safe communication layer.**

----------

## 🛡️ Why Not Use Node.js Directly in React?

Because modern best practices in Electron **disable direct Node.js access in the renderer**, using:

js


```ts
webPreferences: { preload: path.join(__dirname, 'preload.js'), nodeIntegration: false, contextIsolation: true,
}
``` 

That’s why the **preload file is essential** to securely pass data/functions to your frontend.
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTU3OTYwNDM3XX0=
-->