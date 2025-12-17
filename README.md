# Store Performance Analysis & Clustering

## Overview

This project presents a comprehensive methodology for store performance analysis and clustering using multi-dimensional business metrics. We systematically compare various clustering algorithms and preprocessing techniques to identify meaningful store segments based on sales performance, advertising efficiency, and operational characteristics. The analysis reveals **3 distinct store clusters** with clear performance patterns and actionable business recommendations.

## Project Goals

1. **Store Segmentation**: Identify distinct store segments based on multi-dimensional performance metrics
2. **Performance Analysis**: Analyze store performance patterns, advertising efficiency, and operational characteristics
3. **Clustering Optimization**: Systematically compare clustering algorithms and preprocessing techniques
4. **Business Recommendations**: Provide actionable insights for store management and resource allocation

## Key Findings

### Three Distinct Store Clusters

- **Cluster 0 (Low-Volume, High-ROAS)**: 621 stores (26.4%)
  - Average Sales: $14.79/month
  - Average Ad Spend: $502.90/month
  - ROAS: 0.360 (highest efficiency)
  - Ad Active Ratio: 37.7%
  - Channel Focus: Social media (21.6%)

- **Cluster 1 (High-Volume, Low-ROAS)**: 1,085 stores (46.1%)
  - Average Sales: $35.25/month
  - Average Ad Spend: $6,114.21/month
  - ROAS: 0.027 (lowest efficiency)
  - Ad Active Ratio: 66.7%
  - Channel Focus: Search (34.4%)

- **Cluster 2 (Medium-Volume, Balanced)**: 646 stores (27.5%)
  - Average Sales: $21.70/month
  - Average Ad Spend: $2,052.97/month
  - ROAS: 0.317 (balanced efficiency)
  - Ad Active Ratio: 51.4%
  - Channel Focus: Balanced (20.6% search, 13.8% social)

### Clustering Performance

- **Method**: ISO_QT_PCA95_KMeans_k3
- **Silhouette Score**: 0.586 (exceeds target of ≥ 0.40)
- **Balance Ratio**: 1.75:1 (meets target of ≤ 3.0)
- **Stability**: Perfect stability (ARI = 1.0, NMI = 1.0)
- **Stores Analyzed**: 2,557 → 2,352 (removed 205 outliers, 8%)

## Dataset

### Data Sources

- **Store KPI Dataset** (`data/raw/store_kpi.csv`): 61,322 store-month observations, 37 features
- **Sales Transaction Dataset** (`data/raw/sales_data.csv`): 185,294 individual transactions
- **Analysis Period**: 2023-07 to 2025-06 (24 months)
- **Unique Stores**: 2,557 stores

### Processed Data Files

- `data/processed/store_kpi_clean.csv`: Cleaned monthly store KPI data
- `data/processed/store_features.csv`: Store-level aggregated features
- `data/processed/store_features_enhanced.csv`: Enhanced features with ad activity metrics
- `data/processed/store_clusters_final.csv`: Final cluster assignments (2,352 stores)
- `data/processed/cluster_business_metrics_final.csv`: Cluster business metrics summary
- `data/processed/cluster_profiles_final.csv`: Detailed cluster profiles with statistics

### Data Quality Issues

1. **Ad Spend Data Gap**: Zero ad spend recorded from 2023-10 to 2024-05
2. **Missing Values**: 5-15% missing values in various KPI metrics
3. **Outliers**: Extreme values in sales and ad spend metrics
4. **Temporal Inconsistencies**: Inconsistent data collection periods

## Methodology

### 1. Feature Engineering

**11 Key Features Selected:**
1. `sales_mean`: Average monthly sales
2. `total_ad_spend_mean`: Average monthly ad spend
3. `roas_mean`: Return on ad spend
4. `conversion_rate_mean`: Conversion rate
5. `website_visits_mean`: Average website visits
6. `search_spend_ratio_mean`: Search ad spend ratio
7. `social_spend_ratio_mean`: Social ad spend ratio
8. `ad_months_active`: Number of months with ad activity
9. `ad_active_ratio`: Proportion of months with advertising activity
10. `spent_any`: Binary indicator for any advertising investment
11. `avg_ad_spend_active_only`: Average ad spend for active months only

### 2. Preprocessing Pipeline

**Final Pipeline: ISO_QT_PCA95**

1. **IsolationForest Outlier Removal**: Removes ~8% of outliers (contamination=0.08)
2. **QuantileTransformer Normalization**: Maps features to normal distribution
3. **PCA Dimensionality Reduction**: Retains 95% of variance (reduces to 5 components)

### 3. Clustering Algorithm

**Final Method: KMeans (k=3)**

- **Algorithm**: KMeans clustering
- **Parameters**: n_clusters=3, n_init=100, max_iter=1000, random_state=42
- **Performance**: Silhouette Score = 0.586, Balance Ratio = 1.75:1
- **Stability**: Perfect stability (ARI = 1.0, NMI = 1.0)

### 4. Evaluation Metrics

- **Silhouette Score**: 0.586 (target: ≥ 0.40) ✅
- **Davies-Bouldin Index**: 0.706 (lower is better)
- **Calinski-Harabasz Score**: 4919.6 (higher is better)
- **Balance Ratio**: 1.75:1 (target: ≤ 3.0) ✅
- **Stability**: ARI = 1.0, NMI = 1.0 (perfect stability)

## Project Structure

```
.
├── README.md                          # This file
├── Project2_Complete_Analysis_Workflow.md  # Complete analysis workflow documentation
│
├── code/                              # Analysis notebooks
│   ├── project2_data_processing.ipynb        # Data loading and preprocessing
│   ├── project2_eda_analysis.ipynb           # Exploratory data analysis
│   ├── project2_final_clustering.ipynb       # Final clustering implementation
│   ├── project2_clustering_robustness.ipynb  # Robustness analysis
│   ├── project2_clustering_enhanced.ipynb    # Enhanced features analysis
│   └── supervised_learning/                  # Supervised learning models
│       ├── project2_supervised_learning_complete.ipynb
│       └── project2_recommendation_system.ipynb
│
├── data/                              # Data files
│   ├── raw/                           # Raw data files
│   │   ├── store_kpi.csv             # Store KPI data (61,322 observations)
│   │   └── sales_data.csv            # Sales transaction data
│   └── processed/                     # Processed data files
│       ├── store_kpi_clean.csv       # Cleaned monthly store KPI data
│       ├── store_features.csv        # Store-level aggregated features
│       ├── store_features_enhanced.csv  # Enhanced features with ad activity metrics
│       ├── store_clusters_final.csv  # Final cluster assignments
│       ├── cluster_business_metrics_final.csv  # Cluster business metrics
│       ├── cluster_profiles_final.csv  # Detailed cluster profiles
│       └── supervised_learning/      # Supervised learning model files
│
├── website-source/                    # Quarto website source files
│   ├── _quarto.yml                   # Quarto configuration
│   ├── index.qmd                     # Introduction page
│   ├── eda_2.qmd                     # Exploratory Data Analysis page
│   ├── methods_2.qmd                 # Methods page
│   └── conclusion_2.qmd              # Conclusion page
│
└── docs/                              # Generated website files
    ├── index.html                    # Introduction page
    ├── eda_2.html                    # EDA page
    ├── methods_2.html                # Methods page
    └── conclusion_2.html             # Conclusion page
```

## Analysis Workflow

### 1. Data Processing (`code/project2_data_processing.ipynb`)

- Load raw data from `data/raw/store_kpi.csv`
- Clean and preprocess data
- Handle missing values and outliers
- Create derived metrics (ROAS, conversion rate, CTR, etc.)
- Generate store-level aggregated features
- Save processed data to `data/processed/`

### 2. Exploratory Data Analysis (`code/project2_eda_analysis.ipynb`)

- Data quality assessment
- Feature distribution analysis
- Correlation analysis
- Time series analysis
- Performance benchmarking
- PCA visualization

### 3. Clustering Analysis (`code/project2_final_clustering.ipynb`)

- Feature selection (11 features)
- Preprocessing pipeline (ISO_QT_PCA95)
- KMeans clustering (k=3)
- Cluster evaluation and validation
- Cluster profile analysis

### 4. Robustness Analysis (`code/project2_clustering_robustness.ipynb`)

- Bootstrap stability analysis
- Sensitivity analysis
- Ablation study (enhanced features vs baseline features)

### 5. Enhanced Features Analysis (`code/project2_clustering_enhanced.ipynb`)

- Enhanced feature engineering
- Feature importance analysis
- Ablation study results

## Key Results

### Clustering Results

- **Total Stores Analyzed**: 2,557
- **Stores After Outlier Removal**: 2,352 (removed 205 outliers, 8%)
- **Final Clusters**: 3 clusters with balanced sizes (1.75:1 ratio)
- **Clustering Quality**: Silhouette Score = 0.586 (exceeds target of ≥ 0.40)
- **Stability**: Perfect stability (ARI = 1.0, NMI = 1.0)

### Enhanced Features Impact

- **Silhouette Score Improvement**: 33.9% (0.4374 → 0.5855)
- **Balance Ratio Improvement**: 88.3% (14.92:1 → 1.75:1)
- **Calinski-Harabasz Score Improvement**: 292.4%

### Business Insights

1. **Three Distinct Store Segments**: Efficient Small Stores, High Volume Low Efficiency Stores, and Balanced Medium Stores
2. **ROAS Efficiency Trade-off**: Higher sales volume does not necessarily mean higher ROAS efficiency
3. **Channel Preferences**: Different clusters prefer different advertising channels
4. **Ad Activity Patterns**: Ad activity frequency varies by cluster (37.7%, 66.7%, 51.4%)
5. **Size Distribution**: Cluster 1 is the largest (46.1%), indicating most stores are high-volume, low-ROAS stores

## Business Recommendations

### Cluster 0 (Low-Volume, High-ROAS Stores)

**Strengths**: Highest ROAS efficiency (0.360), effective use of limited ad budget

**Recommendations**:
1. Maintain current strategy focusing on social media advertising
2. Scale up carefully while maintaining efficiency
3. Increase ad activity ratio from 37.7% to 50%+
4. Optimize social media campaigns
5. Focus on customer retention

### Cluster 1 (High-Volume, Low-ROAS Stores)

**Strengths**: Highest sales volume ($35.25/month), high ad activity (66.7%)

**Recommendations**:
1. **Priority**: Improve ROAS efficiency (reduce ad spending by 20-30%)
2. Optimize search advertising
3. Diversify channel mix (reduce reliance on search, test social media)
4. Improve conversion rate
5. Focus on quality over quantity in ad activity

### Cluster 2 (Medium-Volume, Balanced Stores)

**Strengths**: Balanced performance, good ROAS (0.317), balanced channel strategy

**Recommendations**:
1. Maintain balanced strategy
2. Gradual growth (increase ad spending by 10-15%)
3. Optimize channel performance
4. Increase ad activity ratio from 51.4% to 60%+
5. Focus on customer acquisition and retention

## Installation and Setup

### Prerequisites

- Python 3.8+
- Jupyter Notebook or JupyterLab
- Required Python packages (see `requirements.txt` or install below)

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd dsan-capstone-2025

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install required packages
pip install pandas numpy matplotlib seaborn scikit-learn scipy jupyter quarto
```

### Required Packages

- `pandas`: Data manipulation and analysis
- `numpy`: Numerical computing
- `matplotlib`: Plotting and visualization
- `seaborn`: Statistical data visualization
- `scikit-learn`: Machine learning algorithms
- `scipy`: Scientific computing
- `jupyter`: Jupyter notebook environment
- `quarto`: Quarto publishing system

## Usage

### Running the Analysis

1. **Data Processing**:
   ```bash
   jupyter notebook code/project2_data_processing.ipynb
   ```

2. **Exploratory Data Analysis**:
   ```bash
   jupyter notebook code/project2_eda_analysis.ipynb
   ```

3. **Clustering Analysis**:
   ```bash
   jupyter notebook code/project2_final_clustering.ipynb
   ```

4. **Robustness Analysis**:
   ```bash
   jupyter notebook code/project2_clustering_robustness.ipynb
   ```

### Generating the Website

```bash
# Navigate to website-source directory
cd website-source

# Render the website
quarto render

# Preview the website
quarto preview
```

The generated website will be available in the `docs/` directory.

## File Descriptions

### Code Files

- **`project2_data_processing.ipynb`**: Data loading, cleaning, and feature engineering
- **`project2_eda_analysis.ipynb`**: Exploratory data analysis and visualization
- **`project2_final_clustering.ipynb`**: Final clustering implementation and evaluation
- **`project2_clustering_robustness.ipynb`**: Robustness and stability analysis
- **`project2_clustering_enhanced.ipynb`**: Enhanced features analysis and ablation study

### Data Files

- **`store_kpi_clean.csv`**: Cleaned monthly store KPI data (61,322 observations)
- **`store_features.csv`**: Store-level aggregated features (2,557 stores)
- **`store_features_enhanced.csv`**: Enhanced features with ad activity metrics (2,557 stores)
- **`store_clusters_final.csv`**: Final cluster assignments (2,352 stores)
- **`cluster_business_metrics_final.csv`**: Cluster business metrics summary
- **`cluster_profiles_final.csv`**: Detailed cluster profiles with statistics

### Website Files

- **`index.qmd`**: Introduction page
- **`eda_2.qmd`**: Exploratory Data Analysis page
- **`methods_2.qmd`**: Methods page (clustering methodology)
- **`conclusion_2.qmd`**: Conclusion page (cluster characteristics and recommendations)

## Reproducibility

- **Random Seeds**: All random seeds fixed (random_state=42)
- **Code Documentation**: Complete code documentation and comments
- **Version Control**: All code and data files are version controlled
- **Methodology Documentation**: Detailed methodology documentation in `Project2_Complete_Analysis_Workflow.md`

## Results Summary

### Clustering Performance

- **Method**: ISO_QT_PCA95_KMeans_k3
- **Silhouette Score**: 0.586 (exceeds target of ≥ 0.40)
- **Balance Ratio**: 1.75:1 (meets target of ≤ 3.0)
- **Stability**: Perfect stability (ARI = 1.0, NMI = 1.0)
- **Stores Analyzed**: 2,557 → 2,352 (removed 205 outliers, 8%)

### Cluster Characteristics

- **Cluster 0**: 621 stores (26.4%) - Low-Volume, High-ROAS Stores
- **Cluster 1**: 1,085 stores (46.1%) - High-Volume, Low-ROAS Stores
- **Cluster 2**: 646 stores (27.5%) - Medium-Volume, Balanced Stores

### Business Impact

- **Targeted Recommendations**: Each cluster receives customized recommendations
- **Efficiency Improvement**: Cluster 1 can improve ROAS by 20-30% through optimization
- **Growth Potential**: Cluster 0 can scale up while maintaining efficiency
- **Balanced Strategy**: Cluster 2 can maintain balance while growing

## References

See `Project2_Complete_Analysis_Workflow.md` for complete references and methodology details.

## License

This project is licensed under the CC BY-NC 4.0 License - see the [LICENSE](LICENSE) file for details.

## Contact

For questions or inquiries about this project, please contact the project team.

---

**Last Updated**: 2025-01-XX
