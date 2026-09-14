# BACHELOR'S THESIS - BACHELOR'S DEGREE IN BIOINFORMATICS (UCAV, 2026)
**Transcriptome-Wide Association Study (TWAS) and integrative analysis in the dorsolateral prefrontal cortex for major depressive disorder (MDD), bipolar disorder (BD), and suicide attempt (SA).**

**Author:** Miguel Rosell Hidalgo  
**Supervisor:** Rubén Villa Muñoz  

---

## DESCRIPTION
This repository contains the code and results of the TWAS pipeline executed with the FUSION software on GWAS summary statistics, integrating expression models of the dorsolateral prefrontal cortex (DLPFC) from the CommonMind consortium. Furthermore, it includes preprocessing, genome-wide TWAS, multiple testing correction, joint/conditional analysis, and functional enrichment analysis.

---

## CONTENTS
* **`0_datos_procesados/`**  
  Cleaned inputs for conditional analysis (`INPUT_CONDICIONAL_{MDD,BD,SA}_LIMPIO.txt`).

* **`1_codigo/`**  
  R scripts for the pipeline (which also correspond to the Appendices of the thesis):
  * `limpiar_daner.R`: QC and Z-score calculation; generates the 4-column input for FUSION (Appendix 9.1).
  * `manhattan_twas_{MDD,BD,SA}.R`: Post-TWAS: FDR/Bonferroni correction, top genes table, and Manhattan plot (Appendix 9.2).
  * `filtrado_condicional_{MDD,BD,SA}.R`: Sanitization and filtering (P<0.001) prior to conditional analysis (Appendix 9.3).
  * `limpiar_resultados_enriquecimiento_{MDD,BD,SA}.R`: Extraction of gene lists (P<0.05) for Enrichr (Appendix 9.4).

* **`2_resultados_TWAS/`**  
  Consolidated TWAS results by phenotype (`RESULTADOS_TWAS_{MDD,BD,SA}_COMPLETOS.txt`; around 5300 gene models).

* **`3_resultados_condicional/`**  
  Outputs of the joint/conditional analysis by phenotype (MDD, BD, SA folders): `.joint_included.dat` and `.joint_dropped.dat` files, and LocusZoom plots (`.loc_*.pdf`) per locus.  
  *Note: the `.joint_dropped` files are empty for all three phenotypes (this is because no gene was dropped due to linkage disequilibrium).*

* **`4_genes_priorizados/`**  
  Suggestive genes (P<0.001) by phenotype: `Top_Genes_MDD.csv`, `Top_Genes_BD.csv`, and `Top_Genes_Suicidio.csv` (SA). These tables already include the P_FDR and P_Bonferroni columns. In addition, the input lists for Enrichr: `Lista_Enrichr_{MDD,BD,SA}_005.txt`.

* **`5_figuras/`**  
  Final Manhattan plots (DLPFC) by phenotype (`Manhattan_TWAS_{MDD,BD,SA}_Etiquetado.png`; Figures 3-5 of the thesis).

---

## WORKFLOW (execution order)
1. `limpiar_daner.R` -> sumstats in FUSION format (4 columns).
2. `TWAS with FUSION` -> `2_resultados_TWAS/RESULTADOS_TWAS_*_COMPLETOS.txt`
3. `manhattan_twas_*.R` -> `4_genes_priorizados/Top_Genes_*.csv` + `5_figuras/*.png`
4. `filtrado_condicional_*.R` -> `0_datos_procesados/INPUT_CONDICIONAL_*_LIMPIO.txt`
5. `FUSION conditional analysis` -> `3_resultados_condicional/{MDD,BD,SA}/`
6. `limpiar_resultados_enriquecimiento_*.R` -> `4_genes_priorizados/Lista_Enrichr_*_005.txt` -> `Enrichr`

*Paths in the scripts are relative to the working directory where they were executed; adjust them if files are relocated.*

---

## EXTERNAL DEPENDENCIES (not included due to size and licensing)
* **FUSION software** and reference panels 1000 Genomes (LDREF) and CommonMind DLPFC (CMC.BRAIN.RNASEQ): available at [http://gusevlab.org/projects/fusion/](http://gusevlab.org/projects/fusion/)
* **Source GWAS data:** provided by Dr. Claudio Toma's group (CBMSO-CSIC), restricted access and authorized use for this Bachelor's thesis.

**ENVIRONMENT:** R 4.3.1 on WSL (Ubuntu).  
**R packages:** `optparse`, `plink2R`, `Rcpp`, `RcppEigen`, `qqman`.

## RESULTS OVERVIEW
### Major Depressive Disorder (MDD)
![Manhattan MDD](5_figuras/Manhattan_TWAS_MDD_Etiquetado.png)

### Bipolar Disorder (BD)
![Manhattan BD](5_figuras/Manhattan_TWAS_BD_Etiquetado.png)

### Suicide Attempt (SA)
![Manhattan SA](5_figuras/Manhattan_TWAS_SA_Etiquetado.png)
