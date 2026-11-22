# ArTTC-145K: Arabic Tweet Topic Classification Corpus

ArTTC-145K is a corpus of 145,053 Arabic tweets in ten topic categories, built from 52 topic-focused public accounts on X (formerly Twitter). Each tweet is labeled by its source account, and two annotators reviewed the tweets and removed those inconsistent with their category.

It accompanies the paper:

> A. M. Qaroush and A. Eleyan, "A Large-Scale Corpus and Comparative Evaluation of Classical, Static-Embedding, and Transformer Models for Arabic Tweet Topic Classification," *IEEE Access*, 2026 (accepted). DOI: [DOI]

The repository contains the corpus, the fixed data splits, the code and configurations, and the test predictions of all evaluated models.

## Corpus at a glance

| Category | Accounts | Tweets | Mean length (words) |
|---|---:|---:|---:|
| Politics | 2 | 16,200 | 13.2 |
| Economy | 3 | 15,066 | 16.4 |
| Religion | 10 | 13,018 | 24.4 |
| Sports | 4 | 15,356 | 11.5 |
| Health | 6 | 14,087 | 16.5 |
| Technology | 5 | 15,423 | 18.2 |
| Science | 4 | 13,297 | 17.4 |
| Cars | 3 | 14,683 | 14.8 |
| Cooking | 4 | 11,483 | 13.6 |
| Thoughts and Wisdom | 11 | 16,440 | 18.1 |
| **Total** | **52** | **145,053** | **16.3** |

- **Language:** predominantly Modern Standard Arabic (95% of an independently annotated sample of 200 tweets).
- **Collection period:** approximately one month in 2022.
- **Splits:** stratified by category, 75% / 10% / 15%, giving 108,786 training, 14,510 validation, and 21,757 test tweets.

## Repository structure

```text
ArTTC-145K/
├── data/
│   ├── train.csv              # id, text, label
│   ├── validation.csv
│   └── test.csv
├── code/
│   ├── preprocessing/         # text cleaning pipeline
│   ├── models/                # TF-IDF + SVM, FastText + BiLSTM, AraBERT, MARBERT
│   ├── evaluation/            # metrics, bootstrap intervals, significance tests
│   └── configs/               # selected hyperparameter configurations
├── requirements.txt
├── LICENSE
└── README.md
```

## Data format

Each split file is a UTF-8 encoded CSV with the following columns:

| Column | Description |
|---|---|
| `id` | Tweet identifier within the corpus |
| `text` | Preprocessed tweet text (user handles, URLs, and source-identifying hashtags removed) |
| `label` | One of the ten categories |

The labels are source-based: each tweet takes the category of the account that published it. In the independent annotation of 200 test tweets, the corpus label was the annotators' main topic for 82.5% of the tweets and their main or secondary topic for 94.0% (inter-annotator Cohen's κ = 0.84 for the main topic).

## Results

Macro-F1 (%) on the test set:

| Model | Reported run | Five runs (mean ± SD) |
|---|---:|---:|
| TF-IDF + SVM | 91.02 | 91.02 (deterministic) |
| FastText + BiLSTM | 88.75 | 88.54 ± 0.23 |
| AraBERT (`aubmindlab/bert-base-arabertv02`) | 94.34 | 94.03 ± 0.07 |
| MARBERT (`UBC-NLP/MARBERT`) | 94.02 | 93.92 ± 0.12 |

> **Note:** Training and test share source accounts, so these scores measure how well the source-based labels of new tweets from the same accounts are reproduced. They are not estimates for tweets from unseen accounts.

## Reproducing the experiments

```bash
pip install -r requirements.txt
```

Tested with Python 3.13, torch 2.14.1, transformers 5.22.1, tensorflow 2.21.0, scikit-learn 1.6.1, gensim 4.4.0, and numpy 2.4.6 on an NVIDIA A100 GPU.

<!-- TODO: add the commands for preprocessing, training each model, and evaluation, e.g.:
python code/models/train_arabert.py --config code/configs/arabert.yaml --seed 1
-->

The five runs use seeds 1–5 on the same fixed splits. TF-IDF + SVM is deterministic.

## Intended use and ethics

- **Intended use:** research on Arabic text and topic classification.
- **Source accounts:** all tweets come from public accounts of news organizations, specialized content pages, and public figures listed in the paper. No private accounts were included.
- **No user identifiers:** the released text contains no user handles or URLs.
- **Scope:** the corpus represents predominantly MSA tweets from topic-focused accounts. It does not represent everyday conversational or dialect-rich Arabic.
- **Platform terms:** users are responsible for complying with the X (Twitter) terms of service when using the data.

## License

[LICENSE, e.g. CC BY 4.0 for the annotations and labels; code under MIT]

## Citation

```bibtex
@article{qaroush2026arttc,
  author  = {Qaroush, Aziz M. and Eleyan, Ameer},
  title   = {A Large-Scale Corpus and Comparative Evaluation of Classical,
             Static-Embedding, and Transformer Models for Arabic Tweet
             Topic Classification},
  journal = {IEEE Access},
  year    = {2026},
  doi     = {[DOI]}
}
```

## Contact

Ameer Eleyan, [email], Faculty of Engineering and Technology, Birzeit University, Palestine.

## Acknowledgments

We thank Dr. Mustafa Jarrar, Mohammad Abu Bader, and Salem Hammad for their assistance with data collection and cleaning.
