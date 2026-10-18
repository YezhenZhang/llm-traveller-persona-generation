# LLM-based Traveller Persona Generation

## Description

This repository presents a method for extracting traveller archetypes from travel survey data using an LLM. Here, an archetype is called a persona: one short sentence covering socioeconomic and travel behavioural information. The method has two steps. First, an LLM infers personas from the survey records, one persona for one traveller at a time. Second, MinHash removes lexically similar personas, then embeddings remove semantically similar ones, leaving a representative persona set. 

This repository is part of a larger project, described in our [GeoAI 2026 paper](https://doi.org/10.5281/zenodo.20312880). That project builds a persona set from both survey data and mobile in-app data, then uses an LLM to generate individual daily activity trajectories. Because the mobile data are private, the version used in the paper is not released here. Nonetheless, that version is almost the same as this repository. The only difference is that it infers personas from the two datasets, then deduplicates them together.

## Structure

This repository has six parts. `code/` is the implementation file. `requirements.txt` lists the packages to install. `data/` stores the inputs required by the notebook and the outputs it writes. `conference_manuscript.pdf` and `conference_slides.pdf` describe this method and the wider project in full. `figures/` stores the high-resolution images used in the manuscript and the slides.

```text
├── code/
│   ├── persona_generation.ipynb   # main file
│   ├── config.json                # settings
│   └── prompts/
│       ├── system.txt
│       └── nts_persona.txt
├── requirements.txt               
├── data/
│   ├── trip_indi_hh_2021.csv      # input: survey; values are codes
│   ├── lookup_table_response.xlsx # input: lookup table for those codes
│   ├── persona_initial.jsonl      # output: LLM-generated personas, one per person
│   ├── minhash_report.txt         # output: lexical dedup report
│   ├── embedding_report.txt       # output: semantic dedup report
│   └── persona_final.txt          # output: final persona set after dedups
├── conference_manuscript.pdf
├── conference_slides.pdf
└── figures/
```

## Usage

To get started with this repository, follow these steps.

1. In `code/config.json`, fill in your OpenAI API key:

```json
{
  "api_key": "", # your OpenAI key
  "llm_model": "gpt-4o-mini",
  "embedding_model": "text-embedding-3-small",
  "temperature": 0.7,
  "max_tokens": 200,
  "minhash_threshold": 0.9,
  "embedding_threshold": 0.55
}
```

2. Install the packages:

```bash
pip install -r requirements.txt
```

3. Open `code/persona_generation.ipynb` and run the cells from top to bottom.

**Note:** If running this repository gives a persona set slightly different from the paper, that is reasonable. This run uses only the survey data, but the paper infers personas from both survey and mobile data.

## Citation

If you would like to cite our work, please use:

```bibtex
@inproceedings{zhang_generating_2026,
	title = {Generating {Individual}-{Level} {Full}-{Day} {Activity} {Trajectory} {Dataset} via {Large} {Language} {Models}},
	url = {https://zenodo.org/doi/10.5281/zenodo.20312881},
	doi = {10.5281/ZENODO.20312881},
	publisher = {Zenodo},
	author = {Zhang, Yezhen and Zhou, Zhengzi and Pang, Yanbo and Zhong, Chen},
	year = {2026},
}
```

## Acknowledgments

This project has received funding from the European Research Council (ERC) under the European Union’s Horizon 2020 research and innovation programme (grant agreement No 949670), and Proof of Concept (grant agreement No 101212775).

We are also grateful to Dr. Jiawei Wang and his [LLMob](https://github.com/Wangjw6/LLMob) project for the help with our work.
