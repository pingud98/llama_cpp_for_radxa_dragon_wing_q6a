# Q6A (Radxa Dragon Wing) Build Artifacts

Cross-compiled llama.cpp with Hexagon NPU backend for Qualcomm QCS6490.

## Structure

- `bin/llama-cli` -- ARM64 llama-cli binary, cross-compiled with Hexagon DSP support
- `dsp/` -- DSP-side FastRPC skeleton libraries (deployed to `/usr/lib/rfsa/adsp/`)
  - `libggml-htp-v68.so` -- main HTP v68 DSP library
  - `libggml-htp-v68-nostart.so` -- variant without startup code
  - `libggml-htp-v68-test.so` -- minimal test skeleton
  - `libhtp_minimal_skel.so` -- stripped-down skeleton for debugging FastRPC loading
- `lib/` -- Host-side Hexagon backend libraries
  - `libggml-hexagon.so.0.9.11` -- Hexagon backend shared library (CPU-side)
  - `libggml-htp-v68.so` -- copy of DSP library for reference

## Status

DSP library loading fails with error 0xe (AEE_EUNSUPPORTED) from `remote_handle64_open`.
FastRPC transport itself works (calculator test passes). Root cause suspected: DSP runtime
library mismatches or URI/skel entry point mismatch.
