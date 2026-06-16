## Input data

The pipeline uses four STL surface models from the same specimen:

```text
pre_left.stl
post_left.stl
pre_right.stl
post_right.stl
```

The STL surface models are not included in this repository. To run the pipeline, place the four user-provided STL files in the `input_data/` folder using the filenames shown above.

The proximal/distal orientation vector required by the pipeline is included as:

```text
config/femur_orientation_vector.json
```

The user specifies which baseline side is used as the reference model with either:

```text
--reference-side L
```

or:

```text
--reference-side R
```

## What the pipeline does

The pipeline performs the following steps:

1. Loads the four STL meshes using `trimesh` processing.
2. Aligns each mesh to its principal inertia axes.
3. Uses `config/femur_orientation_vector.json` to orient the proximal and distal femoral ends consistently.
4. Mirrors left-sided meshes into a common right-sided coordinate convention.
5. Registers all four models to the selected Pre reference model.
6. Separately registers proximal and distal femoral regions.
7. Calculates distal femoral volume ratio and angular change.
8. Writes the aligned meshes, split femoral regions, and summary output tables.

## Running the example pipeline

After placing the four STL files in `input_data/`, run:

```bash
python run_sample_pipeline.py \
  --sample-id example \
  --input-dir input_data \
  --output-dir results/example \
  --reference-side L \
  --orientation-vector femur_orientation_vector.json
```

Change `--reference-side L` to `--reference-side R` if the right femur is the baseline non-operated reference side.

## Output files

The main output files are written to:

```text
results/example/aligned_obj/
results/example/split_meshes/
results/example/tables/distal_sweep.csv
results/example/tables/core_3d_summary.csv
results/example/tables/run_settings.json
```

The file `core_3d_summary.csv` contains the main calculated outputs, including distal femoral volume ratio and angular change components.
