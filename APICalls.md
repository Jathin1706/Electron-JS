### 🧠 1. **From the Renderer Process** (your React UI)

This works the same as in any React app:


```ts
// Using fetch  fetch('https://your-api.com/data')
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err)); // or using Axios  import axios from  'axios';

axios.get('https://your-api.com/data')
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
  ``` 

⚠️ **Note**: If your API is local (like `http://localhost:5000`), ensure CORS is properly configured on your API side.

----------

### 🛡️ 2. **From the Main Process** (Node backend in Electron)

You can use `node-fetch`, `axios`, or even native `http` module:

ts

```ts
// main.js or main.ts  import fetch from  'node-fetch'; fetch('https://your-api.com/data')
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
  ``` 

You might need to install `node-fetch`:

bash

CopyEdit

`npm install node-fetch` 

----------

### 🔄 3. **Secure API Calls via Preload (Bridge IPC)**

To securely call APIs from renderer via main:

#### a) **preload.ts**

ts

```ts
import { contextBridge, ipcRenderer } from  'electron';

contextBridge.exposeInMainWorld('api', { fetchData: (url: string) => ipcRenderer.invoke('api:fetch', url)
});
``` 

#### b) **main.ts**

ts

```ts
import { ipcMain } from  'electron'; import fetch from  'node-fetch';

ipcMain.handle('api:fetch', async (_, url) => { const res = await  fetch(url); return res.json();
});
``` 

#### c) **renderer (React)**

ts


```ts
// @ts-ignore  window.api.fetchData('https://your-api.com/data')
  .then(data => console.log(data))
  .catch(err => console.error(err));
  ```




-----
## Using net module (not recommended)
in main process

``` ts
ipcMain.handle("doSomething", () => {
  const request = net.request("https://www.boredapi.com/api/activity/");
  
  request.on("response", (response) => {
    const data = [];

    response.on("data", (chunk) => {
      data.push(chunk);
      // console.log(chunk);
    });

    response.on("end", () => {
      const json = Buffer.concat(data).toString();
      console.log(json);
    });
  });

  request.end();
});
```


here dosomething is the name of the event that u will use in the frontend via ipc bus 

use 
`windows.indexBridge.dosomething()` to get the json response


in preload.js

```ts
const { contextBridge, ipcMain, ipcRenderer } = require('electron');

let indexBridge = {
  doSomething: async () => {
    var result = await ipcRenderer.invoke("doSomething");
  }
}

contextBridge.exposeInMainWorld("indexBridge", indexBridge);

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNzQwODMwNTVdfQ==
-->