A compilation of software packages for single cell clonal inference, tumor phylogenetics, and DNA-barcode-based lineage tracing. I've divided this list into sections for each type  and included a note of a package uses a hybrid approach or is multi-functional.

This list is a work in progress. If you'd like to add to this list, feel free to submit a pull request or submit an issue.



## Software packages

### Clonal inference and clonal trees
* [cardelino](https://github.com/single-cell-genetics/cardelino)
    * Bayesian clustering of cells to infer clonal tree. 
  
* [SBMClone](https://github.com/raphael-group/SBMClone)
    * clustering of cells that share groups of SNVs. Utilizes a stochastic block model to overcoem sparsity.
  
* [SiCloneFit](https://bitbucket.org/hamimzafar/siclonefit/src/master/) - (2019 paper)[http://www.genome.org/cgi/doi/10.1101/gr.243121.118]
    * Utilizes a hierarchical bayesian model to simultaneously cluster cells into clones and perform phylogenetic inference between clones with an MCMC and partial Gibbs sampling. Since clone sizes may change through sampling, 
    * input: Matrix of 3 genotype states, homozygous ref, heterozygous, homozygous nonref. 
    * interesting features: SiCloneFit uses a doublet model, which is able to assign a doublet to two distinct clones.
 
 * [BitPhylogeny](http://markowetzlab.org/software/BitPhylogeny.php) - (2019 paper)[https://doi.org/10.1186/s13059-015-0592-6]
     * BitPhylogeny uses a non-parametric bayesian clustering method to assign cells to clones and infers the evolutionary relationship between clones by using sequences of mutations. Clone assignment is optimized with a tree structured stick-breaking process, which is essentially a mixture model of hierarchical components. 
     * Unlike *bona fide* phylogenetic methods, taxa (clones) aren't limited to leaves, so internal nodes do not represent common ancestors and can represent observed states/living taxa.
     
* oncoNEM
 
### Maximum likelihood phylogenetic methods

* [CellPhy](https://github.com/amkozlov/cellphy) - [2020 preprint](https://www.biorxiv.org/content/10.1101/2020.07.31.230292v1)
  * an implementation of RAxML-NG that was optimized for single cell tree inference by including parameters for allelic dropout and sequencing/PCR errors. CellPhy uses a mMrkov substitution model that follows the finite sites model, which is reported to work well under both finite sites and infinite sites assumptions.
  * Since CellPhy was built on RAxML, it offers many features that are lacking in other single cell phylogenetic software. For Example, CellPhy can utilize genotype likelihoods of variants, it offers a wide variety of input options and models, it can perform fast tree searches as well as thorough bootstrapped analyses, and from the bootstrap it can compute leaf-level measure of confidence.
  * Note: The authors provide a caveat that CellPhy requires hundreds to thousands of mutations for high accuracy trees in realistic datasets. However, I suspect this is true for all single cell ML methods, but the uncertainty of trees from other published methods is unknown since most do not provide measure of uncertainty and benchmarking is typically performed on synthetic data.
  * Input: genotype matrix with 2-10 states, distance matrices, PAML files, multiple sequence alignments.

* [SCARLET](github.com/raphael-group/scarlet) [2020 preprint](https://doi.org/10.1016/j.cels.2020.04.001)
  * k-Dollo implementation to allow LOH under the finite sites hypothesis. Constructs joint phylogenetic tree from CNA and SNV data.
  * input: A matrix of read counts for each SNV and a course copy number tree file. CNV tree input is optional since it can be inferred from the read counts, however, this is computationally expensive for datasets with high CN heterogeneity or frequent small CNVs.
  
* [SPhyR](https://github.com/elkebir-group/SPhyR) - [2018 paper](10.1093/bioinformatics/bty589)
  * Uses a k-Dollo phylogeny model (an SNV can be gained once, but lost k times) to allow LOH events. 
  * Input is a binary matrix.
  
* SiFit
    * SiFit implements a Markov finite-site model and a heuristic ML tree search.

* [SCITE](https://gitlab.com/jahnka/SCITE) - [C]
  * Produces rooted *mutation trees* using MCMC. In case of mutation tree, SCITE can assign cells to the most likely leaf to infer clones.
  * Uses sequencing error rate as prior.
  * input is a quaternary matrix representing unmutated, heterozygous, homozygous, dropout.
  
* [∞SCITE](https://github.com/cbg-ethz/infSCITE)
  * an implementation of SCITE that relaxes the infinite sites assumption by providing a model for recurrent mutations. *I think* this is to allow parallel or convergent evolution and does not model LOH or backmutations. In addition, it identifies doublets.

  
### Distance methods
* [DENDRO](https://github.com/zhouzilu/DENDRO) - [R]
    * A very lean R package to compute maximum parsimony trees from expressed SNVs in scRNA-seq data
    * DENDO implements a distance kernel based on a beta-binomial model which they suggest mitigates the impact of differential expression on tree topology.
* [MEDALT](https://github.com/KChen-lab/MEDALT) [2020 preprint](https://www.biorxiv.org/content/10.1101/2020.04.12.038281v1.full)
    * Uses large copy number alterations inferred from arm-level under- and over-expression in scRNA-seq (based on methods like inferCNV or CONICSmat) to compute the minimal event distance between cells with a greedy algorithm. From this distance matrix, it uses Chu-Liu/Edmond's algorithm to infer a rooted directed minimal spanning tree.
    * With the ever increasing size of single cell datasets, the computational speed of Edmond's algorithm is attractive. However, (without having tried this method) I would assume that the greedy nature of this method would lead to many false positives and missegregations. It is also worth noting that the graph requires that cells be placed on internal nodes, so this is more like a directed nearest neighbor graph than phylogenetic tree.
    
* TNT

## prospective lineage tracing with synthetic reporters

### phylogenetic approaches

* [GAPML](https://github.com/matsengrp/gapml) - [2019 preprint](https://www.biorxiv.org/content/10.1101/595215v1.full)
    * Builds subtrees based on Camin-Sokol parsimony and iterates pruning and grafting to assemble highest likelihood tree.
 
* [Cassiopeia](www.github.com/YosefLab/Cassiopeia) - 
    * Cassiopeia includes five phylogenetic algorithms, a greedy multi-state compatibility algorithm, an exact Steiner-tree solver, a hybrid of those two, Neighbor-Joining, and Camin-Sokal Maximum Parsimony.
    
### Tree Visualizaton
  
  * [ETE3](https://github.com/etetoolkit/ete) - python
  * [ggtree](https://guangchuangyu.github.io/software/ggtree/) - R
  * [PhyD3](https://github.com/vibbits/phyd3) - D3.js  -  [2018 paper](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2283-2)
  * [phylotree.js](http://phylotree.hyphy.org/documentation/index.html) - D3.js 
  
  
### simulating somatic evolution
* CellCoal
