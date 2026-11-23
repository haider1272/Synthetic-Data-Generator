# Synthetic Data Generator

SAM 3-powered synthetic data generation for YOLO datasets. Cuts each object out with a SAM 3 mask, scales it,
pastes it back on the cleaned background and recomputes the label. Built to cover object sizes the camera has
not seen yet (first use case: metal nuts/screws, measured small 33.00 x 23.91 mm, large 38.55 x 32.79 mm).

## Features
- SAM 3 masks, with GrabCut then rectangle fallback
- per-class scale factors (`--class-scale 0:1.37 1:1.17 2:1.37`)
- collision-aware scaling: neighbours are scaled as a group, capped, or left unchanged
- tight mask-based YOLO boxes (`--box-mode tight|margin|scaled`)
- memory-safe batch inference: chunking, half precision, OOM recovery, CPU fallback
- GUI (run without arguments) and CLI

## Run
```bash
pip install -r requirements.txt
python screw_resizer_yaml.py --yaml final_dataset/data.yaml --out big_dataset \
    --splits train --class-scale 0:1.37 1:1.17 2:1.37
python screw_resizer_yaml.py                      # opens the GUI
python size_sweep.py                              # which training size covers all other sizes
```
`size_sweep.py` imports `screw_resizer_yaml.py`, so keep them in the same folder.

## Layout
```
synthetic-data-generator/
├── screw_resizer_yaml.py   main generator (GUI + CLI)
├── size_sweep.py           training-size sweep
├── legacy/scale_nuts_sam3.py   older single-factor version
└── examples/               before/after and collision-handling images
```
Related: [LabelPilot](https://github.com/haider1272/labelpilot) (annotation tool).
