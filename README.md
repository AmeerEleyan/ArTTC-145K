# ArTTC-145K: Arabic Tweet Topic Classification Corpus

ArTTC-145K is a corpus of **145,053 Arabic tweets** in **ten topic categories**, built from 52 topic-focused public accounts on X (formerly Twitter). Each tweet is labeled by its source account, and two annotators reviewed the tweets and removed those inconsistent with their category. It accompanies the paper:

> A. M. Qaroush and A. Eleyan, "A Large-Scale Corpus and Comparative Evaluation of Classical, Static-Embedding, and Transformer Models for Arabic Tweet Topic Classification," *IEEE Access*, 2026 (accepted). DOI: `[DOI]`

The repository contains the corpus, the fixed data splits, the annotation guidelines and annotations, the code and configurations, and the test predictions of all evaluated models.

## Corpus at a glance

| Category | Accounts | Tweets | Mean length (words) |
|---|---|---|---|
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
- **Splits:** stratified by category, 75% / 10% / 15%, giving **108,786 training, 14,510 validation, and 21,757 test** tweets.

## Repository structure

```
ArTTC-145K/
├── data/
│   ├── train.csv                    # text, label
│   ├── validation.csv
│   └── test.csv                     # row order = test_row used in all other files
├── annotation/
│   ├── annotation_guidelines.md     # guidelines given to the annotators
│   ├── sample_200.csv               # the 200 annotated test tweets (20 per category)
│   ├── annotator_A.csv              # independent annotation A
│   ├── annotator_B.csv              # independent annotation B
│   └── adjudicated.csv              # final labels after adjudication by a third annotator
├── predictions/
│   ├── reported_run/
│   │   └── test_predictions.csv     # predictions of the run analyzed in the paper
│   └── five_runs/
│       ├── five_runs_summary.csv    # macro-F1 and accuracy of every run
│       ├── metrics/                 # one JSON per model and seed (metrics, timings, environment)
│       ├── predictions/             # one CSV of test predictions per model and seed
│       └── cost/                    # latency and memory measurements (Table 12)
├── llm_reference/
│   ├── batches/                     # the 40 prompts pasted into ChatGPT-6 (50 tweets each)
│   ├── sample_2000.csv              # the 2,000 test tweets of the LLM reference
│   ├── chatgpt6_answers_raw.txt     # answers as returned
│   └── chatgpt6_predictions.csv     # parsed answers
├── code/
│   ├── preprocessing/
│   ├── models/                      # TF-IDF + SVM, FastText + BiLSTM, AraBERT, MARBERT
│   ├── evaluation/
│   └── configs/                     # selected hyperparameter configurations
├── requirements.txt
├── LICENSE
└── README.md
```

## Data format

Each split file is a UTF-8 encoded CSV file with the columns:

| Column | Description |
|---|---|
| `text` | Preprocessed tweet text (user handles, URLs, and source-identifying hashtags removed) |
| `label` | One of the ten categories |

The labels are **source-based**: each tweet takes the category of the account that published it. In the independent annotation of 200 test tweets, the corpus label was the annotators' main topic for 82.5% of the tweets and their main or secondary topic for 94.0% (inter-annotator Cohen's κ = 0.84 for the main topic).

## Other files

- **Reported run** (`predictions/reported_run/test_predictions.csv`): the test predictions of the run of each model analyzed in the paper (Tables 6–11), with columns `test_row`, `text`, `label`, `svm`, `bilstm`, `arabert`, `marbert`.
- **Five runs** (`predictions/five_runs/`): five training runs of each neural model with seeds 1–5 on the same fixed splits, plus one run of the deterministic TF-IDF + SVM. These are the runs behind the mean ± SD values in Tables 6 and 8.
- **Cost measurements** (`predictions/five_runs/cost/`): `bilstm_latency_breakdown.json` gives the BiLSTM latency reported in the paper (input preparation plus compiled model call). The `*_predict_call.json` files are an earlier measurement dominated by per-call overhead and are kept for completeness.
- **LLM reference** (`llm_reference/`): zero-shot ChatGPT-6, used through the ChatGPT web interface in October 2026 in temporary chats with memory off. Each batch file contains the full prompt and 50 tweets. This is a reference point, not a controlled baseline, and cannot be reproduced exactly.

## Results

Macro-F1 (%) on the test set:

| Model | Reported run | Five runs (mean ± SD) |
|---|---|---|
| TF-IDF + SVM | 91.02 | 91.02 (deterministic) |
| FastText + BiLSTM | 88.75 | 88.54 ± 0.23 |
| AraBERT (`aubmindlab/bert-base-arabertv02`) | 94.34 | 94.03 ± 0.07 |
| MARBERT (`UBC-NLP/MARBERT`) | 94.02 | 93.92 ± 0.12 |

Training and test share source accounts, so these scores measure how well the source-based labels of new tweets from the same accounts are reproduced. They are not estimates for tweets from unseen accounts.

## Reproducing the experiments

```bash
pip install -r requirements.txt
```

Tested with Python 3.13, torch 2.14.1, transformers 5.22.1, tensorflow 2.21.0, scikit-learn 1.6.1, gensim 4.4.0, and numpy 2.4.6 on an NVIDIA A100 GPU.

`[Add the commands for preprocessing, training each model, and evaluation, e.g. python code/models/train_arabert.py --config code/configs/arabert.yaml --seed 1]`

The five runs use seeds 1–5 on the same fixed splits. TF-IDF + SVM is deterministic.

## Intended use and ethics

- **Intended use:** research on Arabic text and topic classification.
- **Source accounts:** all tweets come from public accounts of news organizations, specialized content pages, and public figures listed in the paper. No private accounts were included.
- **No user identifiers:** the released text contains no user handles or URLs.
- **Scope:** the corpus represents predominantly MSA tweets from topic-focused accounts. It does not represent everyday conversational or dialect-rich Arabic.
- **Platform terms:** users are responsible for complying with the X (Twitter) terms of service when using the data.

## License

`[LICENSE, e.g. CC BY 4.0 for the annotations and labels; code under MIT]`

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

Aziz M. Qaroush, Faculty of Engineering and Technology, Birzeit University, Palestine (aqaroush@birzeit.edu)

## Acknowledgments

We thank Dr. Mustafa Jarrar, Mohammad Abu Bader, and Salem Hammad for their assistance with data collection and cleaning.
