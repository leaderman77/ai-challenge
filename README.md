# AI Challenge: Trait Ontology Mapping

This project provides a comprehensive solution for evaluating the correctness of gene-trait mappings extracted by LLM models using data stored in a Trait Ontology (TO) vector database. The goal is to assess the alignment of trait names to their corresponding TO IDs and propose a systematic approach to correct errors. The approach combines traditional string matching techniques with modern NLP methods to ensure accurate trait identification and correction.

## Table of Contents
- [Data](#data)
- [Methodology](#methodology)
- [Results](#results)
  - [Model Performance Analysis](#model-performance-analysis)
  - [Prediction of TO Mappings](#prediction-of-to-mappings)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Data
The project utilizes the following data:

- Three CSV files containing LLM model outputs including `trait_name` and `trait_id` mappings
- `trait_ontology_details.txt` containing official trait definitions, synonyms, and hierarchical relationships

The ontology entries include:
- Trait ID
- Trait name 
- Definition
- Synonyms
- Hierarchical relationships ('is_a')  
- Obsolescence ('is_obsolete')

## Methodology 
### Develop a Correctness Metric
The solution implements a hierarchical matching system with multiple validation levels:

1. Exact Matching
2. Synonym & Partial Matching
3. String Similarity Matching (SequenceMatcher)
4. Semantic Matching with Mini LM L6
5. Semantic Matching with BERT
6. Synonym Matching + Semantic Matching with Mini LM L6
7. Synonym Matching + Semantic Matching with BERT
8. Ensemble Matcher

These methods are applied to evaluate the correctness of the trait mappings in the input CSV files. 

### Rank CSVs Based on Correctness
The correctness scores are then aggregated to rank the overall quality of the mappings in each file.

### Propose a Method to Correct Errors
Error analysis is performed to identify common mismatch patterns and areas for improvement. A BERT-based semantic similarity approach is proposed to systematically correct the errors.

### Prediction of TO Mappings for Given Trait Terms
The prediction of trait ontology mappings was approached using two distinct methods:

1. **Domain-Specific Model:** Utilized the domain-specific Hugging Face model `cambridgeltl/SapBERT-from-PubMedBERT-fulltext`, optimized for biomedical and domain-specific embeddings.
2. **OpenAI GPT-4 Embeddings:** Leveraged OpenAI's GPT-4 model with embeddings generated using the `text-embedding-ada-002` (3072-dimensional).

Apply your methods to predict TO mappings for a set of challenging trait terms that do not have exact matches in TO. The aim is to provide a well-structured output for a manual data curation step, helping to reduce the time required for manual review.

## Results
### Model Performance Analysis
| Model   | Total Entries | Matched Entries | Non-Matched Entries | Original Accuracy | BERT Accuracy | Combined Accuracy |
|---------|---------------|-----------------|---------------------|-------------------|---------------|-------------------|
| Model 1 | 113           | 112             | 1                   | 0.974             | 0.944         | 0.974             |
| Model 2 | 104           | 103             | 1                   | 0.962             | 0.944         | 0.961             |
| Model 3 | 94            | 75              | 19                  | 0.897             | 0.944         | 0.907             |

### Prediction of TO Mappings
The prediction of trait ontology mappings was approached using two distinct methods:

1. **Domain-Specific Model:** Utilized the domain-specific Hugging Face model `cambridgeltl/SapBERT-from-PubMedBERT-fulltext`, optimized for biomedical and domain-specific embeddings.
2. **OpenAI GPT-4 Embeddings:** Leveraged OpenAI's GPT-4 model with embeddings generated using the `text-embedding-ada-002` (3072-dimensional).

The results from these models were combined to provide a more nuanced understanding of model preferences and further improve the final predictions. The output CSV file contains the following columns:
- `trait_term`
- `to_id`
- `to_term`
- `confidence`
- `explanation`

## Usage
1. Install the required dependencies listed in `requirements.txt`
2. Run the Jupyter notebook `trait_ontology_mapping.ipynb` to reproduce the analysis
3. Modify the notebook to experiment with different matching strategies and parameters

## Contributing
Contributions are welcome! Please follow these steps:
1. Fork the repository
2. Create a new branch for your feature or bug fix
3. Commit your changes and push to your fork
4. Open a pull request describing your changes

## License
This project is licensed under the MIT License. See `LICENSE` for more information.
