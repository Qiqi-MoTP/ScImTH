# ScImTH

Here, we proposed an algorithm (ScImTH) to evaluate the levels of intratumor immune heterogeneity (ImTH). ScImTH defines a tumor’s ImTH level as the Shannon entropy of diverse immune cell types’ composition within the tumor microenvironment.

![ScImTH](E:\luqiqi\ITH\R_pacakges\ImTH\ScImTH.png)

&nbsp
&nbsp;

# Description

ScImTH defines a tumor’s ImTH level as the Shannon entropy of 14 immune cell types within the tumor microenvironment. The ImTH score represents a biologically validated framework for quantifying tumor immune heterogeneity, demonstrating clinical potential in cancer prognosis and immunotherapy response. The ImTH score is expected to emerge as a robust tool for capturing the dynamic evolution of tumor-immune ecosystems, as well as a promising biomarker for immune surveillance in precision oncology. A top priority of future studies should focus on multimodal integration with emerging biomarkers to optimize immunotherapy stratification models.



&nbsp;

# Details

+ The function `get_cibersort()` is used to characterize the fractions of tumor-infiltrating immune cells for the bulk transcriptome set, containing  three parameters:
  + "user_file" is the path of the gene expression data frame, where row names are gene symbols and column names are samples. The gene expression data frame should be 'txt' file.
  + "perm" represents the number of permutations.
  + "QN"  represents perform quantile normalization or not (TRUE/FALSE).
+ The function `ImTH()` is used to calculate ImTH score, containing two parameters:
  + "data" is a data frame of mRNA expression data or immune cell proportions, where row names are samples and column names are immune cells. The values denote the proportion of the immune cell type. It should be noted that the sum of all immune cells’ proportions equals 1 across each sample.
  + "type" is a character vector indicating the input type. The default is '"CIBERSORT". If your input data is the output of `get_cibersort(),` you should choose 'CIBERSORT'; if your input data is an immune cell proportions data, you should choose 'custom'.

&nbsp;
&nbsp;

# Installation

- You can install the released version of **ScImTH** with:
  &nbsp;

```R
if (!requireNamespace("devtools", quietly = TRUE))
    install.packages("devtools")

devtools::install_github("WangX-Lab/ScImTH")
```

&nbsp;
&nbsp;

# Examples

&nbsp
&nbsp;

### **Apply ScImTH in bulk transcriptome**

```R
library(ScImTH)
user_file <- system.file("extdata", "bulk_example.RData", package = "ScImTH")
#load(user_file)
#ls()
#"file_path"  "lncrna"  "methylation"  "mirna"  "mrna"
result <- get_cibersort(user_file,perm = 1000, QN = TRUE)
ImTH_bulk <- ImTH(result, type = "CIBERSORT")
```



**bulk_example**

```R
bulk_example[1:5,1:5]
```

| Gene_ID     | S1       | S2       | S3       | S4       | S5        |
| ----------- | -------- | -------- | -------- | -------- | --------- |
| **TIMM23**  | 19.1687  | 0.7131   | 2.636706 | 0.000000 | 278.5087  |
| **MOXD2**   | 241.4591 | 219.3753 | 97.7579  | 2392.174 | 10.392884 |
| **SSX9**    | 179.5861 | 1237.664 | 2.475163 | 185.4157 | 196.847   |
| **CXorf67** | 2.4553   | 298.915  | 10.2008  | 12.0053  | 10.914296 |
| **EFCAB8**  | 16.4854  | 13.6758  | 0.000000 | 6.300019 | 179.6164  |



**result**

```R
result[1:5,1:5]
```

|                                  | B cells naive | B cells memory | Plasma cells | T cells CD8 | T cells CD4 naive |
| -------------------------------- | ------------- | -------------- | ------------ | ----------- | ----------------- |
| **TCGA-N5-A4R8-01A-11R-A28V-07** | 0.1456667     | 0              | 0.08456818   | 0.001853958 | 0.08456818        |
| **TCGA-N5-A4RA-01A-11R-A28V-07** | 0.1875115     | 0              | 0.2743644    | 0.04440196  | 0                 |
| **TCGA-N5-A4RD-01A-11R-A28V-07** | 0.08702867    | 0              | 0.2780427    | 0.04455536  | 0                 |
| **TCGA-N5-A4RF-01A-11R-A28V-07** | 0.02707169    | 0              | 0.05573348   | 0           | 0                 |
| **TCGA-N5-A4RJ-01A-11R-A28V-07** | 0.1500988     | 0              | 0.1094837    | 0.08102873  | 0                 |



**ImTH_bulk**

```R
ImTH_bulk[1:5,]
```

| **Sample**                       | ImTH_Score |
| :------------------------------- | :--------: |
| **TCGA-N5-A4R8-01A-11R-A28V-07** |  1.848853  |
| **TCGA-N5-A4RA-01A-11R-A28V-07** |  2.319261  |
| **TCGA-N5-A4RD-01A-11R-A28V-07** |  2.361131  |
| **TCGA-N5-A4RF-01A-11R-A28V-07** |  2.011589  |
| **TCGA-N5-A4RJ-01A-11R-A28V-07** |  2.693232  |

&nbsp;




## Apply ScImTH to single cell data

```R
file_path <- system.file("extdata", "sc_example.RData", package = "ScImTH")
load(file_path)
ls()
"file_path"  "sc_example"
ImTH_sc <- ImTH(sc_example, type = "custom")
```

&nbsp;

**sc_example**

```R
sc_example[1:5,1:5]
```

| **row.names** | Dendritic   | Mast-cells | Neutrophils | pDCs        | T-cells   |
| ------------- | ----------- | ---------- | ----------- | ----------- | --------- |
| **AZ_01**     | 0.008368201 | 0.041841   | 0           | 0.0041841   | 0.4142259 |
| **AZ_02**     | 0.005347594 | 0.07486631 | 0.03208556  | 0.005347594 | 0.6737968 |
| **AZ_03**     | 0.03296703  | 0.04029304 | 0.01465201  | 0.01098901  | 0.3186813 |
| **AZ_04**     | 0.1265823   | 0.07172996 | 0           | 0.02953586  | 0.4092827 |
| **AZ_05**     | 0.1193182   | 0.07954545 | 0           | 0.01136364  | 0.3409091 |

&nbsp;

**ImTH_sc**

```R
ImTH_sc[1:5,]
```

| **Sample** | ImTH_Score |
| :--------- | :--------: |
| **AZ_01**  |  1.981976  |
| **AZ_02**  |  1.631447  |
| **AZ_03**  |  2.299599  |
| **AZ_04**  |  2.020397  |
| **AZ_05**  |  2.303584  |



## **Apply ScImTH to spatial transcriptome data**

```R
file_path <- system.file("extdata", "ST_example.RData", package = "ScImTH")
load(file_path)
ls()
"file_path"  "spatial_count"  "spatial_location"  "sc_count2"  "sc_meta2"
#Using the deconvolution algorithm to infer cell proportion. Here we used CARD algorithm (https://github.com/YMa-lab/CARD). Users can choose other algorithm to instead.
library(CARD) 
CARD_obj = createCARDObject(
	sc_count = sc_count2,
	sc_meta = sc_meta2,
	spatial_count = spatial_count,
	spatial_location = spatial_location,
	ct.varname = "celltype_major",
	ct.select = unique(sc_meta2$celltype_major),
	sample.varname = "orig.ident",
	minCountGene = 100,
	minCountSpot = 5) 
CARD_obj = CARD_deconvolution(CARD_object = CARD_obj)
ratio = as.data.frame(CARD_obj@Proportion_CARD)

ImTH_ST <- ImTH(ratio, type = "custom")
```



**spatial_count**

```R
spatial_count[1:5,1:5]
```

| row.names      | 10x10 | 10x11 | 10x12 | 10x13 | 10x14 |
| -------------- | ----- | ----- | ----- | ----- | ----- |
| **FO538757.1** | 0     | 0     | 0     | 0     | 0     |
| **SAMD11**     | 0     | 0     | 0     | 0     | 0     |
| **NOC2L**      | 0     | 0     | 0     | 0     | 0     |
| **KLHL17**     | 0     | 0     | 0     | 0     | 0     |
| **PLEKHN1**    | 0     | 0     | 0     | 0     | 0     |



**spatial_location**

```R
spatial_location[1:5,]
```

| row.names | x    | y    |
| --------- | ---- | ---- |
| **10x10** | 10   | 10   |
| **10x11** | 10   | 11   |
| **10x12** | 10   | 12   |
| **10x13** | 10   | 13   |
| **10x14** | 10   | 14   |



**sc_count2**

```R
sc_count2[1:5,1:5]
```

| row.names         | CID3586_AAAGATGCAGGGAGAG | CID3586_AAATGCCCATCCCATC | CID3586_AACTTTCAGGGCTTGA | CID3586_AATCCAGTCAACTCTT | CID3586_ACGGAGAGTGGGTATG |
| ----------------- | ------------------------ | ------------------------ | ------------------------ | ------------------------ | ------------------------ |
| **RP11-34P13.7**  | 0                        | 0                        | 0                        | 0                        | 0                        |
| **FO538757.3**    | 0                        | 0                        | 0                        | 0                        | 0                        |
| **FO538757.2**    | 0                        | 0                        | 0                        | 0                        | 0                        |
| **AP006222.2**    | 0                        | 0                        | 0                        | 0                        | 0                        |
| **RP4-669L17.10** | 0                        | 0                        | 0                        | 0                        | 0                        |

&nbsp;

**sc_meta2**

```R
sc_meta2[1:5,1:5]
```

| row.names                    | orig.ident | ...  | subtype | celltype_major |
| ---------------------------- | ---------- | ---- | ------- | -------------- |
| **CID3586_AAAGATGCAGGGAGAG** | CID3586    | ...  | HER2+   | B-cells        |
| **CID3586_AAATGCCCATCCCATC** | CID3586    | ...  | HER2+   | B-cells        |
| **CID3586_AACTTTCAGGGCTTGA** | CID3586    | ...  | HER2+   | B-cells        |
| **CID3586_AATCCAGTCAACTCTT** | CID3586    | ...  | HER2+   | B-cells        |
| **CID3586_ACGGAGAGTGGGTATG** | CID3586    | ...  | HER2+   | B-cells        |



**ratio**

```R
ratio [1:5,1:5]
```

| **row.names** | B-cells    | T-cells   | Myeloid    | Plasmablasts |
| ------------- | ---------- | --------- | ---------- | ------------ |
| **10x10**     | 0.1372354  | 0.6009291 | 0.1505987  | 0.1505987    |
| **10x11**     | 0.2447332  | 0.6004037 | 0.6004037  | 0.04513076   |
| **10x12**     | 0.09254974 | 0.6872927 | 0.1072432  | 0.1129143    |
| **10x13**     | 0.2588382  | 0.6474849 | 0.07595571 | 0.01772118   |
| **10x14**     | 0.01772118 | 0.7197694 | 0.1160269  | 0.09341512   |

&nbsp;

**ImTH_ST**

```R
ImTH_ST[1:5,]
```

| **Sample** | ImTH_Score |
| :--------- | :--------: |
| **10x10**  |  1.598489  |
| **10x11**  |  1.490421  |
| **10x12**  |  1.390351  |
| **10x13**  |  1.296294  |
| **10x14**  |  1.291939  |

# Contact

E-mail any questions to Xiaosheng Wang (xiaosheng.wang@cpu.edu.cn)
