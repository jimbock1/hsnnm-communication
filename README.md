Is Communication All You Need? Quality and Wall-Clock Costs of Sparse Lobe Routing

Code, run records and paper for a follow-up to the HSNNM paper (Tjuandra, 2026, Zenodo DOI 10.5281/zenodo.21358950). It compares a dense transformer FFN against HSNNM (top-1 lobe routing, k-WTA, Hebbian strengths) and HSNNM with a TALK reliability gate, on two character-level corpora.

Headline results (validation perplexity, character level)
Corpus	Dense	HSNNM	TALK
TinyStories (10% train split)	2.1900	2.6463	2.6418 (mean of 3 seeds, range 0.0751)
Wikipedia	3.1087	3.4731	3.4554

HSNNM reduces FLOPs by 49.6% (TinyStories) and 36.3% (Wikipedia) but is 20.8% / 11.7% higher in perplexity than dense, and is slower in wall-clock time (about 2.0x / 2.4x dense) on the tested hardware. Dense baselines are single runs. See the paper for caveats.

Layout
Folder	Contents
HSNNM/	Model, config, data, training and evaluation code for dense and HSNNM
TALK/	TALK gate, trainer (train.py), profiling notes (PROFILE.md), tests
scripts/	Run queues, Wikipedia fetch, sampling, per-position perplexity, figure script
results/	Per-run result JSON files and logs used in the paper
logs/	Console logs
paper/	LaTeX source, figures and PDF
data/	Empty; the corpus is downloaded here (not included)

TALK/train.py expects the HSNNM/ and TALK/ folders to be siblings, as they are here.

Reproducing

Experiments ran on an AMD RX 9060 XT, Windows, PyTorch 2.9.1+rocm7.2.1, eager mode. HSNNM/requirements.txt pins a different torch version; use the one above to match the paper.

TinyStories runs (one model and one seed per run, from the TALK/ folder; see TALK/README.md for all options):

cd TALK
python train.py --model dense --seed 0
python train.py --model hsnnm --seed 0
python train.py --model talk --seed 0 --gate-init-std 1.0

Wikipedia runs and figures (from the repo root):

python scripts/fetch_wiki.py          # downloads the Wikipedia corpus into data/
python scripts/run_queue_v3.py        # Wikipedia queue (TALK, HSNNM, dense)
python scripts/make_paper_figures.py results paper

Note: the path edits that make the scripts relative to the repo root were made after the experiments and the scripts have been compile-checked but not re-run end to end. Please open an issue if something breaks.

AI assistance

The code and paper were written with AI assistance (several AI tools). The ideas were created and directed by the author and inspired by prior work. See the paper's AI section for details.

Licences
Code: MIT (see LICENSE).
Paper (paper/): CC BY 4.0.
Citation

Tjuandra, K. (2026). Is Communication All You Need? Quality and Wall-Clock Costs of Sparse Lobe Routing. DOI: https://doi.org/10.5281/zenodo.24557618 (see also CITATION.cff).
