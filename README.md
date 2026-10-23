# Tomographic Reconstruction in MATLAB

Materials for the 2023 DIP OPT reconstruction project, including a MATLAB cone-beam CT example collection.

This repository is a fork of [Flora1216/2023DIP-OPT-Reconstruction-](https://github.com/Flora1216/2023DIP-OPT-Reconstruction-). The implementations and project materials were inherited from upstream; this fork's documentation cleanup does not establish authorship of those algorithms.

## Repository guide

| Path | Purpose |
| --- | --- |
| [DIPproject.mlx](DIPproject.mlx) | MATLAB Live Script project material; open in MATLAB to inspect its workflow |
| [FBP-matlab.txt](FBP-matlab.txt) | Additional filtered backprojection material |
| [CBCT_Kyungsang_matlab_Feb2015/](CBCT_Kyungsang_matlab_Feb2015/) | Cone-beam CT simulation and reconstruction examples |
| [ParamSetting.m](CBCT_Kyungsang_matlab_Feb2015/ParamSetting.m) | Volume, detector, scan geometry, filtering, and GPU settings |
| [MeasurementGen.m](CBCT_Kyungsang_matlab_Feb2015/MeasurementGen.m) | Forward projection of the supplied phantom, with optional Poisson noise; saves `proj.mat` |
| [bin/](CBCT_Kyungsang_matlab_Feb2015/bin/) | Projection, backprojection, filtering, and Parker-weighting functions |

## Reconstruction methods

| Script | Method |
| --- | --- |
| [Recon1_FDK.m](CBCT_Kyungsang_matlab_Feb2015/Recon1_FDK.m) | Analytic filtered backprojection (FDK) |
| [Recon2_MLEM.m](CBCT_Kyungsang_matlab_Feb2015/Recon2_MLEM.m) | Maximum-likelihood expectation maximization |
| [Recon3_SART.m](CBCT_Kyungsang_matlab_Feb2015/Recon3_SART.m) | Simultaneous algebraic reconstruction |
| [Recon4_SQS.m](CBCT_Kyungsang_matlab_Feb2015/Recon4_SQS.m) | Separable quadratic surrogates |

## Setup and measurement generation

Clone this fork, open MATLAB, and change into `CBCT_Kyungsang_matlab_Feb2015` before running the scripts. Their data files and `bin` directory are resolved relative to the working directory.

Review `ParamSetting.m` first:

- The supplied volume is configured as 128 × 128 × 128 voxels, with a 256 × 200 detector.
- GPU processing is enabled by default. For a CPU setup, change `param.gpu = 1` to `param.gpu = 0` in that file. GPU execution requires compatible NVIDIA hardware and MATLAB GPU support.
- `MeasurementGen.m` calls `poissrnd` when `NoiseOn = 1`; that function requires Statistics and Machine Learning Toolbox. To generate noise-free measurements, set `NoiseOn = 0` in the script.

Then run:

```matlab
MeasurementGen
```

The script loads the bundled `img128.mat`, computes projections, saves `proj`, `proj_exp`, and `I0` to `proj.mat`, and displays the projections.

The MLEM example reads that generated file:

```matlab
Recon2_MLEM
```

These steps are based on source inspection; they have not been executed in MATLAB during this documentation cleanup.

## Known limitation

The supplied FDK script loads `img128.mat` and calls `filtering(img128, param)`, while the measurement generator references the phantom as `img` and saves projections separately. Verify the MAT-file variable names and the intended projection input before running FDK. This README does not claim that all inherited examples run unchanged.

## Reproducible experiments

When reporting a comparison, record the MATLAB version, toolboxes, CPU/GPU setting, scan geometry, noise setting and seed, iteration count, filter, and evaluation metric. No new reconstruction results or benchmark claims are introduced here.

## Attribution and licensing

Retain upstream notices. The bundled CBCT code includes its own [license.txt](CBCT_Kyungsang_matlab_Feb2015/license.txt); consult that file for its redistribution terms. Do not infer a repository-wide license for other materials from that bundled notice.
