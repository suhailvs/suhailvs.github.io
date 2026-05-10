## how to bundle a Python + GTK app so that it can be installed using apt

To distribute a Python + GTK app installable with `apt`, you create a Debian package (`.deb`) and optionally host it in an APT repository.

Typical structure:

```text
myapp/
├── DEBIAN/
│   └── control
├── usr/
│   ├── bin/
│   │   └── myapp
│   ├── share/
│   │   ├── applications/
│   │   │   └── myapp.desktop
│   │   ├── icons/
│   │   │   └── hicolor/256x256/apps/myapp.png
│   │   └── myapp/
│   │       └── app.py
```

---

#### 1. Create the GTK app

Example:

```python
#!/usr/bin/env python3

import gi
gi.require_version("Gtk", "4.0")
from gi.repository import Gtk

class App(Gtk.Application):
    def __init__(self):
        super().__init__()

    def do_activate(self):
        win = Gtk.ApplicationWindow(application=self)
        win.set_title("My GTK App")
        win.set_default_size(400, 200)

        button = Gtk.Button(label="Hello")
        win.set_child(button)

        win.present()

app = App()
app.run()
```

Save as:

```text
usr/share/myapp/app.py
```

Make executable:

```bash
chmod +x usr/share/myapp/app.py
```

---

#### 2. Create launcher script

File:

```text
usr/bin/myapp
```

Content:

```bash
#!/bin/bash
exec python3 /usr/share/myapp/app.py
```

Make executable:

```bash
chmod +x usr/bin/myapp
```

---

#### 3. Create desktop entry

File:

```text
usr/share/applications/myapp.desktop
```

Content:

```ini
[Desktop Entry]
Name=My GTK App
Exec=myapp
Icon=myapp
Type=Application
Categories=Utility;
Terminal=false
```

---

#### 4. Add icon

Place:

```text
usr/share/icons/hicolor/256x256/apps/myapp.png
```

---

#### 5. Create Debian control file

File:

```text
DEBIAN/control
```

Content:

```text
Package: myapp
Version: 1.0
Section: utils
Priority: optional
Architecture: all
Maintainer: Your Name <you@example.com>
Depends: python3, python3-gi, gir1.2-gtk-4.0
Description: Simple GTK application
 A sample GTK desktop app written in Python.
```

---

#### 6. Build the `.deb`

From parent directory:

```bash
dpkg-deb --build myapp
```

Result:

```text
myapp.deb
```

Install:

```bash
sudo apt install ./myapp.deb
```

