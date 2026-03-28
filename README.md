# Music Knowledge Graph

Knowledge Graph construction, alignment, expansion, KGE and RAG over music data.

## Domain
Music — 45 artists covering Pop, Rock, Jazz, Hip-Hop, Electronic, Chanson française.

## Project Structure
```
notebooks/          # Jupyter notebooks for each step
kg_artifacts/       # RDF files (TTL, NT, JSON)
reports/            # Final report PDF
README.md
requirements.txt
.gitignore
```

## How to Run

### 1. Install dependencies
```
pip install rdflib requests owlready2 pykeen torch scikit-learn matplotlib spacy
python -m spacy download en_core_web_sm
```

### 2. Run each module in order
- Step 1 : `step1_build_private_kb.ipynb`
- Step 2/3 : `step2_3_alignment.ipynb`
- Step 4 : `step4_expansion_final.ipynb`
- TD5 Part 1 : `td5_part1_swrl.ipynb`
- TD5 Part 2 : `td5_part2_kge.ipynb`
- TD6 RAG : `td6_rag.ipynb`
- Crawler + NER : `crawler_ner.ipynb`

### 3. Run the RAG demo
Start Ollama first:
```
ollama serve
ollama pull gemma:2b
```
Then run `td6_rag.ipynb` and execute the CLI demo cell.

## Hardware Requirements
- RAM : 8 GB minimum
- CPU : all models trained on CPU (no GPU required)
- Storage : ~500 MB for KG files + models

## KB Statistics
- Triplets : 125,028
- Entities : 19,768
- Relations : 150
- Artists : 45

## Models Used
- KGE : TransE, RotatE (PyKEEN)
- LLM : Gemma 2B (Ollama)
- NER : spaCy en_core_web_sm
