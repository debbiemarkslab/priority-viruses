# Mutation Effect Prediction Across Priority Viruses 

This is the official code repository for: ["Variant effect prediction with reliability estimation across priority viruses"](https://www.biorxiv.org/content/10.1101/2025.08.04.668549v1) from the [Marks Lab](https://www.deboramarkslab.com/).
<p align="center">
  <img width="300" height="300" alt="EVEREST_logo" src="https://github.com/user-attachments/assets/daf85430-92ef-402a-a0a1-4c31d2fa1fe8" />
</p>

## Overview
Viruses pose a significant threat to global health due to their rapid evolution, adaptability, and increasing potential for cross-species transmission. While advances in machine learning and the growing availability of sequence and structure data offer promise for large-scale mutation effect prediction, viruses present unique biological and informational constraints that may challenge these models. To quantify this, we introduce EVEREST—a framework for Evolutionary Variant Effect prediction with Reliability ESTimation—which assesses model performance on mutation effect prediction using a curated benchmark of 45 viral deep mutational scanning datasets (over 340 thousand variants) and develop reliability metrics to quantify model uncertainty in the absence of experimental data. This large-scale evaluation revealed wide differences in prediction accuracy across models and viral families. Contrary to findings on non-viral proteins, we find that protein language models trained on diverse sequence corpora under-perform on viral proteins compared to alignment-based models trained on a much smaller set of homologous sequences. We apply this framework across 40 WHO-prioritized pandemic-threat viruses (over 400 thousand variants across 16 viral families), and discover that state-of-the-art models fail to reliably predict mutations in over half of these viruses. Our findings uncover key factors leading to under-performance, offer actionable recommendations for improving viral mutation effect prediction, and provide an objective framework for analyzing dual-use biosecurity risk.

## Data
The [viral DMS substitutions](https://github.com/debbiemarkslab/priority-viruses/tree/main/data/viral_dms_substitutions) folder contains 45 curated and standardized viral deep mutational scans (DMS), listed in [reference file](https://github.com/debbiemarkslab/priority-viruses/blob/main/data/reference_files/viral_dms_reference.csv). The [viral DMS structures](https://github.com/debbiemarkslab/priority-viruses/tree/main/data/viral_dms_structures) folder contains AlphaFold structures of all of the base sequences. The sequences and structures are used as inputs to the models below.

To model the 40 priority and prototype RNA viral pathogens from the [WHO](https://cdn.who.int/media/docs/default-source/consultation-rdb/prioritization-pathogens-v6final.pdf?sfvrsn=c98effa7_7&download=true), sequence and folded structures are also collected of the antigens. 

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
SaProt (AF2 and PDB 650M) | Single sequence & structural tokens (Foldseek) | AF2DB or AF2DB+PDB | [Jin Su, Chenchen Han, Yuyang Zhou, Junjie Shan, Xibin Zhou, Fajie Yuan. (2024). SaProt: Protein Language Modeling with Structure-aware Vocabulary. ICLR](https://www.biorxiv.org/content/10.1101/2023.10.01.560349v5) | [SaProt](https://github.com/westlake-repl/SaProt)

We also report a new hybrid model that combines alignment-based EVE and structural-aware PLM SaProt (SaProt-EVE) and produces reliability estimates, and compare to existing hybrid models.

**Hybrid Models**:
Model name | Input modalities | Training Database | Reference | Github
--- | --- | --- | --- | -- |
VESPA | Single sequence | BFD+Uniref50 | [Marquet, C., Heinzinger, M., Olenyi, T., Dallago, C., Bernhofer, M., Erckert, K., & Rost, B. (2021). Embeddings from protein language models predict conservation and variant effects. Human Genetics, 141, 1629 - 1647.](https://link.springer.com/article/10.1007/s00439-021-02411-y) | [VESPA](https://github.com/Rostlab/VESPA)
Tranception (with MSA retrieval) | MSA | Uniref100 | [Notin, P., Dias, M., Frazer, J., Marchena-Hurtado, J., Gomez, A.N., Marks, D.S., & Gal, Y. (2022). Tranception: protein fitness prediction with autoregressive transformers and inference-time retrieval. ICML.](https://proceedings.mlr.press/v162/notin22a.html) | [Tranception](https://github.com/OATML-Markslab/Tranception)
TranceptEVE | MSA | Uniref100 | [Notin, P., Van Niekerk, L., Kollasch, A., Ritter, D., Gal, Y. & Marks, D.S. &  (2022). TranceptEVE: Combining Family-specific and Family-agnostic Models of Protein Sequences for Improved Fitness Prediction. NeurIPS, LMRL workshop.](https://www.biorxiv.org/content/10.1101/2022.12.07.519495v1?rss=1) | [TranceptEVE](https://github.com/OATML-Markslab/ProteinGym/blob/main/notebooks/TranceptEVE_example.ipynb)
SaProt-EVE | MSA and structural tokens (Foldseek) | Uniref90, Uniref100 or Uniref100+BFD+MGnify and AF2DB+PDB | This work | This work


## Results
The [results](https://github.com/debbiemarkslab/priority-viruses/blob/main/results/) folder contains model scores for mutation effects across all viral DMS assays for each alignment-based and protein language model as well as reported Spearman correlations between models and experiments. Confidence metrics are also reported for both alignment-based models and SaProt, and are used to create a hybrid model EVEREST. Hybrid model mutation effect predictions are made for the antigens of each WHO priority virus.

## Reproducability
The code for training these models and for mutation effect scoring is available through [ProteinGym](https://github.com/OATML-Markslab/ProteinGym).

## Acknowledgements

Special thanks to the teams of experimentalists who developed and performed the viral DMS assays this work is built on. If you are using these assays in your work, please cite the corresponding papers. To facilitate this, details of each paper is included in the [DMS reference file](https://github.com/debbiemarkslab/priority-viruses/blob/main/data/reference_files/viral_dms_reference.csv).

## License
This project is available under the MIT license. 

## Reference
Sarah Gurev*, Noor Youssef*, Navami Jain, Debora S. Marks. Variant effect prediction with reliability estimation across priority viruses. *BioRxiv*, 2025.

(* equal contribution)
