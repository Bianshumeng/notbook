# Repository Guidelines

## Project Structure & Modules
- `StickyNotes/` NetBeans/Ant project; Java sources live in `StickyNotes/src/com/dosse/stickynotes/` (core UI and logic in `Main.java`, `Note.java`, dialogs, color pickers).
- `StickyNotes/src/com/dosse/stickynotes/locale/` holds translations; `fonts/` ships bundled font assets.
- `StickyNotes/nbproject/` NetBeans metadata and generated Ant targets; `build.xml` imports it.
- `_SETUP/` contains installer/packaging assets (Inno Setup scripts, launch4j config); adjust only when producing installers.

## Build, Test, and Development Commands
- Prereqs: JDK 7+ and Ant. Commands run from `StickyNotes/`.
- `ant clean jar` - compile and produce `dist/StickyNotes.jar`.
- `ant run` - compile and launch via Ant target (mirrors NetBeans Run).
- `ant clean` - remove build outputs. If NetBeans is used, disable Compile on Save when relying on Ant.
- Manual run: `java -jar dist/StickyNotes.jar` after a successful build.

## Coding Style & Naming Conventions
- Language: Java Swing; 4-space indentation; UTF-8; keep GPL header on touched files.
- Self-explanatory names; camelCase for methods/fields, UpperCamelCase for classes.
- Avoid new dependencies; keep UI logic minimal and centered in `Note`/`Main`.
- Resource updates: keep locale keys consistent; place new assets under `locale/` or `fonts/` and reference via relative paths.

## Testing Guidelines
- No automated tests; rely on manual verification.
- Smoke test: build jar, create/edit multiple notes, move/resize/zoom, change colors, restart to confirm persistence and backup recovery, check DPI scaling on Windows if possible.

## Commit & Pull Request Guidelines
- Commit messages: concise, imperative (e.g., "Fix note autosave path"). Include scope (UI, persistence, locale) when helpful.
- PR expectations: describe purpose, main changes, manual test scope; attach before/after screenshots for UI tweaks; mention impacted files (e.g., `Main.java`, `Note.java`, locales).
- Keep changes small and focused (KISS/YAGNI). Avoid cross-cutting refactors without discussion.

## Packaging & Distribution Tips
- Windows installer: prepare build via `ant clean jar`, then update `_SETUP` assets and run Inno Setup/launch4j configs as needed.
- Linux/other platforms: distribute `dist/StickyNotes.jar`; ensure launcher scripts point to the built jar.

## Build & Installer Runbook (2025-11-24)
- 工具放置：`.tools/apache-ant-1.10.14`、`.tools/jdk8/jdk8u442-b06`、`.tools/launch4j`（便携下载）。
- 构建 Jar：在 `StickyNotes/` 执行 `..\\.tools\\apache-ant-1.10.14\\bin\\ant.bat clean jar`，产出 `StickyNotes/dist/StickyNotes.jar`。
- 准备 Windows 安装素材：在 `_SETUP/Windows/` 创建 `setupFiles/`，放入 `StickyNotes.jar`、launch4j 生成的 `StickyNotes.exe`、以及从便携 JDK 拷贝的 `jre/`。
- 生成 exe：因路径含中文导致 windres 报错，改在 ASCII 目录 `C:\\l4jwork` 使用 launch4j（配置同 `sticky_launcher.xml`），成功产出 `StickyNotes.exe` 并复制回 `_SETUP/Windows/setupFiles/`。
- 编译安装包：安装 Inno Setup 6 后运行 `ISCC.exe "_SETUP\\Windows\\setup.iss"`（已执行），产出 `_SETUP/Windows/notebot-setup.exe`；有 `pf` 重命名警告但编译成功。
