# AI for Low-Carbon Energy Scheduling

Forecasting electricity grid carbon intensity and recommending cleaner time windows for energy-intensive tasks.

## Project Overview
This project tackles the challenge of reducing carbon emissions in energy systems through intelligent scheduling. The core goal is to **forecast Grid Carbon Intensity (gCO2/kWh)** using three distinct classes of models and use those forecasts to recommend optimal, lower-carbon time windows for running electricity-intensive tasks.

### Core Objectives
1. **Forecast Carbon Intensity**: Predict the "cleanliness" of the grid using XGBoost, LSTM, and Physics-Informed Neural Networks (PINNs).
2. **Comparative Robustness Analysis**: Evaluate models across different data volumes (5, 30, and 60 blocks) to find the accuracy/data-size sweet spot.
3. **Low-Carbon Scheduling**: A recommendation engine that identifies cleaner time windows, quantified by the **Avoided Carbon Potential (ACP)** metric.
4. **Efficiency Analysis**: Evaluate energy efficiency trade-offs via model compression (weight pruning) and latency benchmarking.

## Repository Structure
```
.
├── data_and_preprocessing.ipynb        # 1. Data pipeline merging demand with carbon intensity
├── exploratory_analysis.ipynb          # 2. Visualization of diurnal carbon cycles and autocorrelation
├── modelling_xgb_lstm.ipynb            # 3. Training baseline forecasting models
├── modelling_pinn_and_comparison.ipynb # 4. Physics-Informed models and the Low-Carbon Scheduler
├── efficiency_and_compression.ipynb    # 5. Accuracy-Efficiency Pareto frontier evaluation
├── data/processed/                     # Cleaned train/val/test splits used by the notebooks
├── models/                             # Saved model weights/scalers produced by the notebooks
├── figures/                            # Generated plots and visualizations
├── archive/                            # Raw Kaggle dataset (not tracked in git, see below)
└── requirements.txt
```
Run the notebooks in the numbered order above.

## Data Source
The project uses the **London Smart Meter Dataset** (Kaggle), aggregated across 112 CSV blocks. For this analysis, a 60-block subset was used to ensure statistical robustness.

> **Note:** The raw `archive/` folder (~9.6 GB) is not tracked in this repository due to its size. To reproduce the pipeline from scratch, download the [London Smart Meter Dataset](https://www.kaggle.com/datasets/jeanmidev/smart-meters-in-london) from Kaggle and extract it into an `archive/` folder at the project root before running `data_and_preprocessing.ipynb`. The processed outputs used by later notebooks are already included under `data/processed/`.

### Data Dictionary
| Variable | Unit | Type | Description |
| :--- | :--- | :--- | :--- |
| `timestamp` | Datetime | Index | 30-minute intervals (half-hourly). |
| `carbon_intensity` | gCO2/kWh | Target | The "cleanliness" of the grid (goal: minimize). |
| `energy` | kWh/hh | Feature | Aggregated electricity demand from households. |
| `hour` | 0-23 | Feature | Categorical-ordinal time feature. |
| `day_of_week` | 0-6 | Feature | Categorical-ordinal weekday feature. |
| `intensity_lag_30m`| gCO2/kWh | Feature | Intensity value from 30 minutes prior. |
| `intensity_rolling_mean_6h` | gCO2/kWh | Feature | 6-hour smoothed grid trend. |
| `demand_lag_30m` | kWh | Feature | Recent demand as a grid load proxy. |

## Setup
```bash
git clone https://github.com/Deepakraj03/AI-for-low-carbon-energy-scheduling.git
cd AI-for-low-carbon-energy-scheduling
pip install -r requirements.txt
```

## License
This project is licensed under the [MIT License](LICENSE).
