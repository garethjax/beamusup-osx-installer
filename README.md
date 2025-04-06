# BeamUsUp for macOS

This repository contains instructions and helper files to run the `BeamUsUp` web crawler Java application on **macOS** using a `.jar` file.

The instructions below are tested under **macOS Sonoma 14.7**. Minor differences may apply on other versions.  
Huge thanks to **ChatGPT-4o** for supporting the adaptation of the original instructions to make them clean and accessible. **USE IT if you get stuck!**

---

## 🚀 Quick Start (Terminal Only)

If you just want to run the `.jar` file from the terminal:

1. **Install Homebrew** (if not already):

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.bash_profile
   source ~/.bash_profile
   ```

2. **Install OpenJDK**:

   ```bash
   brew install openjdk
   sudo ln -sfn /opt/homebrew/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk
   echo 'export PATH="/opt/homebrew/opt/openjdk/bin:$PATH"' >> ~/.bash_profile
   source ~/.bash_profile
   ```

3. **Move the `.jar` to a permanent location**:

   ```bash
   mkdir -p ~/apps
   mv ~/Downloads/beamusup.jar ~/apps/
   ```

4. **Run the app**:

   ```bash
   java --add-exports java.desktop/com.apple.eawt=ALL-UNNAMED -jar ~/apps/beamusup.jar
   ```

5. *(Optional)* Add a shortcut inside .bash_profile (it's inside your user home folder)

   ```bash
   echo "alias beamusup='java --add-exports java.desktop/com.apple.eawt=ALL-UNNAMED -jar ~/apps/beamusup.jar'" >> ~/.bash_profile
   source ~/.bash_profile
   ```
   you can replace beamusup with whatever shortcut you want: "buu" or "sfrog" ;) 

---

## 🖥️ Create a `.app` Bundle (GUI Launcher)

If you want a native-like `.app` that you can double-click:

1. **Create the bundle structure**:

   ```bash
   mkdir -p ~/apps/BeamUsUp.app/Contents/{MacOS,Resources}
   ```

2. **Create the `Info.plist` file** at `~/apps/BeamUsUp.app/Contents/Info.plist`:
   > (i recommend using an app like BBEDIT or typinng from terminal "nano ~/apps/BeamUsUp.app/Contents/Info.plist")

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>CFBundleName</key>
       <string>BeamUsUp</string>
       <key>CFBundleDisplayName</key>
       <string>Beam Us Up</string>
       <key>CFBundleIdentifier</key>
       <string>com.example.beamusup</string>
       <key>CFBundleVersion</key>
       <string>1.0</string>
       <key>CFBundlePackageType</key>
       <string>APPL</string>
       <key>CFBundleSignature</key>
       <string>????</string>
       <key>CFBundleExecutable</key>
       <string>beamusup</string>
   </dict>
   </plist>
   ```

4. **Create the launcher script** at `~/apps/BeamUsUp.app/Contents/MacOS/beamusup`:
> (again, using BBEDIT or typing "nano ~/apps/BeamUsUp.app/Contents/MacOS/beamusup"

   ```bash
   #!/bin/bash
   exec /opt/homebrew/bin/java --add-exports java.desktop/com.apple.eawt=ALL-UNNAMED -jar ~/apps/beamusup.jar
   ```

   Then make it executable:

   ```bash
   chmod +x ~/apps/BeamUsUp.app/Contents/MacOS/beamusup
   ```

6. *(Optional)* Add an icon:

   - Save an icon as `beamusup.icns` in `~/apps/BeamUsUp.app/Contents/Resources/`
   - Add this to `Info.plist`:

     ```xml
     <key>CFBundleIconFile</key>
     <string>beamusup</string>
     ```

7. **Move to Applications**:

   ```bash
   mv ~/apps/BeamUsUp.app /Applications/
   ```

Now you can launch BeamUsUp from **Finder**, **Spotlight**, or pin it to your **Dock** ✅

<img width="332" alt="image" src="https://github.com/user-attachments/assets/71c7e045-7d09-4abd-b562-5398ab6b6e32" />

## 🎨 Customize the App Icon (macOS)


<img width="324" alt="image" src="https://github.com/user-attachments/assets/f7618648-3654-419b-bf28-2d5293a0e50a" />


You can customize the icon of `BeamUsUp.app` to give it a more polished and professional look.

### ✅ 1. Prepare an `.icns` icon file

If you already have an `.icns` file (e.g. `beamusup.icns`), you're good to go.

If not, convert a `.png` image to `.icns` using one of the following methods:

- Online: [https://cloudconvert.com/png-to-icns](https://cloudconvert.com/png-to-icns)
- macOS Preview: open the image → **File → Export** → choose **ICNS** (or PNG, then convert)

Make sure the icon is at least **512x512** pixels.

---

### ✅ 2. Copy the icon to the app bundle

Open Terminal and run:

```bash
mkdir -p /Applications/BeamUsUp.app/Contents/Resources
cp ~/Downloads/beamusup.icns /Applications/BeamUsUp.app/Contents/Resources/
```

Replace the path with the location of your actual `.icns` file if needed.

---

### ✅ 3. Edit the `Info.plist`

Edit the app’s property list file:

```bash
sudo nano /Applications/BeamUsUp.app/Contents/Info.plist
```

Add this key inside the `<dict>` block (anywhere before `</dict>`):

```xml
<key>CFBundleIconFile</key>
<string>beamusup</string>
```

> 💡 Do **not** include the `.icns` extension — just the filename.

---

### ✅ 4. Refresh the icon cache

To force macOS to display the new icon:

```bash
touch /Applications/BeamUsUp.app
killall Finder
```

Alternatively, restart your Mac.

---

The app should now appear with the custom icon in Finder, Spotlight, and the Dock! 🎉

---

## 🧠 Notes

- These instructions were tested under **macOS Sonoma 14.7**.
- The `--add-exports` JVM flag is required for GUI integration with the macOS menu bar.
- You may see font-related warnings (e.g., "Times not available") — they do not impact functionality.

---

## 🙌 Credits

- Author: [Andrea Scarpetta](https://www.andreascarpetta.com) - Tecnical SEO & Ai Evangelist (2025)
- Thanks to **ChatGPT-4o** for assisting with command cleanup, Java packaging, and compatibility clarification.


---
