# Changelog

Notable changes to the VANTAGE-Bench harness. The newest release is first.

Versions follow [Semantic Versioning](https://semver.org/).

## [1.1.0] - 2026-10-06

More models, more ways to run them, and a smoother path from a fresh clone to a valid submission.

### Added

- **Native Gemini support.** `GeminiFlash3-6` and `GeminiFlash3-5-Lite` are registered, and `GEMINI_API_KEY` is accepted alongside `GOOGLE_API_KEY`.
- **Box coordinate conventions.** A model declares the order of its 2D boxes with `box_coord_order` (`xyxy` or `yxyx`). The order is stored in the submission metadata and the grounding, Astro2D and SOT evaluators read it.
- **Agentic skills.** `skills/` holds eleven playbooks that let a coding agent run the full pipeline: preflight, data preparation, model configuration, inference, validation, packaging and the portal form.
- **Submission tooling.** `scripts/preflight_check.py` reports environment readiness, `scripts/validate_submission.py` checks a submission before upload, and `scripts/run_manifest.py` records run provenance and drafts the portal form fields.
- **Public dataset layout.** `scripts/run_lmudata.py` reads the released `anonymous/VANTAGE-Bench` layout, including both published layouts for EventVerification and 2DPointing. `--dry-run` confirms that each task's files resolve before any download.
- **Run summary and exit code.** `run.py` prints a status table for every model and dataset combination and exits 1 when a combination did not complete. `--allow-partial-failures` keeps exit 0.
- **Packaging from any run mode.** `scripts/package_submission.py` accepts the submission files written by `--mode all`, `--mode eval` and `--mode infer`, and prints the file it chose for each task.
- **`VANTAGE_2DPointing` as a `--data` key.**
- **More timestamp formats.** Temporal Localization accepts numeric values and `ss`, `mm:ss` and `hh:mm:ss` strings. Dense Video Captioning accepts unit suffixes (`4.72s`), compound durations (`1m5s`), comma decimals, `start - end` ranges and `hh:mm:ss:ff` timecodes.
- **Test suite.** `tests/` covers parsing, packaging, inference handling and the dataset keys named in the README.

### Improved

- **Faster DVC scoring.** BERTScore runs on the job's GPU when one is present: about 20 seconds instead of about 20 minutes for a 104-video submission on a V100. `VANTAGE_BERT_DEVICE` sets the device explicitly.
- **Long DVC predictions are kept in full.** Predictions pass to the evaluator without the 32,767-character spreadsheet cell limit.
- **Single Object Tracking ids.** Sequence ids from the public dataset map to the canonical form, so all 200 sequences join the ground truth. Every sequence reports the same metric set.
- **Inference continues past a sample that raises.** The sample is recorded, the count is reported at the end, and `VANTAGE_FAIL_FAST=1` stops at the first one instead.
- **Qwen video sampling.** Clips shorter than the requested frame count are sampled at their full length, and the video reader is pinned for current torchvision releases.
- **Lighter installs.** The `hipho`, `sarena_mini` and `uni_svg` datasets are optional, so the package imports without their dependencies.
- **Config mode** accepts pre-sampled video datasets such as `VANTAGE_SOT`, and Astro2D follows the `--lmudata-root` override.

### Documentation

- Reorganised README with a news section, a supported-models table and collapsible reference material, plus this changelog.
- Installation guidance for dependency and compiler setup.
- Developer guide, seven sample configs, the submission guides and the prompt guide.
- Metric names, key code paths and dataset layouts in the docs match the code.

## [1.0.0] - 2026-05-27

The first public release of the VANTAGE-Bench harness, alongside the leaderboard.

### Added

- Eight tasks in four pillars: Video QA, Event Verification, Referring Expressions, Spatial Pointing, Object Localization, Temporal Localization, Dense Video Captioning and Single Object Tracking.
- Dataset loaders, prompts and output parsers for each task, built on VLMEvalKit.
- Submission emitter that writes one JSONL file per task for server-side scoring.
- Cosmos model wrappers for local, vLLM and API inference.

