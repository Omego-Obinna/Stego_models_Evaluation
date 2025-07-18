# Stego_models_Evaluation
Part 1
# Hybrid Multichannel Steganography

This repository contains the implementation of unified embedding and extraction algorithms for both CMO (Cover Modification) and Hybrid steganography models, as described in *Multichannel Steganography: A Provably Secure Hybrid Steganographic Model*. The directory structure and usage instructions are provided below.

---

## Repository Structure

```
├── cmo_stego_adaptive/            # Stego images output by CMO adaptive embedding
├── cover_images/                  # Cover images for CMO embedding
├── cover_messages/                # Cover images/messages for Hybrid embedding
├── cover_messages_hybrid/         # Alternate cover messages for Hybrid
├── Hybrid_stego_images/           # Stego images output by Hybrid embedding
├── CSVs/                          # Metric CSVs (results)
├── unified_embed_2.py             # Unified embedding script (CMO + Hybrid)
├── unified_extract_well.py        # Unified extraction script (CMO + Hybrid)
├── myenv/                         # Python virtual environment directory
└── secret_messages.txt            # Sample secret messages (one per line)
```

---

## Prerequisites

1. **Python 3.8+**
2. Virtual environment (recommended):

   ```bash
   python3 -m venv myenv
   source myenv/bin/activate
   ```
3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

   *If no `requirements.txt` is provided, install manually:*

   ```bash
   pip install opencv-python numpy pandas matplotlib seaborn scikit-image
   ```

---

## Embedding

The `unified_embed_2.py` script performs both CMO adaptive embedding and Hybrid adaptive embedding in one pass. It reads from:

* `cover_images/` + `secret_messages.txt` for CMO embedding (outputs to `cmo_stego_adaptive/`).
* `cover_messages/` + fixed key for Hybrid embedding (outputs to `Hybrid_stego_images/`).

### Usage

```bash
python unified_embed_2.py
```

This script will:

1. Embed secrets into the CMO stego set and save to `cmo_stego_adaptive/`.
2. Embed secrets into the Hybrid stego set and save to `Hybrid_stego_images/`.
3. Compute and save embedding imperceptibility metrics (PSNR, SSIM) as CSV files.
4. Generate diagnostic plots (`*.png`) in the working directory.

---

## Extraction

The `unified_extract_well.py` script runs adversarial extraction for both CMO and Hybrid models. It reads from:

* `cmo_stego_adaptive/` + `cover_images/` for CMO extraction.
* `Hybrid_stego_images/` + `cover_messages/` for Hybrid extraction.

### Usage

```bash
python unified_extract_well.py
```

This script will:

1. Extract bit-vectors using variance-aware LSB extraction.
2. Unmask Hybrid payloads via brute-force key search over a small keyspace (e.g., 8-bit candidates).
3. Compute extraction performance metrics (BER, correlation).
4. Save results to CSV (e.g., `cmo_adaptive_extraction_results.csv`, `hybrid_adaptive_extraction.csv`).

---

## Metrics & Outputs

* **Embedding Metrics**:

  * `cmo_stego_results_adaptive.csv`
  * `hybrid_stego_results_adaptive.csv`

* **Extraction Metrics**:

  * `cmo_adaptive_extraction_results.csv`
  * `hybrid_adaptive_extraction.csv`

* **Plots**:

  * `cmo_adaptive_psnr.png`, `cmo_adaptive_ssim.png`
  * `hybrid_adaptive_psnr.png`, `hybrid_adaptive_ssim.png`

---

## Notes

* Ensure `secret_messages.txt` contains at least as many lines as there are cover images.
* For larger keyspaces in Hybrid extraction, adjust the candidate key generator in `unified_extract_well.py`.
* To reproduce results exactly, use the provided virtual environment or replicate the `requirements.txt`.

---

## License

This code is provided under the MIT License. See `LICENSE` for details.
