## Using Electron with Django on Ubuntu

A common architecture is:

```text
Electron (Frontend Desktop App)
        ↓ HTTP/WebSocket
Django Backend (Local Server)
        ↓
SQLite/PostgreSQL
```

## Recommended Structure

```text
myproject/              # Django project
├── myapp
├── manage.py    
└── electron/       # Electron app
    ├── main.js
    └── package.json
```

# 1. Create Electron Frontend

Go back:

```bash
cd ..
mkdir electron
cd electron
```

Initialize:

```bash
npm init -y
npm install electron --save-dev
```


# 2. Create Electron Main Process

Create `main.js`

```javascript
const { app, BrowserWindow } = require('electron');
const path = require('path');
const { spawn } = require('child_process');

let djangoProcess = null;

function createWindow() {
    const win = new BrowserWindow({
        width: 1200,
        height: 800,
        webPreferences: {
            contextIsolation: true
        }
    });

    win.loadURL('http://127.0.0.1:8000');
}

function startDjango() {
    djangoProcess = spawn(
        '../env/bin/python',
        ['../manage.py', 'runserver', '127.0.0.1:8000'],
    );

    djangoProcess.stdout.on('data', data => {
        console.log(data.toString());
    });

    djangoProcess.stderr.on('data', data => {
        console.error(data.toString());
    });
}

app.whenReady().then(() => {
    startDjango();

    setTimeout(() => {
        createWindow();
    }, 3000);
});

app.on('window-all-closed', () => {
    if (djangoProcess) {
        djangoProcess.kill();
    }

    if (process.platform !== 'darwin') {
        app.quit();
    }
});
```

---

# 3. Update `package.json`

```json
{
  "name": "frontend",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": {
    "start": "electron . --no-sandbox"
  },
  "devDependencies": {
    "electron": "^42.0.1",
  }
}
```

Run:

```bash
npm start
```

Electron launches Django automatically.


# 4. Packaging for Ubuntu

Install Electron Builder:

```bash
npm install electron-builder --save-dev
```

Add to `package.json`:

```json
"build": {
  "appId": "com.suhailvs.myproject",
  "linux": {
    "target": "AppImage"
  }
},
```

Build:

```bash
sudo apt update
sudo apt install libfuse2t64
npx electron-builder
```

Output:

```bash
mv dist/*.AppImage .
chmod +x *.AppImage
./*.AppImage
```

* [Electron Documentation](https://www.electronjs.org/docs/latest/)
