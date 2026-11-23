# ArTTC-145K: Annotation Guidelines

These are the guidelines given to the annotators of the independent annotation sample described in the paper (Section IV-D). They are reproduced as given, with formatting adapted for this repository.

## 1. Independent annotation (annotators A and B)

### Task

For each Arabic tweet, record:

1. its **main topic**;
2. a **secondary topic**, if it clearly has one;
3. its **language variety**.

The purpose is to check how accurately the corpus labels describe the tweets, and how much of the corpus is written in Modern Standard Arabic (MSA). There are no right answers to guess: we need your own judgment.

### Rules

- Work alone. Do not discuss the tweets with the other annotator, and do not look up the account, the corpus label, or any model output.
- Judge each tweet from its text only.
- Fill in the main topic, secondary topic, and language variety for every tweet, using the drop-down lists. The Notes column is optional.
- If the topic cannot be determined from the text, choose the closest category and write "unclear" in Notes.
- Annotate the tweets in order. Both annotators must stop at the same tweet. Expected time: about 20 seconds per tweet.

The sheet given to the annotators contained 500 tweets, with the first 200 as the minimum. Both annotators completed the first 200 (20 per category), which form the released sample.

### The ten categories

| Category | Arabic | What it covers | How to decide in borderline cases |
|---|---|---|---|
| Politics | سياسة | Government and state affairs: elections, officials and their statements, diplomacy and international relations, conflicts and security, laws and political decisions. | An economic decision by a government → Economy if the tweet is mainly about its economic effects. |
| Economy | اقتصاد | Markets and money: companies and their results, stock markets, prices and inflation, oil and energy markets, trade, banking, investment, employment, economic policy. | Technology companies' products → Technology; their business results → Economy. |
| Religion | دين | Islamic religious content: Qur'anic verses, hadith, supplications (دعاء), fatwas and rulings, worship and religious occasions (prayer, fasting, Hajj), sayings of scholars about faith and worship. | A religious text (verse, hadith, supplication) → Religion as main topic, even if it is phrased as a saying; add Thoughts and Wisdom as secondary topic if it is also a general reflection. |
| Sports | رياضة | Matches, results, line-ups, players, clubs, coaches, transfers, tournaments and championships. | — |
| Health | صحة | Human health: diseases and symptoms, treatments and medicines, medical advice, nutrition for health, fitness and weight, mental health, skin and body care, public health. | Mainly a scientific discovery or research finding → Science; mainly a recipe or dish → Cooking. |
| Technology | تكنولوجيا | Devices and digital products: smartphones and computers, apps and software, the internet and social media platforms, artificial intelligence, video games, tech companies' products and features, cybersecurity. | Scientific research behind a technology → Science. |
| Science | علوم | Scientific knowledge and discovery: research findings, space and astronomy, physics and chemistry, biology, animals and nature, earth, environment and climate, general scientific facts. | If the main point is a person's health or a treatment → Health. |
| Cars | سيارات | Cars and driving: car models, specifications, prices, reviews, the automotive industry and motor shows, maintenance and driving tips. | — |
| Cooking | طهي | Food preparation: recipes, dishes, ingredients, cooking methods, kitchen tips. | If the main point is the health effect of a food → Health (add Cooking as secondary topic). |
| Thoughts and Wisdom | خواطر واقتباسات | A genre rather than a subject: personal reflections, aphorisms, sayings and quotations, poetry, motivational or emotional messages, life advice. | A general reflection or quotation → Thoughts and Wisdom, even if it mentions God briefly; a religious text → Religion (see above). |

### Secondary topic

Choose a secondary category only if the tweet also clearly belongs to it, for example a tweet on the health benefits of a fruit: main topic Health, secondary topic Cooking. Otherwise choose "None".

### Language variety

| Value | Meaning |
|---|---|
| MSA | Modern Standard Arabic throughout (the language of news and formal writing). |
| Dialect | Mainly a regional dialect, e.g., words such as وش، شلون، ايش، مو، بدي، هلأ، دحين، عشان. |
| Mixed | Substantial MSA and dialect in the same tweet. |

### Example of a completed row (illustration only; not one of the annotated tweets)

| Tweet | Main topic | Secondary topic | Language variety | Notes |
|---|---|---|---|---|
| تحافظ على صحة الجهاز الهضمي وتقلل مستويات السكر في الدم: إليكم فوائد فاكهة الكاكا | Health | Cooking | MSA | fruit + health benefits |

## 2. Adjudication (third annotator)

The tweets on which annotators A and B disagreed were resolved by a third annotator under these rules:

- Decide from the text of the tweet and the category guidelines above only. Do not look up the account, the corpus label, or any model output.
- If one annotator's label is clearly an entry slip (e.g., a physics tweet labeled Sports), correct it.
- If both labels are defensible, apply the borderline rules above to choose the main topic, and record the other category as the secondary topic.
- Labels on which the annotators agreed are kept unchanged.

## 3. Released files

| File | Content |
|---|---|
| `sample_200.csv` | The 200 annotated test tweets: `id`, `test_row` (row index in the test set), `text`, `corpus_label` |
| `annotator_A.csv`, `annotator_B.csv` | The independent annotations: `id`, `main_topic`, `secondary_topic`, `language_variety` |
| `adjudicated.csv` | Final labels after adjudication, same columns |

Agreement on the main topic between annotators A and B: Cohen's κ = 0.84. Against the adjudicated labels, the corpus label is the main topic of 82.5% of the tweets and the main or secondary topic of 94.0%.
