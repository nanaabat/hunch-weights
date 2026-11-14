# hunch-weights

Experimental trained atmosphere checkpoints for [hunch](https://aprewaturkson.com/hunch).

These weights predict how people tend to say a space feels, learned from self-reported responses. They do not read a person's emotions, and they are not production-ready. See [MODEL_CARD.md](MODEL_CARD.md) for class names, training scores, and what is missing.

The default head is **extB**, an exterior-architecture atmosphere classifier (ViT-B/32). The calm class is stored as `exxteriorArch_calm` (typo preserved). Release tag: [`v0.3-weights`](https://github.com/nanaabat/hunch-weights/releases/tag/v0.3-weights). License: MIT.

The 350 MB checkpoints are GitHub Release assets, not git files. The hunch zip on the site stays small and fetches them during install, with a sha256 check.

## 1. Download hunch

```bash
curl -L -A "Mozilla/5.0" -o hunch.zip https://aprewaturkson.com/downloads/hunch.zip
unzip hunch.zip
cd hunch
```

(A browser User-Agent avoids some Cloudflare filters. You can also click download on [aprewaturkson.com/hunch](https://aprewaturkson.com/hunch).)

## 2. Install (fetches the default weights)

Use CPython 3.10 or newer. A virtual environment is recommended.

```bash
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
python install.py
```

`install.py` installs Pillow, numpy, torch, and torchvision, then downloads `extB` from this release, checks sha256, and caches it at `~/.hunch/weights/extB`. The download can be resumed if it is interrupted. Use `python install.py --no-weights` to skip the fetch, or `python install.py --all-weights` to also fetch extBNB, artB, and artNB.

The CLI uses that cached head by default when it is present.

Box test, 6:44 PM America/New_York on Saturday 14 Nov 2026: the zip was unpacked and `install.py` was pointed at a venv that already had the Python packages, so pip reported them already satisfied. The weights fetch itself ran against this release, wrote the files to a clean cache (`HUNCH_WEIGHTS_DIR` for the test; on a normal install that folder is `~/.hunch/weights/extB`), and checked sha256:

```
fetching trained atmosphere weights (sha256 checked, resumable):
fetching SHA256SUMS from https://github.com/nanaabat/hunch-weights/releases/download/v0.3-weights
weights extB: exterior architecture atmosphere (default)
  model_info.json 100% (176 / 176 bytes)
  model_info.json sha256 ok
  model_weights.pth 0% (1048576 / 349888771 bytes)
  model_weights.pth 5% (17825792 / 349888771 bytes)
  model_weights.pth 10% (35651584 / 349888771 bytes)
  model_weights.pth 15% (53477376 / 349888771 bytes)
  model_weights.pth 20% (70254592 / 349888771 bytes)
  model_weights.pth 25% (88080384 / 349888771 bytes)
  model_weights.pth 30% (105906176 / 349888771 bytes)
  model_weights.pth 35% (122683392 / 349888771 bytes)
  model_weights.pth 40% (140509184 / 349888771 bytes)
  model_weights.pth 45% (158334976 / 349888771 bytes)
  model_weights.pth 50% (175112192 / 349888771 bytes)
  model_weights.pth 55% (192937984 / 349888771 bytes)
  model_weights.pth 60% (210763776 / 349888771 bytes)
  model_weights.pth 65% (227540992 / 349888771 bytes)
  model_weights.pth 70% (245366784 / 349888771 bytes)
  model_weights.pth 75% (263192576 / 349888771 bytes)
  model_weights.pth 80% (279969792 / 349888771 bytes)
  model_weights.pth 85% (297795584 / 349888771 bytes)
  model_weights.pth 90% (315621376 / 349888771 bytes)
  model_weights.pth 95% (332398592 / 349888771 bytes)
  model_weights.pth 100% (349888771 / 349888771 bytes)
  model_weights.pth sha256 ok
trained head ready at /workspace/hunch-test/v2/weights/e2e-cache/extB
the CLI uses this head by default when it is present.
```

The fetched `model_weights.pth` was 349,888,771 bytes, a real PyTorch zip (not an HTML page), sha256 `cdb3c527acaa6a432a6b63f7325edb57d37d1f0084ecfde539398ba39b54a44f`.

## 3. Manual download (if you skip install.py's fetch)

```bash
mkdir -p ~/.hunch/weights/extB
cd ~/.hunch/weights

curl -L -O https://github.com/nanaabat/hunch-weights/releases/download/v0.3-weights/SHA256SUMS
curl -L -O https://github.com/nanaabat/hunch-weights/releases/download/v0.3-weights/extB-model_weights.pth
curl -L -O https://github.com/nanaabat/hunch-weights/releases/download/v0.3-weights/extB-model_info.json

sha256sum -c SHA256SUMS --ignore-missing

mv extB-model_weights.pth extB/model_weights.pth
mv extB-model_info.json extB/model_info.json
```

`hunch` looks for `model_info.json` and `model_weights.pth` inside `~/.hunch/weights/extB`. You can also set `HUNCH_WEIGHTS_DIR` (or `HUNCH_CACHE_DIR/weights`). To use another shipped head, fetch the matching `NAME-model_*` files into `~/.hunch/weights/NAME/`.

## 4. Run the CLI on the sample model

From the unzipped `hunch` folder, with the environment active:

```bash
python hunch.py samples/hunch-house.3dm --site Boston --orient 90 --person 2.5,0,2.5 --target calm
```

Box test, 6:44 PM America/New_York on Saturday 14 Nov 2026, after the install above, from the unzipped zip, with no `--model` flag. The command used the fetched extB head in the test cache (on a normal install the printed path is `~/.hunch/weights/extB`). The command above is the one to copy.

```
site: Boston, Massachusetts, United States (42.3584, -71.0598)
climate cached: 2024 hourly weather, comfort hours 10% (simple outdoor band, experimental)
person viewpoint in kitchen: enclosure 39.2, prospect 11.4, view out=True
using trained atmosphere head: /workspace/hunch-test/v2/weights/e2e-cache/extB
rendering views of samples/hunch-house.3dm into /workspace/hunch-test/v2/weights/e2e/cli/renders
reading /workspace/hunch-test/v2/weights/e2e/cli/renders/hunch-house_front.png
reading /workspace/hunch-test/v2/weights/e2e/cli/renders/hunch-house_three-quarter.png
reading /workspace/hunch-test/v2/weights/e2e/cli/renders/hunch-house_elevated.png
  kitchen: cozy because: timber/warm materials 95%, warm surface colour 62, 18 C est, modest facade sun
  loft: calm because: mid surface value 91, even daylight 0, 18 C, material warmth 48
  stair: open because: window-to-wall 8%, ceiling 5.20 m, prospect 8
top suggestion toward calm: use lighter plaster surfaces (toward calm: 24 to 31 (+7.1). comfort hours 8% to 8% (+0.0 points). predicted feel: calm.)
report written: /workspace/hunch-test/v2/weights/e2e/cli/report.html /workspace/hunch-test/v2/weights/e2e/cli/report.json
this building tends to feel open in 1 of 3 rooms. kitchen: cozy (cozy because: timber/warm materials 95%, warm surface colour 62, 18 C est, modest facade sun) loft: calm (calm because: mid surface value 91, even daylight 0, 18 C, material warmth 48) stair: open (open because: window-to-wall 8%, ceiling 5.20 m, prospect 8)
predicts how this space tends to feel, from building features (geometry, materials, colour, daylight, temperature) and, if present, a rendered view. experimental. it does not read a real person's emotions.
```

The trained head ran on the three rendered views and predicted `exxteriorArch_calm` at 83.6%, 89.8%, and 72.6%. Those scores are one factor in the room breakdown, labelled "studio image model (experimental, not a reading of a person)".

## 5. Rhino and Grasshopper

Rhino and Grasshopper were not tested on the packaging machine. They are labelled untested in rhino. The intended path is:

```bash
python install.py --rhino
```

If auto-detect fails, open Rhino 8's Python 3 editor, run `import sys; print(sys.executable)`, then `python install.py --rhino /that/path`. That also installs `rhino3dm`.

Open `samples/hunch-house.3dm`. It is a small house (kitchen, loft, stair) with glass openings, a point named `person-feet`, and a vertical line for the viewpoint. Y is up. The model's +X is meant to face east (`--orient 90`), so the long glazed wall faces south. You can also run `samples/run_in_rhino.py` from the Script Editor.

For Grasshopper: add a Python 3 script component, open or paste `grasshopper/Hunch.py`, and wire:

- Inputs: `Run` (boolean), `Geometry` or `ModelPath` (path to the `.3dm`), `Site` (for example Boston), `Target` (for example calm), `Person` (the `person-feet` point; a viewpoint, not a mood subject), `Orient` (90), `PackageRoot` (the unzipped hunch folder), `OutFolder`
- Outputs: `ReportPath`, `HeatmapPath`, `Suggestions`, `Log`

Follow `TEST-IN-RHINO.md` in the zip. Please tell us what worked and what did not.

## 6. Reading the report

`report.html` and `report.json` are written to the output folder (`hunch-report` by default, or `-o`). In the box test they included:

- **Atmosphere scores** for each room. Kitchen: cozy 36.7, calm 24.1, open 17.2, focused 13.3, plus smaller uneasy / closed-in / energized scores. The breakdown names materials, colour, a labelled indoor temperature estimate, and facade sun. The kitchen also listed the studio head: `exxteriorArch_calm 84%`.
- **Climate**, under "site climate". Open-Meteo 2024 hourly weather for Boston, cached after the first fetch. Annual outdoor comfort hours were 10% (833 of 8784 hours) on a simple 18-26 C / 30-70% RH outdoor band. That is not ASHRAE 55, and indoor temperatures are a solar-gain estimate, not a thermal simulation. `climate.png` plots the monthly values.
- **Sun path**. Room scores include facade exposure (here "modest facade sun"; this run was after sunset in Boston, so mean exposure was 0). For a day-and-seasons chart:

```bash
python hunch.py sun samples/hunch-house.3dm --site Boston --orient 90 --person 2.5,0,2.5
```

Box test output (6:39 PM America/New_York, 14 Nov 2026):

```
march equinox: daylight ~12.0 h, max altitude 47.5 deg
june solstice: daylight ~15.0 h, max altitude 70.9 deg
september equinox: daylight ~12.0 h, max altitude 47.9 deg
december solstice: daylight ~9.0 h, max altitude 24.1 deg
sun path chart: sun-path.png
```

- **Heatmap**, a room-by-room plan:

```bash
python hunch.py heatmap samples/hunch-house.3dm --site Boston --orient 90 --person 2.5,0,2.5 --target calm --rooms samples/rooms.json
```

Box test: the same room feels as the analyse run, and `heatmap.png` was written next to `report.html`.

- **Suggestions**, labelled experimental. Toward a target of calm, the top change on this sample was "use lighter plaster surfaces" in the kitchen (calm score 24 to 31, predicted feel: calm). Comfort hours did not move. The report says these are model predictions, not a design guarantee.

## 7. Limits and honesty

- The model predicts how people tend to say a space feels, learned from self-reported responses. It does not read, detect, or measure a real person's emotions.
- A person placed in the model is a viewpoint, not a mood subject.
- This is a research prototype, not production-ready, and not a clinical or design guarantee.
- extB training accuracy in `model_info.json` is 0.9955. extBNB is 1.0 and is likely overfit. Treat the scores as experimental.
- The extB calm class name is stored as `exxteriorArch_calm` (typo preserved).
- artB and artNB are artwork atmosphere heads, not architecture. They are optional (`--all-weights` or a manual fetch).
- No interior checkpoint could be shipped: Drive had `model_info.json` for the interior heads, but no matching `model_weights.pth`.
- Climate comfort uses a simple outdoor band, not a certified comfort model. Indoor temperature in the report is a labelled estimate, not a thermal simulation.
- Rhino and Grasshopper paths are untested in rhino on the packaging machine.
