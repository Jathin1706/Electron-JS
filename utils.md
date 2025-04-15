
## **What is the Use of the `utils.ts` File in Electron?**

In Electron projects, a **`utils.ts` (or `utils.js`) file** is used to store **utility functions** that can be reused across multiple files. These functions typically perform common tasks such as **data formatting, file operations, logging, error handling, and system information retrieval**.

----------

## **Why Use a `utils.ts` File?**

1.  **Code Reusability** → Avoids repeating the same logic in multiple places.
    
2.  **Separation of Concerns** → Keeps the main logic clean by moving helper functions elsewhere.
    
3.  **Improved Maintainability** → Changes to a function affect all parts of the app that use it.
    
4.  **Better Testing** → Utility functions can be unit-tested separately.
    

----------

## **Example Use Cases of `utils.ts` in Electron**

| **Utility Function** | **Use Case** |
|----------------------|-------------|
| `formatBytes(bytes: number)` | Converts bytes into a human-readable format (e.g., KB, MB, GB). |
| `getAppPath()` | Returns the application's installation path. |
| `logError(error: Error)` | Logs errors to a file for debugging. |
| `readJSONFile(filePath: string)` | Reads and parses JSON configuration files. |
| `writeToFile(filePath: string, data: string)` | Writes data to a file. | 

----------

## **Example: Common Utility Functions in `utils.ts`**

#### **1️⃣ Convert Bytes to a Readable Format**

ts

```ts
export  const formatBytes = (bytes: number, decimals = 2): string => { if (bytes === 0) return  "0 Bytes"; const k = 1024; const sizes = ["Bytes", "KB", "MB", "GB", "TB"]; const i = Math.floor(Math.log(bytes) / Math.log(k)); return  parseFloat((bytes / Math.pow(k, i)).toFixed(decimals)) + " " + sizes[i];
};
``` 

✔ **Use Case:** Displaying file sizes in a readable format.

----------

#### **2️⃣ Get the Application Path**

ts

CopyEdit

```ts
import { app } from  "electron"; export  const getAppPath = (): string => { return app.getAppPath();
};
``` 

✔ **Use Case:** Getting the installation directory of the Electron app.

----------

#### **3️⃣ Log Errors to a File**

ts

```ts
import fs from  "fs"; import path from  "path"; const logFilePath = path.join(__dirname, "error.log"); export  const logError = (error: Error): void => { const logMessage = `[${new  Date().toISOString()}] ERROR: ${error.message}\n`;
    fs.appendFileSync(logFilePath, logMessage);
};
``` 

✔ **Use Case:** Logging application errors for debugging.

----------

#### **4️⃣ Read and Parse JSON File**

ts

```ts
import fs from  "fs"; export  const readJSONFile = (filePath: string): any => { if (!fs.existsSync(filePath)) return  null; const data = fs.readFileSync(filePath, "utf-8"); return  JSON.parse(data);
};
``` 

✔ **Use Case:** Reading a configuration file in JSON format.

----------

#### **5️⃣ Write Data to a File**

ts

```ts
export  const writeToFile = (filePath: string, data: string): void => {
    fs.writeFileSync(filePath, data, "utf-8");
};
``` 

✔ **Use Case:** Writing logs, user preferences, or cache files.

----------

## **How to Use Utility Functions?**

#### **Example in `main.ts`**

ts

```ts
import { formatBytes, getAppPath } from  "./utils"; console.log("App Path:", getAppPath()); console.log("File Size:", formatBytes(1048576)); // Output: "1 MB"
``` 

----------

## **Conclusion**

-   `utils.ts` improves **code reusability**, **maintainability**, and **separation of concerns**.
    
-   It stores helper functions like **file handling, logging, formatting, and system operations**.
    
-   It **keeps the main process and renderer code clean** by moving common logic elsewhere.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NzMzODc4Ml19
-->