# J2ME Builder IDE

App icon 

<img width="512" height="512" alt="1791543948-picsay" src="https://github.com/user-attachments/assets/33a665b1-516a-4206-b89c-0330708cba57" />


Screenshots

<img width="480" height="960" alt="Screenshot_20261005-134122" src="https://github.com/user-attachments/assets/4617e2ed-c022-461f-949b-c8b22cccedd7" />
<img width="480" height="960" alt="Screenshot_20261006-080257" src="https://github.com/user-attachments/assets/8d45b017-1145-47b1-8048-785b2b4e1d07" />
<img width="480" height="960" alt="Screenshot_20261006-080401" src="https://github.com/user-attachments/assets/b1a67a25-a2ea-4362-8396-0a996b603cdd" />
<img width="480" height="960" alt="Screenshot_20261006-080342" src="https://github.com/user-attachments/assets/0a193960-3354-4c07-b7f5-15f90039fc96" />
<img width="480" height="960" alt="Screenshot_20261004-202039" src="https://github.com/user-attachments/assets/50afac31-bbe6-40b4-99f7-369ab301a454" />



**A complete J2ME (MIDP 2.0 / CLDC 1.1) development environment that runs on your Android phone.**
Write MIDlets, compile them, preverify them, package a `.jar` + `.jad`, and open the result in an emulator — without a PC.

By **DASH ANIMATION V2** · Package `com.dashers.j2mebuilderide` · Version **1.0 (1)**

---

## Compatibility

| Android version | Install | Compile / build |
|---|---|---|
| **Android 9** (API 28) | ✅ | ✅ full support |
| **Android 8** (API 26–27) | ✅ | ✅ full support |
| **Android 6** (API 23) | ✅ installs and opens | ❌ **cannot compile** |
| Android 5.x, 7.x, 10 and newer | installs (minSdk 21) | not tested yet |

The compiler (ECJ 3.20.0) and the preverifier (ProGuard-core 9.1.1) need Java APIs that Android only has from 7.0 (API 24). On Android 6 you can still install the app, edit projects and files, use MIDI Studio and import / export, but you cannot build a MIDlet.

---

## What it does

- **Code editor** with syntax highlighting, line numbers, undo / redo, find and replace, pinch-to-zoom, 6 colour schemes, code suggestions, auto-import for missing types and **closing-bracket suggestions** for any `(` `[` `{` still open. Built to stay smooth with files of 4000+ lines.
- **Project files drawer** — swipe right from the left edge of the editor (or tap the folder button) to browse `src/` and `res/`; open `.java` files, preview images, edit small text files.
- **Compiler** — ECJ (the Eclipse compiler) produces Java 1.2 class files for J2ME; a built-in compiler is the automatic fallback.
- **Preverify** — ProGuard-core inlines `jsr`/`ret` and writes the CLDC `StackMap` attribute.
- **Packaging** — `MANIFEST.MF`, `.jad`, resources and libraries into a `.jar`, optional obfuscation, then straight into an emulator such as J2ME Loader.
- **Live build log** with Log / Errors / Warnings tabs, clear error messages (`file:line:column`, the source line with `^^^`, and a J2ME-specific tip) and a numbered **How to fix** list when a build fails.
- **Projects** — package manager (create, rename, move, health check), import from **ZIP or folder**, resource import, icon chooser (auto-detect or pick any image in `res/`).
- **Templates** — Hello Form, Canvas animation, Menu list, 3D cube (M3G), OpenGL ES triangle.
- **MIDI Studio** — write music as text, 128 General MIDI instruments, live preview, add to `res/`.
- **Themes** — 9 app themes (Midnight, Daylight, AMOLED Black, Dracula, Ocean, Forest, Sunset, Solarized Light, Rose Light).

---

## Quick start

1. Open the app and allow storage access when asked.
2. Create a project (pick a template or start empty) — or import a ZIP / folder.
3. Edit your classes under `src/`. Put images, sounds and other data under `res/`.
4. Tap **Build**. Watch the steps in the log: files found → libraries → compile → classes → preverify → packaging.
5. When the build succeeds the `.jar` / `.jad` opens in your emulator (for example **J2ME Loader**).
6. If it fails, read the red lines: each error has a tip. Fix the **first** error first — later ones are often caused by it.

### Where your files are

| What | Location |
|---|---|
| Projects | `/J2MEBuilder/projects/<name>/` on shared storage |
| Project folders | `src/` (code), `res/` (resources), `lib/` (jars compiled against and packed), `lib/provided/` (compile-only jars), `bin/` (build output) |
| Optional ECJ override | `/J2MEBuilder/Library/` — a dexed `ecj*.jar` or `.dex` placed here is used instead of the bundled ECJ |

### Writing code for J2ME

CLDC / MIDP is much smaller than Java SE. Common swaps (the error tips suggest them for you):

| Java SE | Use on J2ME |
|---|---|
| `ArrayList`, `LinkedList`, `List` | `Vector` |
| `HashMap`, `Map` | `Hashtable` |
| `Iterator` | `Enumeration` |
| `StringBuilder` | `StringBuffer` |
| `String.contains(x)` | `s.indexOf(x) >= 0` |
| `String.isEmpty()` | `s.length() == 0` |
| `java.io.File` | JSR-75 `FileConnection` (add the library) |
| `java.net.*` | `javax.microedition.io.Connector` |
| Swing / AWT | `javax.microedition.lcdui` (Form, Canvas, Graphics) |

### Libraries

- Drop your own J2ME `.jar` files into the project's library list. Jars in `lib/` are compiled against **and** packed into the MIDlet; jars in `lib/provided/` are compile-only (the phone supplies them).
- Optional platform APIs (MMAPI, JSR-75 file access, M3G, Bluetooth ...) are detected from your `import` lines and reported as *Library loaded* or *Library missing*.

---

## How a build works

```
 src/*.java
     │  1. read sources, check libraries
     ▼
 ECJ 3.20.0  ── (or built-in compiler if ECJ cannot start) ──►  .class files (version 46)
     │  2. preverify  (ProGuard-core: jsr/ret inlined, CLDC StackMap)
     ▼
 classes + res/ + libraries ──► MANIFEST.MF + .jad ──► .jar
     │  3. package
     ▼
 open in emulator
```

- **Compile classpath:** the bundled J2ME API (`assets/j2me-sdk-api.jar`, 449 CLDC / MIDP / JSR classes, also unpacked in `assets/j2me/api/`).
- **Preverify classpath:** the real API jars in `assets/j2me/jars/` — `cldcapi11`, `midpapi20`, `mmapi`, `jsr75`, `jsr184`, `jsr226`, `jsr234`, `jsr238`, `jsr239` — plus your project libraries.
- **ECJ start-up** is retried 3 times with a fresh class loader before the built-in compiler is used, and the log shows the real reason.
- Classes from the built-in compiler already carry their own StackMap, so they are not preverified again.

---

## Troubleshooting

| Symptom | What to do |
|---|---|
| Build log says *"ECJ could not start on this device, built-in compiler used instead"* | Read the cause in the amber line. The build still runs with the built-in compiler; its messages are less detailed. |
| Build fails on Android 6 | Not supported yet — use Android 8 or 9. |
| `Library missing: import ...` | Add the matching `.jar` / JSR under Libraries, or remove the import. |
| *"cannot be resolved to a type"* for `ArrayList`, `HashMap`, … | They do not exist on J2ME — see the table above. |
| *"log4j-api is missing"* in the preverify line | The preverify step only warns. Add `org.apache.logging.log4j:log4j-api:2.19.0` (Gradle does it automatically with proguard-core). |
| Emulator says the class is not preverified | Check the preverify line in the build log — it must say N of N classes preverified. |

---

## Building the app from source

**Requirements:** Android Studio (Gradle) — `compileSdkVersion 29`, `minSdkVersion 21`, `targetSdkVersion 29`, Java 8.

Dependencies in `app/build.gradle`:

```gradle
implementation 'org.eclipse.jdt:ecj:3.20.0'
implementation 'com.guardsquare:proguard-core:9.1.1'      // also pulls log4j-api and kotlin-stdlib
implementation 'org.apache.logging.log4j:log4j-api:2.19.0'
```

- **Why proguard-core 9.1.1:** 7.0.0 is not supported by the Sketchware builder; 9.1.1 is.
- Both libraries are reached **by reflection** (`EcjCompiler`, `ProGuardPreverifier`), so the project still compiles and runs without them — it then uses the built-in compiler and skips the extra preverify.
- **Sketchware:** add ECJ and proguard-core as Local libraries (Maven download). `ecj-mini.jar` in `app/libs/` is for Sketchware only and is excluded from Gradle builds to avoid duplicate classes.
- **Bundled assets:** `j2me-sdk-api.jar`, `j2me/api/`, `j2me/jars/`, `ecj/ecj-dex.jar` (ECJ 3.20.0 already dexed, with its resource files), `proguard/proguard-core-dex.jar`, `api_index_core.txt`, `ecj_messages.properties`, `vendor.png` (150×150 — replace it to change the logo in About), fonts.

### Source layout

```
app/src/main/java/com/dashers/j2mebuilderide/
├── MainActivity.java       UI: home, editor, files, settings, MIDI Studio, About, splash
├── JCompilerBridge.java    build pipeline: compile → preverify → package, progress steps
├── compiler/               built-in compiler (lexer, parser, code generator, StackMap),
│                           EcjCompiler (reflection), ErrorTips, API index
├── ide/                    projects, package manager, libraries, manifest/JAD, ZIP writer,
│                           ProGuardPreverifier, EcjLoader, ApiAssets, templates, emulator launcher
└── ui/                     small view helpers
```

---

## Known limitations

- Android 6 cannot compile; Android 5.x, 7.x and 10+ are untested.
- Preverified classes have been checked structurally (every method gets a `StackMap` attribute), but have not yet been run on a real CLDC phone — emulators are the tested path.
- Internet is only used for the link icons in About (cached after the first download).

---

## Credits and licences

| Component | Used for | Licence |
|---|---|---|
| ECJ (Eclipse Compiler for Java) 3.20.0 | main compiler | Eclipse Public License |
| ProGuard-core 9.1.1 | preverifier | Apache License 2.0 |
| log4j-api 2.19.0 | needed by proguard-core | Apache License 2.0 |
| CLDC / MIDP / JSR API jars in `assets/j2me/jars/` | preverify classpath | Sun / Oracle / vendor API stubs — check their redistribution terms before publishing |

**App licence:** MIT

© DASH ANIMATION V2 
