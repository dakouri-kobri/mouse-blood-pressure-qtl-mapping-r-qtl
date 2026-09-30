# QTL Mapping with R/qtl<br>Mouse Blood Pressure Dataset

A reproducible QTL mapping exercise using **R/qtl**, **Quarto**,
**renv**, and **Git**. This is my version of one the computer exercise sessions 
done as part of the course of [Genome analysis BK0003 HT2026, 15 ECT credits](https://www.slu.se/en/student-web/studies/courses-and-programmes/course-search/kurser/g/genome-analysis/), at SLU (Swedish 
University of Agricultural Sciences), Uppsala, Sweden, autumn 2026

The analysis explores genetic regions associated with blood pressure
in the mouse hypertension dataset `hyper`, supplied with R/qtl.

## Dataset

The dataset contains 250 backcross individuals, with blood-pressure
and sex phenotypes and markers across 20 chromosomes.

The initial dataset contains 174 markers. Removing the marker with
no observed genotypes leaves 173 markers for analysis.

No separate dataset download is required:

```r
library(qtl)
data(hyper)
```

## Analysis workflow

The Quarto report covers:

1. Dataset summaries, missing genotypes, genetic maps, and phenotypes.
2. Pairwise recombination fractions, linkage LOD scores, and map estimation.
3. Identification and inspection of possible genotyping errors.
4. Single-QTL scans using EM, Haley–Knott regression, and multiple imputation.
5. Genome-wide significance using 1,000 Haley–Knott permutations.
6. Fitting, estimating effects, and refining an additive two-QTL model.

Intermediate cross objects are preserved separately to make the
analysis stages explicit.

## Main results

The permutation-based genome-wide 5% significance threshold was
approximately **LOD 2.65**. The Haley–Knott scan identified significant
peaks on chromosomes 1 and 4:

| Chromosome | Peak position (cM) | LOD |
|---|---:|---:|
| 1 | 48.3 | 3.55 |
| 4 | 29.5 | 8.09 |

The three scanning methods produced broadly consistent results,
with the strongest signal on chromosome 4.

The initial multiple-imputation model used QTL positions of 49.2 cM
on chromosome 1 and 29.5 cM on chromosome 4. The requested chromosome
1 position of 48.3 cM was represented by the nearest available
position in the imputation grid.

Position refinement improved the overall model fit:

| Measure | Initial model | Refined model |
|---|---:|---:|
| Chromosome 1 QTL position (cM) | 49.2 | 67.8 |
| Chromosome 4 QTL position (cM) | 29.5 | 29.5 |
| Overall LOD | 12.67 | 13.81 |
| Variance explained (%) | 20.82 | 22.46 |

Both QTL contributed after accounting for the other. The chromosome
4 QTL made the larger conditional contribution.

## Interpretation and limitations

Some markers were genotyped primarily in individuals with extreme
phenotypes or selected recombination events. This sampling pattern
helps explain unusual recombination estimates and expansion of the
re-estimated genetic map.

The main analysis retains the original map. Error LOD scores flag
potential genotype inconsistencies; calculating these scores does
not correct genotype calls.

The fitted formula is additive:

```r
additive_formula <- y ~ Q1 + Q2
```

It includes both QTL main effects and does not test an interaction.

Estimated QTL positions identify genomic regions associated with
blood pressure, rather than specific causal genes. The variance
explained describes the fitted model in this dataset and is not an
estimate of total blood-pressure heritability.

## Requirements

- R; preferably the version recorded in `renv.lock`.
- Quarto CLI.
- RStudio, recommended for opening the `.Rproj` file.
- Git, for cloning and version control.

R package dependencies and their versions are recorded in
`renv.lock`. R and the Quarto CLI must be installed separately.

## Reproduce the analysis

### 1. Clone and open the project

Clone this repository, then open:

```text
qtl-mapping-rqtl.Rproj
```

### 2. Restore R packages

From the R console in the project:

```r
renv::restore()
```

### 3. Render the report

From a terminal in the project directory:

```bash
quarto render qtl-mapping.qmd --to html
```

Alternatively, open the Quarto document in RStudio and click
**Render**.

Rendering runs the analysis and produces an HTML report. The
1,000-permutation calculation may take several minutes.

## Reproducibility

The analysis uses:

- `renv.lock` to record R package versions.
- Explicit random seeds for genotype simulation and permutation tests.
- Quarto to combine code, output, figures, and interpretation.
- `sessionInfo()` to document the R environment used to render the report.
- Git to track changes to the analysis and documentation.

Genotype probabilities use a 1 cM step. Multiple imputation uses
a 2 cM step and 16 genotype draws. Both use an assumed genotyping
error probability of 0.01.

## Acknowledgments

_Supervising instructor_: **Dominic Wright**, Professor, HBIO, Molecular Genetics and Bioinformatics

This educational analysis follows the supplied QTL mapping worksheet,
based on Karl W. Broman's *A brief tour of R/qtl* (21 March 2012).

The `hyper` dataset originates from the mouse hypertension study
by Sugiyama et al., *Genomics* 71:70–77 (2001). The worksheet credits
Bev Paigen and Gary Churchill for providing the data.

## Author
**Dakouri Kobri**<br>
Data Science, AI/ML, Bioinformatics, Pharmacology, Toxicology<br>
& Health Science Enthusiast

LinkedIn: https://www.linkedin.com/in/dakouri-m-kobri-009192208/

## Resources

- [R/qtl](https://rqtl.org/)
- [Quarto](https://quarto.org/)
- [renv documentation](https://rstudio.github.io/renv/)