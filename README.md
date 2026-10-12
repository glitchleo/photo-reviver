# Photo Reviver

A Python class project for restoring old or damaged photos and adding color to black-and-white images.

The desktop app lets you:

- Restore photos with Microsoft's Bringing-Old-Photos-Back-to-Life (BOPTL) model.
- Add color with DeOldify and optionally apply one of six color palettes.
- Adjust brightness, contrast, color, denoise, and sharpening.
- Compare each stage and export the finished photo.

## Quick start

Requires Python 3.11 or newer. From the project folder, run these commands in Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -e .
python .\app.py
```

Without model files, use `passthrough` to try the interface and basic image adjustments. This mode skips AI restoration. For AI restoration and colorization, complete the model setup below.

## Using the app

1. Upload a photo, choose a restoration engine, and click **Restore Photo**.
2. Optionally run DeOldify, try a palette, and keep your preferred version.
3. Adjust the enhancement controls and compare the results.
4. Click **Download Final Product** to save the finished image.

Run folders, intermediate images, comparisons, and `run_summary.json` are saved under `artifacts/runs/` by default.

## AI model setup

Install the additional dependencies:

```powershell
python -m pip install -r requirements-boptl.txt
```

Download the model repositories and pretrained weights separately. The app expects these paths:

```text
external/bringing-old-photos-back-to-life/
  run.py
  Face_Detection/shape_predictor_68_face_landmarks.dat
  Face_Enhancement/checkpoints/
  Global/checkpoints/
external/deoldify/
  deoldify/visualize.py
  models/ColorizeArtistic_gen.pth
```

The app reports missing model files. If BOPTL runs out of CPU memory, lower **BOPTL max side** to `640` or `512`.

## Command line

Try the basic pipeline:

```powershell
photo-reviver --input "C:\path\to\old_photo.jpg"
```

Run AI restoration and colorization after setting up both models:

```powershell
photo-reviver --input "C:\path\to\old_photo.jpg" --config .\configs\pipeline.boptl.deoldify.json
```

JSON files in `configs/` control pipeline settings. Use `pipeline.boptl.json` for restoration only. CLI colorization defaults to grayscale source images; `--output-root` changes the output folder.

## Development

App and pipeline code lives in `src/photo_reviver/`; tests live in `tests/`.

```powershell
python -m unittest discover -s tests
```
