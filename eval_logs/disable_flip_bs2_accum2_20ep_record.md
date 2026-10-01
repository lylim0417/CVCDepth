# CVCDepth DDAD Experiment Record

## Experiment
repro_cvcdepth_ddad_single_gpu_disable_flip_bs2_accum2_20ep

## Objective
Evaluate CVCDepth on DDAD with flip augmentation disabled and micro-batch size increased to 2.

## Training
Dataset: DDAD
Resolution: 384x640
Epochs: 20
GPU: 1
DDP: disabled
Micro batch size: 2
Gradient accumulation: 2
Effective optimizer batch: 4
Learning rate: 0.0001
Scheduler: StepLR(step_size=15, gamma=0.1)
Flip augmentation: disabled
Seed: 42

## Checkpoint
results/repro_cvcdepth_ddad_single_gpu_disable_flip_bs2_accum2_20ep/models/weights_19/depth_net.pth

## Evaluation Protocol
Eval batch size: 4
Max depth: 200 m
Post-process: OFF
Overlap: OFF
GT source: DDAD LiDAR
GT files verified previously: 23700 / 23700

## Native Metric Depth
AbsRel: 0.210
SqRel: 3.510
RMSE: 12.866
logRMSE: 0.330
a1: 0.704
a2: 0.874
a3: 0.934

## Median-Scaled Diagnostic
AbsRel: 0.210
SqRel: 3.416
RMSE: 12.673
logRMSE: 0.317
a1: 0.708
a2: 0.882
a3: 0.940

## Interpretation
This is the best verified result so far.

Relative to the previous verified baseline, two training variables changed:
- flip augmentation: v5 -> disabled
- micro-batch/accumulation: 1x4 -> 2x2

Therefore the result should not be attributed solely to disabling flip.

The flip implementation will not be repaired in this experiment series.
