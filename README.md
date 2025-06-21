# **Electron JS**

**Build cross-platform desktop apps using JavaScript, HTML, and CSS**

---

## **What is Electron?**

Electron is an open-source framework developed by GitHub that enables developers to build cross-platform desktop applications using web technologies — JavaScript, HTML, and CSS. It combines:

* **Chromium**: For rendering the user interface (like a browser).
* **Node.js**: To interact with the operating system and run backend code.

---

## **How an Electron App Works**

### 1. **Two Main Types of Processes**

#### ✅ **Main Process**

* This is the **entry point** of an Electron application (`main.js` or `main.ts`).
* It runs in a **Node.js environment**.
* Responsible for:

  * Managing the **application lifecycle** (e.g., app ready, quit).
  * Creating and managing **BrowserWindows** (windows that show your UI).
  * Interacting with **native system APIs** (like menus, notifications, Tray, clipboard, file system, etc.).
  * Can use **Node.js modules** like `fs`, `os`, `child_process`, etc.

#### ✅ **Renderer Process**

* Each `BrowserWindow` runs its own **renderer process**.
* These are like separate browser tabs, powered by **Chromium**.
* Responsible for:

  * Rendering the **UI** (HTML, CSS, JavaScript).
  * Executing client-side logic (like any frontend app).
* Runs in a **restricted environment** (like a browser sandbox).
* By default, **Node.js is not available** in the renderer for security reasons unless explicitly enabled.

---

### 2. **BrowserWindow**

* A core class provided by Electron to create a window on the desktop.
* Loads a local HTML file or a remote URL.
* Each instance runs in its own **renderer process**.
* Example:

  ```js
  const { BrowserWindow } = require('electron');

  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: false,  // disables Node in renderer (recommended)
      contextIsolation: true   // enables context isolation
    }
  });

  win.loadFile('index.html');
  ```

---

### 3. **Multiple Windows**

* Electron allows you to open **multiple BrowserWindows**.
* Each window has **its own independent renderer process**.
* The **main process** can manage all windows.

---

### 4. **Inter-Process Communication (IPC)**

Since the main and renderer processes are **isolated**, they communicate via **IPC (Inter-Process Communication)**.

* **Main process can listen and respond to renderer events.**
* **Renderer process can send messages to the main process.**

#### Example:

* **Renderer ➡️ Main**

  ```js
  // Renderer process
  window.electronAPI.sendMessage('get-data');
  ```

* **Main Process**

  ```js
  ipcMain.on('get-data', (event) => {
    event.reply('send-data', { name: 'Electron' });
  });
  ```

* **Main ➡️ Renderer**

  ```js
  // Renderer process
  ipcRenderer.on('send-data', (event, data) => {
    console.log(data); // { name: 'Electron' }
  });
  ```

> Note: Use `contextBridge` and `preload.js` to safely expose APIs to the renderer without enabling full Node.js access (for security).

---

## **Why Use Electron?**

* Cross-platform support (Windows, macOS, Linux).
* Single codebase using familiar web technologies.
* Access to low-level system APIs.
* Rich developer tools and community support.

---

## **Security Tips**

* **Disable Node.js integration in renderer**.
* **Enable `contextIsolation`**.
* Use **`preload.js`** for safe communication.
* Sanitize and validate all data received from renderer processes.
* Avoid loading remote content unless trusted.

---

## ****
