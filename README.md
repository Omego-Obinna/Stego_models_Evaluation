# Stego_models_Evaluation
The file is very large: 
📦 [Download `Adversary Extraction Algorithms`](https://drive.google.com/uc?export=download&id=1t_I-Kxy-SHl9BZ7AhGhkOToNcbt1N4-C)
 
Part 1
# Adversarial Extraction Algorithms (Part 1)

This repository contains the implementation of unified embedding and extraction algorithms for both CMO (Cover Modification) and Hybrid steganography models, as described in *Multichannel Steganography: A Provably Secure Hybrid Steganographic Model for Secure Communication*. The directory structure and usage instructions are provided below.

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
   Code imports for both `unified_embed.py` and `unified_extract.py`, here is the **complete list of Python packages** that these two scripts depend on:

   ### ✅ Core and Standard Library Packages
   
   These are included with Python by default:
   
   * `os`
   * `time`
   * `hashlib`
   * `re`
   
   ### 📦 Third-Party Python Packages
   
   These must be installed via `pip`:
   
   | Package                                                                              | Purpose                                                                        |
   | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
   | `opencv-python` (`cv2`)                                                              | Reading, writing, and manipulating image files (for LSB embedding/extraction). |
   | `numpy`                                                                              | Array manipulation and bit-wise operations.                                    |
   | `pandas`                                                                             | Data logging, results management, and CSV/table handling.                      |
   | `matplotlib`                                                                         | Visualization of results (e.g., BER, PSNR, SSIM).                              |
   | `seaborn`                                                                            | Statistical plotting for presentation and analysis.                            |
   | `scikit-image` (used as `from skimage.metrics import structural_similarity as ssim`) | Image quality metric (SSIM).                                                   |
   
   ---
   
   ### 📦 Suggested `pip install` command:
   
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
Part 1
# Adversarial Extraction Algorithms (Part 2)

This repository contains the implementation of unified embedding and extraction algorithms for both CSY (Cover Synthesis) and CSE (Cover Selection), as described in *Multichannel Steganography: A Provably Secure Hybrid Steganographic Model for Secure Communication*. The directory structure and usage instructions are provided below.

Here's a polished **`README.md` instructions block** for your GitHub repository, tailored to the adversarial unified embedding and extraction setup for **CSY (Cover Synthesis)** and **CSE (Cover Selection)**. This includes directory structure, dependencies, and script usage.

---

## 🕵️‍♂️ Adversarial Unified Embedding and Extraction for CSY and CSE

This repository provides a unified framework for adversarial embedding and extraction using **Cover Synthesis (CSY)** and **Cover Selection (CSE)** steganographic models, with evaluation support for bit-error rate (BER), correlation, PSNR, and extraction success.

---

### 📁 Directory Structure

```
.
├── cover_library/             # Image pool for CSE cover selection
├── cover_images/              # Base images used for synthesis or visual comparison
├── cse_stego/                 # Stego images generated using Cover Selection
├── csy_stego/                 # Stego images generated via Cover Synthesis (e.g. StyleGAN2)
├── csv_files/                 # Output CSVs logging metrics per run (BER, correlation, etc.)
├── plot_after_embedding/      # Evaluation plots: imperceptibility (PSNR, SSIM)
├── plots_after_extraction/    # Extraction performance plots: BER, correlation, success rate
├── stego_images/              # Final stego images from both pipelines
├── stylegan2-pytorch/         # Pre-trained GAN model and related architecture for CSY
├── myenv/                     # Python virtual environment (optional local setup)
├── resnet_secret_regressor.pth  # Trained regression model (ResNet18) for CSY secret extraction
├── unified_embedding.py       # Embeds secrets using CSY or CSE schemes
├── unified_extraction.py      # Adversarial extraction routine for both CSY and CSE
├── resnet_regression_train.py # Script to train the regression model for CSY extraction
```

---

### ⚙️ Setup Instructions

1. **Clone the repository and navigate into it:**

   ```bash
   git clone https://github.com/your-username/hybrid-csy-cse-stego.git
   cd hybrid-csy-cse-stego
   ```

2. **Create and activate virtual environment (optional but recommended):**

   ```bash
   python -m venv myenv
   source myenv/bin/activate  # On Windows: myenv\Scripts\activate
   ```

3. **Install required packages:**

   ```bash
   pip install opencv-python numpy pandas matplotlib seaborn scikit-image torch torchvision
   ```

4. **Download and place your pretrained StyleGAN2 weights** (if using CSY) and the trained regression model `resnet_secret_regressor.pth` into their respective folders.

---

### 🚀 Usage Instructions

#### 1. Unified Embedding

Run the following to embed secrets using either **CSY** or **CSE**:

```bash
python unified_embedding.py --mode [csy|cse] --input_dir cover_library/ --output_dir csy_stego/ --secret_file secrets.txt
```

Options:

* `--mode`: `csy` for synthesis using StyleGAN, `cse` for cover selection.
* `--input_dir`: Path to base or candidate images (used in `cse`).
* `--secret_file`: File containing plaintext secrets (one per line).
* `--output_dir`: Where stego images will be saved.

#### 2. Unified Extraction (Adversary Simulation)

Run adversarial extraction against both models:

```bash
python unified_extraction.py --mode [csy|cse] --stego_dir csy_stego/ --model_path resnet_secret_regressor.pth --output_csv csv_files/extract_metrics.csv
```

Options:

* `--mode`: `csy` or `cse`
* `--stego_dir`: Folder of stego images to extract from.
* `--model_path`: Only required for `csy` — pre-trained ResNet regression head.
* `--output_csv`: Location to save extracted results and metrics.

#### 3. (Optional) Train Regression Model for CSY

```bash
python resnet_regression_train.py --train_dir csy_stego/ --secrets_csv train_labels.csv --epochs 100 --output_model resnet_secret_regressor.pth
```

---

### 📊 Output

After running embedding and extraction:

* Visual plots are saved under `plot_after_embedding/` and `plots_after_extraction/`.
* Extraction results include BER, correlation, PSNR, and recovery success rates.

---

### 🔍 Citation

If you use this framework for publication, please cite:

```
@article{omego2025hybrid,
  title={Multichannel Steganography: A Provably Secure Hybrid Steganographic Model},
  author={Omego, Obinna and Bosy, Michal},
  journal={arXiv preprint arXiv:2401.XXXXX},
  year={2025}
}
```

---

## License

This code is provided under the MIT License. See `LICENSE` for details.
