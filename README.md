# PTI: a top-down approach to track tumor evolution

### About

PTI (Phylogenetic Tree Inference) is a novel method which use an iterative top-down approach to infer the phylogenetic tree structure of multiple tumor biopsies from same patient using somatic mutations without the needs of accurate allele frequencies. Moreover, this method is generally applicable to infer phylogeny for any other datasets (such as epigenetics) with a similar zero and one feature-by-sample matrix. 

At a high level, PTI 's execution can be broken down into the following steps: (1)  defining the intersection of somatic mutations in all samples from the same patient as the length of the root trunk of the tree structure, (2) remove the shared mutations, and then find the optimal branch split, (3) remove the mutation information of the sample that has been the leaf node. Then, iterate through the above steps until all samples are split into leaf nodes. (5) each tree structure is weighted by a score, (6) output the tree structure with the largest weight score. In addition, our method also annotates the putative 299 driver genes on the branches of the tree for downstream analysis.

For more information about the algorithm please see: 

**Phylogenetic tree inference: a top-down approach to track tumor evolution**

### Program Parameters

PTI has only one parameter to set.

`--AF <arg>` The allele frequency of mutation, default is 0.1 

`-i <arg>` Input file path (required)

`-o <arg>` Output file path where the results should be written (required)

### Input File Types

PTI can accept two input file types, one is ‘one sample per file’, the other is ‘binomial matrix’, also known as ‘0-1 matrix’. The number of mutations used to reconstruct the phylogenetic tree has a significant impact on the accuracy of the tree structure. We recommend that the number of mutations used to build a tree for each tumor sample is no less than 30. However, while a large number of mutations increase the accuracy of the tree topology, it takes more runtime. Therefore, in the case of a large number of mutations, you can use the AF parameters (default is 0.1) which is calculated by the count of mutant reads and reference reads to filter the mutations to improve the speed of the PTI.

1. **One sample per file**

   Each tumor sample requires a somatic mutation file including high confidence mutation information, which can be extracted from a VCF file or a MAF file. The file name needs include a patient-specific ID and a sample-specific ID separated by underline, such as ‘P1_Met1.txt’. The first column in the file are the information about unique somatic mutation id, including chromosome, start and end position of the mutation, reference base and variant base. The other columns are the count of mutant reads, the count of reference reads and genes involved in the mutation. For each mutation, if it involves two or more genes, the genes need to be separated by a semicolon. 

   An example is shown below:

   ```
   # uniq_mutation_id         var_count          ref_count          gene_name
   3_178952_178952_A_G           156                5031              PIK3CA
   10_29821_298219_C_G           130                4203               SVIL
   11_77961_77961_C_T            474                15317              GAB2
   ```

2. **Binomial matrix**

   Each patient only needs one binary matrix and its elements can only be 0 or 1. The first column of the file records unique mutation id. The middle columns record whether the sample has a certain mutation. If the sample has a certain mutation, it is recorded as 1, otherwise it is recorded as 0. The last column is about the genes involved in the mutation. Since the binomial matrix does not contain the count of mutant reads and reference reads, the PTI command line does not contain restrictions on AF.

   An example is shown below:

   ```
   # uniq_mutation_id      R2     LN1a     LN1b     R1      R3     gene_name
   1_214818_214818_A_C     1        1       1       0       1       CENPF
   1_157556_157556_A_-     1        0       1       1       1       FCRL4
   1_200948_200948_-_TT    1        1       1       0       1       KIF21B
   ```

### How to run

For "One sample per file":

```
$  python  PTI.py  --AF  0.1  -i  input_dir  -o  output_dir
```

For "Binomial matrix":

```
$  python  PTI.py  -i  input_dir  -o  output_dir
```

### System Requirements

Python 3.7.4

### License

MIT License

### Support

For help running the program or any questions/suggestions/bug reports, please contact zhangly@shanghaitech.edu.cn



