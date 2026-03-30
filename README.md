# Music Knowledge Graph

Knowledge Graph construction, alignment, expansion, KGE and RAG over music data.
**ESILV · Web Mining & Semantics · 2025–2026**

---

## Authors

**Lunalor Sandy TEFOUEGOUM & Harold TAGNY**

---

## Project Overview

This project builds a complete Knowledge Graph pipeline over a music domain corpus of 45 artists, from raw data collection to a RAG-powered chatbot.

| Step | Description | Output |
|------|-------------|--------|
| 01 · Crawler + NER | Wikipedia & MusicBrainz APIs + spaCy NER | `textes_crawles.json` |
| 02 · KB Construction | RDF/OWL ontology, 45 artists, 1 741 triples | `private_kb.ttl` |
| 03 · Alignment | owl:sameAs Wikidata + equivalentProperty | `aligned_kb.ttl` |
| 04 · SPARQL Expansion | 5-level expansion to 125k triples | `expanded_kb.nt` |
| 05 · SWRL Reasoning | Inference rules (oldPerson, transitivity) | — |
| 06 · KGE | TransE + RotatE with PyKEEN | `kge_resultats.json` |
| 07 · RAG | NL to SPARQL with Gemma 2B via Ollama | `rag_resultats.json` |

---

## KB Statistics

```
Triples    : 125 028   (target: 50k-200k)
Entities   :  19 768   (target: 5k-30k)
Relations  :     150   (target: 50-200)
Artists    :      45
```

---

## Repository Structure

```
music_knowledge_graph/
│
├── step1_build_private_kb.ipynb       # KB construction
├── step2_3_alignment.ipynb            # Wikidata alignment
├── step4_expansion_final.ipynb        # SPARQL expansion
├── td5_part1_swrl.ipynb               # SWRL reasoning
├── td5_part2_kge.ipynb                # KGE training + evaluation
├── td6_rag.ipynb                      # RAG chatbot
├── crawler_ner.ipynb                  # Crawler + NER
│
├── kg_artifacts/
│   ├── private_kb.ttl                 # Initial KB (1 741 triples)
│   ├── aligned_kb.ttl                 # After Wikidata alignment
│   └── expanded_kb.nt                 # Expanded KB (125k triples)
│
├── data/
│   ├── train.txt                      # KGE training split (56 762)
│   ├── valid.txt                      # KGE validation split (6 187)
│   └── test.txt                       # KGE test split (6 153)
│
├── reports/
│   └── final_report.pdf
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/sandy792/music_knowledge_graph.git
cd music_knowledge_graph
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Download spaCy model

```bash
python -m spacy download en_core_web_sm
```

### 4. Install and start Ollama (for RAG)

Download Ollama from https://ollama.com, then:

```bash
ollama pull gemma:2b
ollama serve
```

---

## How to Run

Run notebooks in order:

```
1. crawler_ner.ipynb
2. step1_build_private_kb.ipynb
3. step2_3_alignment.ipynb
4. step4_expansion_final.ipynb
5. td5_part1_swrl.ipynb
6. td5_part2_kge.ipynb          (requires train.txt / valid.txt / test.txt)
7. td6_rag.ipynb                (requires Ollama running)
```

Note: make sure `ollama serve` is running in a separate terminal before executing `td6_rag.ipynb`.

---

## KGE Results

| Metric | TransE | RotatE |
|--------|--------|--------|
| MRR | 0.2646 | 0.0165 |
| Hits@1 | 0.0994 | 0.0054 |
| Hits@3 | 0.3832 | 0.0137 |
| Hits@10 | 0.5349 | 0.0308 |

Configuration: embedding_dim=128, epochs=50, batch_size=512, CPU only.

---

## RAG Evaluation

| Question | Baseline | RAG | Correct |
|----------|----------|-----|---------|
| Genre de Daft Punk ? | "techno" | electronic, house, french house | RAG correct |
| Label de Radiohead ? | "EMI Records" | Parlophone | RAG correct |
| Membres de Pink Floyd ? | Incomplet | 5 membres exacts | RAG correct |
| Artistes genre electronic ? | Invente | Aucun resultat | Incorrect |
| Qui a influence Nirvana ? | "Kurt Cobain" | Aucun resultat | Incorrect |

3/5 correct (60%). Limited by Gemma 2B size — few-shot prompting required.

---

## Tech Stack

| Tool | Usage |
|------|-------|
| `rdflib` | RDF graph construction and SPARQL queries |
| `owlready2` | SWRL reasoning |
| `pykeen` | KGE training (TransE, RotatE) |
| `spacy` | Named Entity Recognition |
| `ollama` + `gemma:2b` | Local LLM for RAG |
| `matplotlib` / `sklearn` | t-SNE visualization |

---

## GitHub

https://github.com/sandy792/music_knowledge_graph
