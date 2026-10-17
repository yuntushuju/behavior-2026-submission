# Evaluation instructions

This package covers task IDs 0-49, public instances 0-9, rollout 0: 500 original result JSON files and 500 matching unmodified MP4 videos. Tasks 50-99 are not included. Each task has one fixed model for all instances.

## Run

Use OmniGibson v3.9.3-post1 outside the policy container, with the included unchanged robot configuration and RGBDFullResWrapper.

```bash
docker run --rm --gpus '"device=0"' -p 127.0.0.1:8000:8000 ghcr.io/yuntushuju/behavior-2026-runtime@sha256:fa5c2a7afa8d0448d5857e220a06e5dd2545e3e59261b94897d43a829046bd6a --port 8000 --initial-model checkpoint_2
python -m omnigibson.eval.eval --task-name TASK_NAME --instance-indices INSTANCE_INDEX --mode public_test --num-rollouts 1 --host 127.0.0.1 --port 8000 --robot-config ./r1pro.yaml --output-dir ./output --write-video --num-envs 1 --env-wrapper omnigibson.eval.wrappers.RGBDFullResWrapper
```

INSTANCE_INDEX is public index 0-9; raw output instance IDs are 301-310. Use official default seeds, task-dependent maximum steps and scoring. The policy selects the model from the supplied task ID.

## Materials

Download submission.zip and all videos_part_*.tar files from the [release](https://github.com/yuntushuju/behavior-2026-submission/releases/tag/submission-v3-20261017); extract video parts into one directory. results_manifest.json records task/instance identity, fixed runtime model, result/video SHA256 and steps/frames. SHA256SUMS.txt and assets_manifest.json verify the assets. Metrics and videos are unmodified prescribed runs, with no per-instance model selection or best-of-repeat filtering.

The public deployment is intended for a single 24GB policy GPU. Full closed-loop validation of this packaging is pending; file integrity alone is not runtime validation. Simulator/GPU numerical variation may affect scores. Upstream attribution and license notices are in UPSTREAM.md and the image. Publication alone does not establish an official verified score.
