# Supplementary Material for Review — Access-2026-30102

**"Microsecond-Latency, Fully On-Chip Limit Order Book Prediction on an Edge FPGA:
A Lightweight INT8 1D-CNN Hardware–Software Co-Design"**

This package provides evidence supporting the manuscript's claims
(data protocol, hardware synthesis/place-and-route, on-board verification, quantized
parameters). 
---

## Provided (for the reviewers)

| Folder | Contents |
|--------|----------|
| `1_qat_config/` | `hyperparams_table3.md` — full QAT/training hyperparameter table (optimizer, lr, epochs, seed, dropout, etc.) | 
| `2_model_weights/` | Trained parameters: `variant_k3_f144_tail_CF9.pth` (FP32), `best_qat_pcin_CF9_f144.pth` (QAT), `int8_params_CF9_f144_pcin.npz` (fixed-point), `results_int8_pcin_CF9_f144.json` |
| `3_hls_code/` | Per variant: `weights.h` (exported INT8 weight arrays), `lob_tb.cpp` (HLS **test bench** with golden I/O), `hls_config.cfg`, `vitis-comp.json`  | 
| `4_dataset_manifest_and_leakage/` | `dataset_manifest_CF1..9.{csv,json}`, `leakage_check_CF1..9.log`, `test_set_access_audit_step{2,3}.log`   | 
| `5_normalization/` | `NORMALIZATION.md` (causal Z-score, Eq. 7) | 
| `6_seeds/` | `SEEDS.md` (3407 / 2026 / 5-seed set) | 
| `7_board_test/` | `board_manifest.csv` (2,000 on-board sample IDs), `board_golden.npz` (golden outputs), `board_paired_results.csv` (FP32/INT8/board pairs), `board_test_144.py` (host **test** driver), `board_test_provenance.log` | 
| `8_postroute_reports/` | Vivado routed `timing_summary` / `utilization_placed` / `power_routed` reports for the 5 feasible Table-7 variants on xcku040-ffva1156-2-e | 


---

## Notes
- **Main fold** CF_9 (train days 1–9, test day 10): training pool 325,069 samples,
  test set 31,828; purge/embargo T+k = 110 ticks at every boundary.
- **Deployed model:** 144-dim input, K=3, 86,211 parameters; FP32 Macro-F1 0.8509 →
  INT8 (per-channel input, weight-folded) 0.8331.
- **Board:** ALINX AXKU041 (xcku040-ffva1156-2-e), 100 MHz, JTAG-to-AXI.
- All `weights.h` across the 8 HLS variants are identical (same INT8 parameters);
  variants differ only in loop folding / stream count.
