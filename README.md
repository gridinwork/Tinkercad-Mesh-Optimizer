# Tinkercad Mesh Optimizer

![Tinkercad Mesh Optimizer interface](IMG/1.png)

A Windows desktop utility for analyzing and simplifying 3D mesh models before importing them into Tinkercad. The application can automatically reduce complex STL/OBJ/PLY models to configurable Tinkercad-oriented limits, or provide full manual control over mesh simplification parameters.

## Project Goal

Complex 3D models exported from CAD, scanning, reconstruction, or online model libraries can be too large or contain too many triangles for convenient import into browser-based CAD tools such as Tinkercad. This project was created to provide a simple desktop workflow for inspecting a model, estimating whether it fits configured import limits, reducing polygon count when required, and saving a clean optimized STL together with processing metadata.

The application is designed for users who want to keep as much geometry as practical while reducing file size and polygon count enough for further editing in Tinkercad.

## Main Features

- Load and analyze **STL, OBJ, and PLY** mesh files.
- Display source file size, triangle count, vertex count, format, and compatibility status.
- One-click **Optimize for Tinkercad** mode.
- Manual target control using percentage or target triangle count.
- Adjustable quadric simplification aggressiveness.
- Optional border preservation.
- Optional lossless mode when supported by the simplification library.
- Gentle optimization mode intended to preserve proportions and topology more conservatively.
- Background processing so the GUI remains responsive during heavy mesh operations.
- Progress reporting and cancellation.
- Export optimized models as binary STL.
- Save JSON metadata describing before/after geometry and optimization parameters.
- Dark Windows desktop interface.
- Standalone Windows EXE build support via PyInstaller.

## Tinkercad-Oriented Automatic Mode

The automatic mode iteratively simplifies the model until it reaches the limits configured in the application.

Current default targets in this project are:

- **Maximum file size:** 25 MB
- **Maximum triangle count:** approximately 300,000 triangles

These values are application defaults and can be changed in the source if different limits are required.

If the original model is already within the configured limits, the program can preserve it without unnecessary simplification. If both limits cannot be reached without excessive reduction, the optimizer keeps the best result found and reports a warning instead of silently destroying model detail.

## Manual Optimization

For models that require more control, the manual mode allows the user to configure:

- percentage of geometry to retain;
- exact target triangle count;
- simplification aggressiveness;
- border preservation;
- lossless simplification option;
- conservative/gentle processing.

This makes the tool useful not only for Tinkercad preparation but also as a general lightweight mesh-reduction utility.

## Mesh Processing Pipeline

1. **Load** the source STL, OBJ, or PLY file.
2. **Analyze** file size, vertices, triangles, and mesh readability.
3. **Normalize and clean** the mesh where possible.
4. **Simplify** geometry using Fast Quadric Mesh Reduction through `pyfqmr`.
5. **Verify** the processed model against the configured limits.
6. **Export** the result as binary STL.
7. **Write metadata** to a companion JSON file.

The loader also includes a fallback for some malformed binary STL files whose triangle-count header does not match the actual file length.

## Technology Stack

- **Python 3.11**
- **Tkinter / ttk** — desktop GUI
- **NumPy** — mesh-array processing
- **Trimesh** — mesh loading, validation, cleanup, and STL export
- **pyfqmr** — Fast Quadric Mesh Reduction
- **SciPy** — numerical dependency
- **PyInstaller** — optional standalone Windows executable

## Project Structure

```text
Tinkercad-Mesh-Optimizer/
├── IMG/
│   └── 1.png
├── bootstrap/
│   ├── config.ps1
│   └── ensure_runtime.ps1
├── gui/
│   ├── main_window.py
│   └── theme.py
├── optimization/
│   ├── models/
│   │   └── schemas.py
│   ├── services/
│   │   ├── mesh_io.py
│   │   ├── quadric.py
│   │   ├── save_result.py
│   │   └── tinkercad.py
│   ├── ui/
│   │   └── optimization_tab.py
│   └── utils/
├── build_exe.py
├── install.bat
├── requirements.txt
├── run_app.py
└── Start.bat
```

## Running on Windows

### Option 1 — Project launcher

Run:

```text
Start.bat
```

The included bootstrap scripts are designed to prepare the required Python environment automatically.

### Option 2 — Existing Python installation

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the application:

```bash
python run_app.py
```

## Building a Standalone EXE

The project includes a PyInstaller build script:

```bash
python build_exe.py
```

The generated executable is placed in:

```text
Windows/Tinkercad_Optimizer.exe
```

## Output

Optimized geometry is exported as a binary STL file. A JSON file with the same base name is also generated and records information such as:

- source file path;
- original and resulting file sizes;
- triangle counts before and after optimization;
- vertex counts before and after optimization;
- processing mode;
- parameters used;
- UTC processing timestamp;
- optional warnings or notes.

## Use Cases

- Preparing complex models for Tinkercad.
- Reducing large STL files before browser-based editing.
- Simplifying 3D-scanned geometry.
- Reducing polygon count in downloaded printable models.
- Preparing CAD exports for lightweight sharing or further processing.
- Quickly comparing geometry size before and after simplification.

## Notes

This project is an independent utility and is not affiliated with or endorsed by Autodesk or Tinkercad. Tinkercad import behavior and platform limits may change over time; the limits used by the optimizer are configurable application defaults.
