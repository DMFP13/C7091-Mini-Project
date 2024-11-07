# Mini Project C7091

**Author**: Dan Peters  
**Published**: October 7, 2024

## Rendered version https://rpubs.com/DanJPeters/1242407

## Background

Biological nitrogen fixation (BNF) is a critical process in agricultural ecosystems that allows certain plants, particularly legumes like soybeans (*Glycine max*), to obtain nitrogen through symbiotic relationships with nitrogen-fixing bacteria, such as *Rhizobium* species. Nitrogen is an essential macronutrient for crop yield and quality. However, intensive monocropping with cereals has led to a decline in soil nutrients and a reduction in effective nitrogen-fixing bacteria populations.

Traditional BNF enhancement methods, such as bacterial inoculants, face limitations in effectiveness due to environmental factors and establishment challenges within existing soil microbial ecosystems. Moreover, these inoculants do not provide a lasting effect, often requiring repeated applications, which increases costs and reduces sustainability.

This project explores a novel nanotechnology-based BNF treatment designed to establish long-term interactions between beneficial microbes and plant roots. The technology works by affecting the gene expression of nitrogen-fixing bacteria, enhancing microbial activity in response to specific root exudates and signals. We hypothesize that this treatment will improve nitrogen availability and result in increased soybean yields over multiple crop cycles.

## Objectives and Hypotheses

The main goals of this study are:
1. To determine whether the nanotechnology-enhanced BNF treatment will yield better results than traditional inoculants and untreated controls in both 2016 and 2017.
2. To assess whether the nanotechnology treatment provides a sustained, long-term improvement in crop yield.
3. To evaluate whether traditional BNF inoculants yield better results than untreated control due to improved availability of nitrogen-fixing bacteria.

**Null Hypotheses**:
1. There is no significant difference in soybean yield between the nanotechnology treatment, regular BNF inoculant, and control.
2. The yield of the nanotechnology treatment does not improve in the second year.
3. Regular BNF inoculants do not yield better results than the control.

## Methodology

The study utilizes a two-way ANOVA to analyze yield differences across treatments and years, with a post hoc analysis to examine specific pairwise differences. Data preprocessing includes handling categorical variables and addressing missing values in yield data.

Key tools used:
- **R libraries**: `ggplot2`, `dplyr`, `car`, `emmeans`, `readxl`, `writexl`
- **File**: `data (3).xlsx` in the "data" sheet, containing information on yield across treatments and years.

## Analysis Code

Install necessary libraries and load the dataset as follows:

```r
# Load and install required libraries
if (!require(ggplot2)) install.packages('ggplot2', dependencies = TRUE)
if (!require(dplyr)) install.packages('dplyr', dependencies = TRUE)
if (!require(car)) install.packages('car', dependencies = TRUE)
if (!require(emmeans)) install.packages('emmeans', dependencies = TRUE)
if (!require(readxl)) install.packages('readxl', dependencies = TRUE)
if (!require(writexl)) install.packages('writexl', dependencies = TRUE)

# Load libraries
library(ggplot2)
library(dplyr)
library(car)
library(emmeans)
library(readxl)
library(writexl)

# Define file path and load data
data_path <- "/Users/mac1/Downloads/data (3).xlsx"
data <- read_excel(data_path, sheet = "data")

# Data preprocessing
data$soil <- as.factor(data$soil)
data$yr <- as.factor(data$yr)
data$Yield <- as.numeric(data$Yield)
data <- data %>% filter(!is.na(Yield))

# ANOVA analysis
anova_result <- aov(Yield ~ soil * yr + rep, data = data)
summary(anova_result)

# Post hoc analysis
posthoc_result <- emmeans(anova_result, pairwise ~ soil * yr)
print(posthoc_result)
