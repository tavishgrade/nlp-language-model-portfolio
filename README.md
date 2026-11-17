# NLP Language Model Portfolio

**Tavish Grade** · M.S. Engineering, Artificial Intelligence (Virginia Commonwealth University, in progress)

Hands-on notebooks from a graduate course in Advanced Language Processing (CMSC 516). Each one builds a piece of the modern NLP stack from the ground up: counting words, parsing structure, measuring meaning, attention, and evaluating LLMs. Every notebook follows the same loop: **predict → build → measure → find where it breaks → verify the AI's claims against primary sources.**

I came to NLP from nearly a decade in film sound (Emmy and C.A.S. Award–winning Foley mixing) and a Cognitive Science / HCI degree from UC San Diego. So the running example word through these notebooks, *bass* (the fish or the instrument?), felt like home.

---

## Notebooks

| # | Notebook | What I built | Key result |
|---|---|---|---|
| 1 | [Lexical: Bigram LM & Collocations](Talktorial_1_Lexical_TavishGrade.ipynb) | Tokenized *Alice in Wonderland*; built a bigram language model with Laplace smoothing; ranked collocations by PMI vs. log-likelihood | Held-out perplexity of **515** smoothed vs. **∞** unsmoothed. Stopword filtering turned noisy PMI pairs into real story collocations (*mock turtle*, *march hare*, *white rabbit*) |
| 2 | [Syntactic: Dependency Parsing](Talktorial_2_Syntactic_TavishGrade.ipynb) | Parsed sentences with spaCy; tested PP-attachment, a garden-path sentence and domain shift; compared an LLM's parse with spaCy's | spaCy attached *with a fork* and *with meatballs* to the same word, misread *"The old man the boats"*, and mislabeled tweet slang (*omg* → NOUN) |
| 3 | [Semantics: WordNet & WSD](Talktorial_3_Semantics_student_TavishGrade.ipynb) | Explored synsets; computed path, Wu-Palmer, Resnik, Lin and JCN similarity; ran Lesk disambiguation; extended it to my own field's vocabulary (movie, audio, microphone) | *dog/cat* vs. *dog/car*: the gap grows from 2.6× (path) to 5–6× (information-content measures). Lesk picked the wrong sense of *bass* in both test sentences, and I traced why |
| 4 | [Embeddings: Meaning as Vectors](Talktorial_4_Embeddings_TavishGrade.ipynb) | Queried GloVe (400k words); tested analogies, including ones built to fail; trained my own word2vec on a Gutenberg corpus | *bass*'s top 15 neighbors are all music, yet the fish sense is still in the vector (first fish word at rank 19; *bass + fishing − guitar* → fishermen, trout). The corpus decides what *king* means |
| 5 | [Attention from Scratch & BERT's 144 Heads](Talktorial_5_Attention_TavishGrade.ipynb) | Implemented scaled dot-product attention, self-attention and causal masking in NumPy; extracted every attention head of `bert-base-uncased` and wrote a head "census" | Without √d scaling the softmax saturates (max weight **0.998** at d = 64 vs. 0.32 scaled). Early BERT heads send *bass* → *river* or → *plucked/song* by context. **72 of 144** heads park on `[SEP]`/`[CLS]`, and only 6 are positional. Cross-checked against Clark et al. (2019) |
| 6 | [Sentiment: Where Each Method Breaks](Talktorial_6_Sentiment_TavishGrade.ipynb) | Tracked BERT's contextual vector for *bass* layer by layer; trained Naive Bayes and logistic regression on Pang & Lee reviews; compared them with VADER and a zero-shot LLM (Qwen2.5-0.5B); built an adversarial "break set" | Fish-vs-music similarity falls from **0.97 → 0.55** across BERT's layers. Logistic regression reached **85.2%** (published baseline 82.9%). On 40 reviews: LR 95%, zero-shot LLM 75%, VADER 47.5%. 23 sarcasm, negation and domain cases show where each method fails |
| 7 | [Information Extraction & Knowledge Graph](Talktorial_7_Information_Extraction_TavishGrade.ipynb) | Clinical NER with a news model, a biomedical model and a zero-shot LLM; wrote exact/partial-match P/R/F1 scoring; added a grounding check for hallucinated entities; entity linking; relation extraction into a `networkx` graph | The news model labels *Lisinopril* as a PERSON. Zero-shot LLM partial F1 was **0.78** vs. **0.67** for the biomedical model, with 0 of 25 entities invented. Linking brand to generic names (*Accupril → quinapril*) made a multi-hop query answerable, and upstream NER errors erased 12 of 12 gold graph edges |

*Coming next:* retrieval-augmented generation (chunking, dense + TF-IDF retrieval, reciprocal rank fusion, recall@k / MRR). Then the course final project, **Reading the Line**: emotion classification of dialogue, from written conversation (DailyDialog) to performed TV dialogue (MELD), comparing a TF-IDF classifier against a retrieval-augmented few-shot LLM.

---

## The verification habit

Each notebook ends with an **AI Verification Log**. I asked an assistant (ChatGPT or Copilot) a real question, then checked its answer against the textbook (Jurafsky & Martin, *Speech and Language Processing*, 3rd ed.), the library docs, the research paper, or my own output. Each check gets a verdict (✅ correct · ⚠️ needs a caveat · ❌ wrong) and the evidence.

The most common verdict was ⚠️. The AI was usually right in outline and missed the caveat that mattered: the corpus dependence of information-content similarity, the `[SEP]`-parking heads, a parser's labeled-attachment error. One of my logs is a clean ❌: the LLM predicted what Lesk would choose based on what *made sense*, not on what the algorithm actually computes.

## Tools

Python · Jupyter / Google Colab · NLTK · spaCy · gensim · scikit-learn · NumPy · PyTorch · Hugging Face Transformers · sentence-transformers · networkx · matplotlib

Models used: GloVe (`glove-wiki-gigaword-100`), word2vec (self-trained), `bert-base-uncased`, `d4data/biomedical-ner-all`, `Qwen2.5-0.5B-Instruct`

## Running the notebooks

Each notebook runs top to bottom in Google Colab on a CPU (no GPU needed). The setup cell installs anything missing. Notebooks 5–7 download BERT (~440 MB) and/or Qwen (~1 GB) on first run. Each notebook also writes a small JSON artifact of its key outputs, which feeds into the course final project.

## About me

Technical operations and audio technology professional moving into AI evaluation and product work. I currently run AV and unified-communications projects at Virginia Housing, and before that I spent years as a Foley mixer (Arcane, Barbie, Avatar: The Way of Water, Dune: Part Two).

[LinkedIn](https://www.linkedin.com/in/tavish-grade/) · [IMDb](https://www.imdb.com/name/nm5243369/) · [Sound portfolio](https://tavishgrade.wixsite.com/soundportfolio)
