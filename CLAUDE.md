# CLAUDE.md

## Purpose

This repository hosts TouchTronix glove calibration JSON files through GitHub Pages.

Customers scan a QR code that points directly to a hosted calibration JSON file.

## Repository Structure

```text
.nojekyll
index.html
pairs/
  PAIR001/
    Pair1_perpixel_pressure_DRAFT.json
    Pair1_perpixel_pressure_DRAFT_qr.png
```

Use one folder per glove pair:

```text
pairs/<PAIR_ID>/
```

Use the externally provided JSON filename as-is unless explicitly asked to rename it.

Do not create duplicate `latest.json` or versioned copies unless requested.

## Adding a New Calibration

Assume the calibration JSON is provided externally, for example:

```text
/path/to/SomeCalibration.json
```

Steps:

```bash
cd glove-calibrations
mkdir -p pairs/PAIR002
cp /path/to/SomeCalibration.json pairs/PAIR002/
```

Validate JSON before committing:

```bash
python -m json.tool pairs/PAIR002/SomeCalibration.json >/dev/null
```

## Generating QR Code

QR code should encode the direct hosted JSON URL.

Use Python for cross-platform support, including Windows:

```bash
pip install "qrcode[pil]"
python -c "import qrcode; qrcode.make('https://touchtronix-robotics.github.io/glove-calibrations/pairs/PAIR002/SomeCalibration.json').save('pairs/PAIR002/SomeCalibration_qr.png')"
```

PNG is for quick preview and printing labels.

## Updating Index Page

Update `index.html` with a link to the new JSON file and optionally its QR PNG/SVG.

Keep links relative, for example:

```html
<a href="pairs/PAIR002/SomeCalibration.json">PAIR002 / SomeCalibration.json</a>
```

## Commit and Push

```bash
git add .
git commit -m "Add PAIR002 calibration"
git push
```

After deploy, test:

```bash
curl -I "https://touchtronix-robotics.github.io/glove-calibrations/pairs/PAIR002/SomeCalibration.json"
```

Expected:

```text
HTTP/2 200
content-type: application/json
```

## Notes

- Keep calibration files public-safe. This repository is public.
- Keep original JSON filenames unless requested otherwise.
- QR codes must point to JSON files, not the repository page.
- Avoid storing customer-private information in public calibration JSON files.
- Do not add a `CNAME` file unless explicitly requested.
