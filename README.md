
# Sublime Text Portable C++ Build System (with I/O Redirection & Timeout)

This repository/set of files contains a custom **build system for Sublime Text** (portable version), designed to:

- Compile C++ code using `g++.exe`
- Run the compiled `.exe` file with **input/output redirection**
- Automatically **terminate execution after 15 seconds** to avoid infinite loops or long runtimes
- Use a **shared `IO` folder** placed inside the Sublime Text portable directory for inputs and outputs

---

## 📁 Folder Structure

```
SublimeTextPortable/
├── IO/
│   ├── input.txt
│   └── output.txt
├── sublime_text.exe
└── Data/
    └── Packages/
        └── User/
            └── C++ with IO and Timeout.sublime-build
```

- Place your input in `IO/input.txt`.
- Output will be written to `IO/output.txt`.

---

## ⚙ Build System Setup

Create a file named:

```
Data/Packages/User/C++ with IO and Timeout.sublime-build
```

Paste the following content inside:

```json
{
  "cmd": [
    "cmd", "/c",
    "g++.exe -std=c++17 \"${file}\" -o \"${file_base_name}.exe\" && (start \"\" /b cmd /c \"\"${file_base_name}.exe\" < \"${packages}\\..\\..\\IO\\input.txt\" > \"${packages}\\..\\..\\IO\\output.txt\" & timeout /t 15 >nul & taskkill /im \"${file_base_name}.exe\" /f\")"
  ],
  "selector": "source.cpp",
  "shell": true,
  "working_dir": "$file_path"
}
```

---

## 🛠 How It Works

1. **Compiles your current C++ file** using `g++.exe` with C++17.
2. **Redirects input/output** from/to files in the `IO` folder.
3. **Executes in a background shell**.
4. **Waits 15 seconds**, then checks and kills the process if it's still running.

This ensures you don't get stuck in infinite loops or long waits during development.

---

## 🧪 Example Workflow

1. Write C++ code in Sublime Text.
2. Save your file.
3. Provide necessary input in `IO/input.txt`.
4. Press `Ctrl + B` to build.
5. Check `IO/output.txt` for the program's output.

---

## ❗ Requirements

- `g++.exe` (MinGW) available in your `PATH` or bundled with your portable setup.
- Windows environment (uses `cmd`, `timeout`, and `taskkill`).
- Sublime Text Portable.

---

## ✅ Notes

- If you open multiple `.exe` programs with the same name, the timeout kill may affect all of them.
- To prevent this, work with unique filenames or enhance the script to track process IDs.

---

## 📦 Optional Improvements

- Use **PowerShell** or **WSL/bash** for more reliable control.
- Extend with **build variants** for debug/release modes.
- Auto-open `output.txt` on build finish (optional scripting).

---

## License

MIT License or Public Domain — use freely.
