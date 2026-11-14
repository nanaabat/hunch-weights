# Hunch atmosphere checkpoints (model card)

These files are experimental machine-learning checkpoints for [hunch](https://aprewaturkson.com/hunch). They predict how people tend to say a space feels, learned from self-reported responses. They do not read a person's emotions, and they are not a clinical or production mood detector.

The hunch code stays in a separate private repository. This public repository only hosts the trained heads, their `model_info.json`, checksums, and this card.

## How they were trained (what we can verify)

Each head is a ViT-B/32 ImageNet backbone with a replaced classification layer. The studio trainer froze the backbone by default (`freeze_pretrained_parameters = True`) and trained the head. The published files are the full saved `state_dict` (about 349,888,771 bytes each), which is what the hunch classifier loads as `model_weights.pth` next to `model_info.json`.

Training accuracy numbers below come from those `model_info.json` files. High scores, especially 1.0, likely include overfitting. Treat every score as experimental.

## Checkpoints in release `v0.3-weights`

These are the only folders in Drive that contained a `model_weights.pth`. Default install fetches `extB` only.

| name | classes (as stored) | training accuracy | training loss | notes |
|---|---|---|---|---|
| extB (default) | exteriorArch_anxiety, exxteriorArch_calm | 0.9955 | 0.0566 | Exterior architecture atmosphere. The calm class name has a typo (`exxteriorArch_calm`); it is kept as stored. |
| extBNB | anxiety_high, calm_low | 1.0 | 0.0675 | Exterior atmosphere with generic class names. Training accuracy 1.0 is likely overfit. |
| artB | art_anxiety, art_calm | 0.9840 | 0.0702 | Artwork atmosphere, not architecture. |
| artNB | art_anxiety, art_calm | 0.9854 | 0.0518 | Artwork atmosphere, not architecture. |

## Not in this release

These folders had `model_info.json` only. No matching `model_weights.pth` was present on Drive, so they could not be fetched or shipped.

| name | classes (as stored) | training accuracy | why missing |
|---|---|---|---|
| intB | interiorArch_anxiety, interiorArch_calm | 0.9775 | no weights file |
| intBNB | anxiety_high, calm_low | 1.0 | no weights file |
| saveIntModel | interiorArch_anxiety, interiorArch_calm, interiorArch_steadyAlertness | 0.9337 | no weights file |
| saveExtModel | exteriorArch_anxiety, exteriorArch_steadyAlertness, exxteriorArch_calm | 0.98 | no weights file |
| saveartmodel | art_anxiety, art_calm, art_steadyAlertness | 0.8717 | no weights file |
| savetrainmodel | art, ext-arch, int-arch, landscape | 0.9321 | no weights file; this is a subject classifier, not an atmosphere head |
| extNB | exteriorArch_anxiety, exxteriorArch_calm | 0.9873 | only `model_traced.pt` was present, not `model_weights.pth` |

Training images, videos, and `.bak` duplicates are intentionally not published.

## Intended use

Use with the hunch CLI or the Grasshopper component after `python install.py`, which downloads the default `extB` head, checks sha256, and caches it at `~/.hunch/weights/extB`. See the README for tested commands.

## Out of scope

- Reading or measuring a real person's emotions
- A guarantee about how a finished building will feel
- Climate or thermal simulation (hunch's comfort band is a simple outdoor 18-26 C / 30-70% RH rule, not ASHRAE 55)
- Interior atmosphere prediction from a trained interior head, until those weights exist
