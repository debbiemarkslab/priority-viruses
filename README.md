# Mutation Effect Prediction Across Priority Viruses

This is the official code repository for the paper: ["Sequence-based protein models for the prediction of mutations across priority viruses"](https://openreview.net/pdf?id=DvC6VL7TJK) from the [Marks Lab](https://www.deboramarkslab.com/).

## Overview
Advances in machine learning for predicting mutation effects have enhanced viral surveillance and enabled the proactive design of vaccines and therapeutics, but the accuracy of these methods across priority viruses remain unclear. We perform the first large-scale modeling across 40 WHO priority pandemic-threat pathogens, many of which are under-surveilled, discovering that most have sufficient sequence or structural information for effective modeling, highlighting the potential for using these approaches in pandemic preparedness. To understand the limits of current modeling capabilities for viruses, we curate 45 standardized viral deep mutational scanning assays to systematically evaluate the performance of different models, using different training databases. 

We find marked differences in performance of these models on viruses relative to non-viral proteins. For viral proteins, we find alignment-based models perform on par with PLMs though with predictable differences in which model is better for a particular function or virus depending on data available. We define confidence metrics for both alignment-based models and PLMs that indicate when additional sequence or structural data may be needed for accurate predictions and to guide model selection in the absence of available data for evaluation. We use these metrics to inform the development a confidence-weighted hybrid model that builds on the strength of each approach, adapts to the quality of data available, and outperforms either of the best alignment or PLM models alone.

## Data
The [viral DMS substitutions](https://github.com/debbiemarkslab/priority-viruses/tree/main/data/viral_dms_substitutions) folder contains 45 curated and standardized viral deep mutational scans (DMS), listed in [reference file](https://github.com/debbiemarkslab/priority-viruses/blob/main/data/reference_files/viral_dms_reference.csv). The [viral DMS structures](https://github.com/debbiemarkslab/priority-viruses/tree/main/data/viral_dms_structures) folder contains AlphaFold3 structures of all of the base sequences. The sequences and structures are used as inputs to the models below.

To model the 40 priority and prototype RNA viral pathogens from the [WHO](https://cdn.who.int/media/docs/default-source/consultation-rdb/prioritization-pathogens-v6final.pdf?sfvrsn=c98effa7_7&download=true), sequence and folded structures are also collected. 

## Models
Our analysis includes models from the following papers.

**Alignment-based Models**:
Model name | Input modalities | Training Database | Reference | Github
--- | --- | --- | --- | -- |
Site Independent | MSA | Uniref90, Uniref100 or Uniref100+BFD+MGnify | [Hopf, T.A., Ingraham, J., Poelwijk, F.J., Schärfe, C.P., Springer, M., Sander, C., & Marks, D.S. (2017). Mutation effects predicted from sequence co-variation. Nature Biotechnology, 35, 128-135.](https://www.nature.com/articles/nbt.3769) | [EVcouplings](https://github.com/debbiemarkslab/EVcouplings)
EVmutation | MSA | Uniref90, Uniref100 or Uniref100+BFD+MGnify | [Hopf, T.A., Ingraham, J., Poelwijk, F.J., Schärfe, C.P., Springer, M., Sander, C., & Marks, D.S. (2017). Mutation effects predicted from sequence co-variation. Nature Biotechnology, 35, 128-135.](https://www.nature.com/articles/nbt.3769) | [EVcouplings](https://github.com/debbiemarkslab/EVcouplings)
EVE | Alignment-based model | Uniref90, Uniref100 or Uniref100+BFD+MGnify | [Frazer, J., Notin, P., Dias, M., Gomez, A.N., Min, J.K., Brock, K.P., Gal, Y., & Marks, D.S. (2021). Disease variant prediction with deep generative models of evolutionary data. Nature.](https://www.nature.com/articles/s41586-021-04043-8) | [EVE](https://github.com/OATML-Markslab/EVE)

**Protein Language Models**:
Model name | Input modalities | Training Database | Reference | Github
--- | --- | --- | --- | -- |
ESM-1v (ensemble) | Single sequence | Uniref90 |[Meier, J., Rao, R., Verkuil, R., Liu, J., Sercu, T., & Rives, A. (2021). Language models enable zero-shot prediction of the effects of mutations on protein function. NeurIPS.](https://proceedings.neurips.cc/paper/2021/hash/f51338d736f95dd42427296047067694-Abstract.html) | [ESM](https://github.com/facebookresearch/esm)
Tranception (without retrieval) | Single sequence | Uniref100 | [Notin, P., Dias, M., Frazer, J., Marchena-Hurtado, J., Gomez, A.N., Marks, D.S., & Gal, Y. (2022). Tranception: protein fitness prediction with autoregressive transformers and inference-time retrieval. ICML.](https://proceedings.mlr.press/v162/notin22a.html) | [Tranception](https://github.com/OATML-Markslab/Tranception)
VESPA | Single sequence | BFD+Uniref50 | [Marquet, C., Heinzinger, M., Olenyi, T., Dallago, C., Bernhofer, M., Erckert, K., & Rost, B. (2021). Embeddings from protein language models predict conservation and variant effects. Human Genetics, 141, 1629 - 1647.](https://link.springer.com/article/10.1007/s00439-021-02411-y) | [VESPA](https://github.com/Rostlab/VESPA)
SaProt (AF2 and PDB 650M) | Single sequence & structural tokens (Foldseek) | AF2DB or AF2DB+PDB | [Jin Su, Chenchen Han, Yuyang Zhou, Junjie Shan, Xibin Zhou, Fajie Yuan. (2024). SaProt: Protein Language Modeling with Structure-aware Vocabulary. ICLR](https://www.biorxiv.org/content/10.1101/2023.10.01.560349v5) | [SaProt](https://github.com/westlake-repl/SaProt)

We also report a new hybrid model that combines alignment-based EVE and structural-aware PLM SaProt with reliability estimation (EVEREST).

**Hybrid Models**:
Model name | Input modalities | Training Database | Reference | Github
--- | --- | --- | --- | -- |
TranceptEVE | MSA | Uniref100 | [Notin, P., Van Niekerk, L., Kollasch, A., Ritter, D., Gal, Y. & Marks, D.S. &  (2022). TranceptEVE: Combining Family-specific and Family-agnostic Models of Protein Sequences for Improved Fitness Prediction. NeurIPS, LMRL workshop.](https://www.biorxiv.org/content/10.1101/2022.12.07.519495v1?rss=1) | [TranceptEVE](https://github.com/OATML-Markslab/ProteinGym/blob/main/notebooks/TranceptEVE_example.ipynb)
EVEREST | MSA and structural tokens (Foldseek) | Uniref90, Uniref100 or Uniref100+BFD+MGnify and AF2DB or AF2DB+PDB | This work | This work


## Results
The [results](https://github.com/debbiemarkslab/priority-viruses/blob/main/results/) folder contains model scores for mutation effects across all viral DMS assays for each alignment-based and protein language model as well as reported Spearman correlations between models and experiments. Confidence metrics are also reported for both alignment-based models and SaProt, and are used to create a hybrid model. Hybrid model mutation effect predictions are made for the antigens of each WHO priority virus.

## Reproducability
The code for training these models and for mutation effect scoring is available through [ProteinGym](https://github.com/OATML-Markslab/ProteinGym).

## Acknowledgements

Special thanks to the teams of experimentalists who developed and performed the viral DMS assays this work is built on. If you are using these assays in your work, please cite the corresponding papers. To facilitate this, details of each paper is included in the [DMS reference file](https://github.com/debbiemarkslab/priority-viruses/blob/main/data/reference_files/viral_dms_reference.csv).

## License
This project is available under the MIT license. 

## Reference
