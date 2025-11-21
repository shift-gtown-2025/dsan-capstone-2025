# Project 1 Sales Forecasting Model Enhancement

## Project Goals

1. **Demand Pattern Segmentation** using ADI, zero-share, and ACF12.  
2. **Model Benchmarking** for Croston, SARIMA, Prophet, and other candidate models.  
3. **Comprehensive Evaluation** using MAPE, RMSE, MAE, sMAPE, Bias, and MASE.  
4. **Error Structure Analysis** including distributional variability and win-count dominance.  
5. **Pattern-Specific Recommendations** for forecasting in retail environments.

## Key Findings

### Distinct Demand Behaviors
- Intermittent series: sparse, zero-inflated, irregular spikes.  
- Regular series: smooth, stable, low-variance.  
- Seasonal series: repeating cycles, often with < 2 full seasonal periods.

### Model Performance Depends on Pattern
- **Intermittent:** Croston consistently dominates.  
- **Regular:** SARIMA is the only viable and most accurate model.  
- **Seasonal:** SARIMA outperforms Prophet due to limited seasonal depth.

### Robustness Across Metrics
- Rankings are consistent across MAPE, sMAPE, Bias, and MASE.  
- Prophet shows the highest error variance and strongest negative bias.  
- SARIMA exhibits stable performance for regular and seasonal demand.

### Model Dominance
- Croston wins all intermittent series.  
- SARIMA wins all regular and most seasonal series.  
- Prophet rarely achieves best performance.

## Dataset

### Data Source
- Monthly store–item sales data.  
- Feature indicators computed:  
  - Zero-share  
  - Average inter-demand interval (ADI)  
  - Autocorrelation at lag 12 (ACF12)

### Processed Data Files
- `series_pattern_summary.csv`: demand pattern labels  
- Cleaned monthly series for forecasting  
- Engineered indicators

### Data Challenges
- Short history limits seasonal model reliability.  
- High sparsity in intermittent series.  
- ML lag-based models infeasible due to insufficient length.
---

## 1. `project1_data.ipynb`

### **Purpose**
Processes raw store–item sales data and constructs the demand classification used throughout the forecasting analysis.

### **Key Functions**
- **Data loading and preprocessing**  
  Cleans missing values, handles zero-inflation, aggregates sales to monthly frequency.
- **Feature engineering**  
  Computes:
  - zero-share  
  - average inter-demand interval (ADI)  
  - autocorrelation at 12 months (ACF12)  
  - other statistical indicators
- **Demand pattern classification**  
  Applies established rules to assign each series to one of:
  - **intermittent**
  - **regular**
  - **seasonal**

### **Outputs**
- A processed dataset containing:
  - store ID, item ID
  - cleaned sales series
  - engineered features
  - **segment label** (intermittent / regular / seasonal)
- This processed dataset serves as the input to `project1_analysis.ipynb`.

---

## 2. `project1_analysis.ipynb`

### **Purpose**
Implements forecasting models, produces predictions for each demand pattern, and evaluates performance using a comprehensive set of error metrics.

### **Models Implemented**
- **Croston's method** (intermittent)
- **SARIMA** (all patterns, especially regular + seasonal)
- **Prophet** (seasonal)
- Additional attempts (may be skipped if insufficient data):
  - Exponential Smoothing  
  - MLR, Random Forest, XGBoost (lag-based ML models)

### **Evaluation Metrics**
The notebook computes:

- Mean Absolute Percentage Error (**MAPE**)  
- Root Mean Square Error (**RMSE**)  
- Mean Absolute Error (**MAE**)  
- Symmetric MAPE (**sMAPE**)  
- Forecast **Bias**  
- Mean Absolute Scaled Error (**MASE**)  
- Model **Win-Counts** (number of series where a model achieves lowest MAPE)

### **Visualizations**
Generates all figures used in the Results section, including:
- Error distribution plots (overall and within-pattern)
- Mean error barplots
- Win-count comparison
- sMAPE and ​:contentReference[oaicite:0]{index=0}​


## Analysis Workflow

### Step 1 — Data Processing
- Load → clean → aggregate  
- Compute ADI, zero-share, ACF12  
- Label each series  
- Save as processed dataset  

### Step 2 — Forecasting
- Load processed data  
- Fit appropriate models per pattern  
- Generate out-of-sample predictions  
- Compute metrics and visualize results  

## Key Results

### Intermittent Demand
- Croston achieves lowest magnitude errors across all metrics.  
- SARIMA under-forecasts and performs inconsistently.  
- Prophet not applicable.

### Regular Demand
- SARIMA provides stable and accurate forecasts.  
- No alternative models can generate feasible outputs.

### Seasonal Demand
- SARIMA outperforms Prophet.  
- Prophet shows high error variance and strong negative bias.

### Overall
**Pattern-specific model selection is essential for accurate forecasting.**

## Installation & Setup

### Requirements
- Python 3.8+  
- Jupyter Notebook  
- Libraries:  
  `pandas`, `numpy`, `statsmodels`, `prophet`, `matplotlib`, `seaborn`, `scikit-learn`

### Setup
```bash
git clone <repo-url>

File Descriptions
project1_data.ipynb

Data cleaning and aggregation

Feature computation

Demand pattern classification

Output of processed datasets

project1_analysis.ipynb

Model fitting and forecasting

Evaluation metrics

Plot generation for the Results section

Reproducibility

Fixed random seeds

Deterministic SARIMA specification

Fully documented computational workflow

Version-controlled data and code

Limitations

Short time series constrain seasonal model performance

High sparsity prevents ML approaches

Prophet unstable under incomplete seasonal cycles

Contact

For questions, please contact the project author.
