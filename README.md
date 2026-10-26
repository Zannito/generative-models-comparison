Empirical Comparison of Generative Models on Fashion-MNIST
Course project for Deep Learning (D6421) at BINUS University. The project studies image generation on Fashion-MNIST and compares six generative-model approaches, alongside an MLP-classifier experiment that illustrates the difference between classification and generation.

Models and experiments
The project includes these model families:
1. Vanilla Autoencoder (AE) — reconstructs input images through a learned latent representation.
2. Sparse Autoencoder — adds a sparsity penalty to the latent representation.
3. Variational Autoencoder (VAE) — learns a probabilistic latent space and supports sampling from that space.
4. Vanilla GAN — trains a generator and discriminator adversarially.
5. Deep Convolutional GAN (DCGAN) — uses convolutional layers in the generator and discriminator.
6. Denoising Diffusion Probabilistic Model (DDPM) — learns to predict noise added to training images over diffusion steps.
An MLP classifier is also defined for the classification-versus-generation limitation experiment. The notebook is retained as an exploratory and reporting artifact; the src/generative_models/ package contains the modular implementation.

Requirements
- Python 3.10–3.12 recommended.
- PyTorch and torchvision.
- NumPy and Matplotlib.
- Jupyter Notebook.
- A CUDA-capable GPU is optional. The code selects CUDA when available and otherwise uses CPU; CPU training may take considerably longer.
Setup
1. Clone the repository
Replace YOUR-USERNAME with the GitHub username that owns the repository:
git clone https://github.com/Zannito/generative-models-comparison.git
cd generative-models-comparison
2. Create a virtual environment
Windows PowerShell:
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
macOS/Linux:
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
3. Install dependencies
pip install -r requirements.txt
For GPU acceleration, install the PyTorch/torchvision build compatible with your operating system and CUDA setup using the official PyTorch installation selector, then install the remaining packages from requirements.txt if needed. The package versions selected by the PyTorch installer should be mutually compatible.
Run the modular training code
From the repository root, with the virtual environment activated:

Windows PowerShell:
$env:PYTHONPATH = "src"
python scripts/train_all.py
macOS/Linux:
PYTHONPATH=src python scripts/train_all.py
The script downloads Fashion-MNIST through torchvision if it is not already available, prints the selected device and dataset sizes, trains the six generative models, and saves model checkpoints and training history under outputs/. Training time depends on hardware.
Evaluate reconstruction models
After training has completed:

Windows PowerShell:
$env:PYTHONPATH = "src"
python -m generative_models.evaluate
macOS/Linux:
PYTHONPATH=src python -m generative_models.evaluate
This evaluation script reports MSE, PSNR, and inference time for Vanilla AE, Sparse AE, and VAE, then writes outputs/evaluation_metrics.json. It does not currently calculate FID or evaluate all six models with every metric.
Export sample grids
After training, for example:
python -m generative_models.generate --model vanilla_ae --n 64
The generation script supports vanilla_ae, sparse_ae, vae, gan, and dcgan. It saves the sample grid to figures/. DDPM sample-grid export is not implemented in this script yet. Run it from the repository root with PYTHONPATH=src configured as shown above.
Run the notebook
From the repository root:
jupyter notebook notebooks/main.ipynb
Run cells in order. The notebook may duplicate some functionality available in the modular Python package; use it for notebook-specific experiments, comparisons, and plots.
Reproducibility and experiment settings
Default settings in src/generative_models/config.py include:
- Random seed: 42
- Batch size: 128
- Latent dimension for AE/Sparse AE/VAE: 64
- Noise dimension for GAN/DCGAN: 100
- Epochs: 5 for AE-family models, GANs, and DDPM
- Diffusion steps: 100
- Device: CUDA if available, otherwise CPU
These are the configured defaults, not a guarantee of identical results across machines. Record the software versions, hardware, runtime, and actual outputs used in the final report. Do not report placeholder or illustrative values as experimental results.

The modular data loader reserves the official Fashion-MNIST test set for testing and splits the official training set into training and validation subsets. Check the notebook's split logic separately before comparing notebook results with modular-script results; keep the data protocol consistent for any reported comparison.
Outputs and version control
The training script writes checkpoints and training_history.json under outputs/. Evaluation writes evaluation_metrics.json; generated sample grids are written under figures/. Check .gitignore before committing results. Avoid committing downloaded datasets, virtual environments, caches, or large checkpoints unless the instructor explicitly requires them.
Academic references and integrity
Fashion-MNIST is used as the image dataset. Cite the dataset, PyTorch/torchvision, and any external sources or implementations referenced in the report. Explain any changes made to the original notebook and report only results obtained from actual experiment runs.
