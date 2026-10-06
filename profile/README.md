# XUL-J: stream the interface, not the page

**XUL-J** is a JSON version of Mozilla's [XUL](https://en.wikipedia.org/wiki/XUL), sent as a stream
of small operations. A server, an MQTT device or an **unmodified desktop application** describes
*what* to show and *what can be done*; any client decides how it looks. Clicks go back as intents.

```jsonl
{"op":"command","id":"cmd_deploy","label":"Deploy","key":"ctrl+enter"}
{"op":"node","in":"root","tag":"window","id":"win","label":"Deploy console"}
{"op":"node","in":"win","tag":"toolbar","children":[{"tag":"toolbarbutton","command":"cmd_deploy"}]}
{"op":"rows","source":"log","append":[{"t":"12:01","msg":"build ok"}]}
```

▶ **[Live demo, docs and an LLM generator: xul-j.github.io](https://xul-j.github.io/)**

## Repositories

| | |
|---|---|
| [**xul-j**](https://github.com/xul-j/xul-j) | The protocol (JSON Schema), browser renderer, SSE server with resume, MQTT transport (the UI as retained topics), terminal renderer, demos and end-to-end tests. Node, no framework. |
| [**net-bridge**](https://github.com/xul-j/net-bridge) | Serves unmodified **.NET WinForms** apps to browsers, one instance per session. Infers flex layout from `Dock`/`Anchor`, maps real menus, and shows `MessageBox` and file dialogs in the browser. [Docs](https://xul-j.github.io/net-bridge/) |
| [**java-bridge**](https://github.com/xul-j/java-bridge) | The same for **Java Swing**: layout managers, `JOptionPane`, `JFileChooser`, real menus, trapped `System.exit`. JDK only, Java 8+. [Docs](https://xul-j.github.io/java-bridge/) |
| [**xul-j.github.io**](https://github.com/xul-j/xul-j.github.io) | The project site, an in-browser demo, and a generator where a model of your choice (OpenRouter, your own key) streams a UI and then acts as its backend. |

## Ideas borrowed from XUL

- **Ids and overlays:** anything can be patched later, and an element waits for a parent that hasn't arrived yet.
- **Commands:** intents separate from widgets. Disable one command, and its button, menu item and shortcut follow.
- **Broadcasters:** named values that attributes observe (`"observes": {"disabled": "busy"}`).
- **Boxes and flex:** a half-streamed layout still looks reasonable, and `pending` placeholders hold space.
- **Data apart from structure:** declare a tree once and stream rows into it (100k rows ≈ 30 DOM nodes).

## Status

A working prototype with end-to-end tests for every transport and bridge, not a finished product.
The bridges are tested on Linux (Mono's WinForms, OpenJDK Swing); Windows testing, context menus,
grid selection and authentication are next. Issues, ideas and pull requests are welcome.

All repositories are MIT licensed.
