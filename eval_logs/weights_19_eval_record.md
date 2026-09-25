# CVCDepth DDAD Baseline Evaluation

## Experiment
Name: repro_cvcdepth_ddad_single_gpu
Date: 2026-09-25 12:18:21 +0800

## Code
Branch: fix_ddad_reproduction
Commit: 9e39688560fd254c77263a523dbfe9831983d9ca
Git status:
?? Dockerfile.cvcdepth.repro
?? cache/
?? configs/ddp/repro_cvcdepth_ddad_single_gpu_smoke.yaml
?? eval_logs/
?? requirements.cvcdepth.runtime.txt
?? requirements.repro.txt

## Environment
Docker image: cvcdepth:repro
GPU: NVIDIA RTX 4000 SFF Ada Generation
CUDA: 11.3
PyTorch: 1.12.0+cu113
Python: 3.8.10

## Dataset
Dataset: DDAD
Dataset path inside container: /data/ddad
Validation timestamps: 3950
Cameras: 6
GT depth files verified: 23700 / 23700
GT depth source: LiDAR
GT layout: depth/lidar/CAMERA_xx/<timestamp>.npz
GT NPZ key: depth

## Training
Config: configs/ddp/repro_cvcdepth_ddad_single_gpu.yaml
Epochs: 20
Batch size: 1
Learning rate: 0.0001
Resolution: 384x640
Seed: 42
DDP: disabled
Final checkpoint:
results/repro_cvcdepth_ddad_single_gpu/models/weights_19/depth_net.pth

## Evaluation Protocol
Eval batch size: 4
Eval workers: 8
Min depth: 0 m
Max depth: 200 m
Post-processing: OFF
Overlap evaluation: OFF
Checkpoint: weights_19

## Native Metric Depth
AbsRel: 0.227
SqRel: 3.726
RMSE: 13.922
logRMSE: 0.353
a1: 0.656
a2: 0.842
a3: 0.919

## Median-Scaled Diagnostic
AbsRel: 0.218
SqRel: 3.503
RMSE: 13.330
logRMSE: 0.329
a1: 0.679
a2: 0.865
a3: 0.934

## Evaluation Command
docker run --rm \
  --gpus all \
  --shm-size=16g \
  -i \
  --user "$(id -u):$(id -g)" \
  -v "$PWD":/workspace \
  -v "$PWD/cache/torch":/tmp/.cache/torch \
  -v "$HOME/mcde/Dataset/DDAD/ddad_train_val":/data/ddad:ro \
  -w /workspace \
  -e HOME=/tmp \
  -e MPLCONFIGDIR=/tmp/matplotlib \
  -e PYTHONDONTWRITEBYTECODE=1 \
  -e PYTHONPATH="/workspace:/workspace/external/packnet_sfm:/workspace/external/dgp" \
  cvcdepth:repro \
  python eval.py \
  --config_file configs/ddp/repro_cvcdepth_ddad_single_gpu.yaml \
  --weight_path /workspace/results/repro_cvcdepth_ddad_single_gpu/models/weights_19

## Notes
- Training completed successfully for 20 epochs.
- Final depth checkpoint loaded successfully.
- DDAD GT coverage verified before evaluation.
- Dataset compatibility fix required:
  - depth/<camera> -> depth/lidar/<camera>
  - NPZ key arr_0 -> depth
- No model, loss, training, or metric implementation was changed for evaluation.
- Native metric-depth result is the primary baseline.
- Median-scaled result is diagnostic only.
