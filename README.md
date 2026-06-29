# Faster xmgrace axis dialogs

A tiny patch for a surprisingly annoying pause.

When I was editing dense plots in Grace/xmgrace, double-clicking an axis could
feel like the program had frozen before the Axes dialog appeared. This patch
keeps that interaction fast.

In my workflow the problem was especially visible on macOS with XQuartz, but
the fix is not macOS-specific: it changes the shared Grace/xmgrace Motif UI
code.

## What it does

- Checks whether the user clicked an axis before scanning nearby data points.
- Creates the heavy Special-ticks tab only when that tab is actually used.

That is it. No UI redesign, no file-format changes, no installation-path
assumptions.

On my 200k-point test plot, the first axis-dialog open went from "did it hang?"
to roughly 0.2-0.3 seconds.

## Use it

From the root of a Grace/xmgrace source tree:

```sh
patch -p1 < /path/to/xmgrace-axis-latency.patch
```

Then build Grace/xmgrace as usual for your platform or package recipe.

The patch touches only:

```text
src/events.c
src/tickwin.c
```

For a quick check, open a dense plot, double-click an axis, and confirm the
Axes dialog opens promptly. Then open the Special tab once to confirm the tick
rows appear.

## License and note

The patch is distributed under `GPL-2.0-or-later`, matching the Grace/xmgrace
source files it modifies. The `LICENSE` file contains the GPL v2 text; the
"or later" permission comes from the upstream Grace/xmgrace source notices.

The coding and validation work for this patch was carried out with OpenAI
GPT-5, under my direction and with manual GUI testing on a real xmgrace build.
