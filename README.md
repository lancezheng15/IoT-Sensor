# IoT Sensor Analysis Challenge

## Overview
This project analyzes time-series data from IoT sensors, focusing on temperature and motor current readings from two machines. The goal is to identify periodic patterns, detect deviations, and generate alerts using machine learning techniques.

---

## 1. Data Overview
- **Temperature Data**: Includes timestamped readings for two devices (Device 1 & 2).
- **Motor Current Data**: Includes timestamped current values (in amperes) for a single device.
- All data is clean with no missing values.

---

## 2. Exploratory Data Analysis (EDA)
- **Temperature Range**: Most values fall between 0°C and 40°C, with a few invalid values corrected using forward/backward fill.
- **Patterns**: Strong periodicity was observed, indicating repeated heating/cooling cycles.
- **Motor Current**: Exhibits a spike-like pattern with clear ON/OFF cycles, sampled more frequently than temperature.

---

## 3. Modeling

### Temperature
- **Approach**: LSTM with input sequence of 60 readings (about 10 minutes).
- **Training Window**: April 13–20
- **Test Results**:  
  - **MSE**: 0.9539  
  - **R² Score**: 0.9697  
- **Insights**:  
  - **Cycle Period**: ~40 minutes  
  - **Amplitude**: ~15°C  
  - **Deviations Detected**: ~100+ points with >2°C error

### Motor Current
- **Approach**: Resampled to 10s interval → LSTM model
- **Training Window**: April 13 only
- **Test Results**:  
  - **MSE**: 1.2551  
  - **R² Score**: 0.9859  
- **Insights**:  
  - **Pattern**: Repeating square-wave pattern indicating periodic motor activation  
  - **Cycle Frequency**: ~1 cycle every 2 minutes

---

## 4. Deviation Detection
- Deviation threshold: **2°C / 2A**
- New column `is_deviation` added:  
  - `1` for significant prediction gaps  
  - `0` otherwise
- Alerts generated with timestamps and deviation causes.

---

## 5. Visualizations
- `timeseries.png`: Actual vs. predicted with deviation markers
- `cycle_pattern.png`: Single-cycle illustration of periodic pattern
- `error_histogram.png`: Histogram of prediction errors with threshold overlay

---

## 6. Output
Each device's dataset exported with:
- `device_id`
- `timestamp`
- `temperature` / `motor_current`
- `predicted_temperature` / `predicted_current`
- `is_deviation`

---

## 7. Comparative Discussion

### Performance:
- **Motor current model** slightly outperforms the temperature model in R².
- Both capture periodic behavior effectively, but temperature is more gradual while motor current is binary-like.

### Challenges:
- **Temperature**: Slight underperformance at cycle troughs (coldest points).
- **Motor current**: Peaks/valleys are sharp and sensitive to timing, requiring fine resolution and careful windowing.

### Key Differences:
| Feature             | Temperature          | Motor Current         |
|---------------------|----------------------|------------------------|
| Signal Shape        | Smooth sinusoidal    | Square-like pulses     |
| Frequency           | Lower (per ~30-40min)| Higher (per ~2min)     |
| Data Volume         | ~100k records/day    | ~400k records/day      |
| Preprocessing       | Imputation required  | Resampling needed      |

---

## 8. Bonus – Motor Current Analysis
- Motor current predictions aligned well with actual values.
- Detectable cyclical pattern made it well-suited for LSTM modeling.
- Challenges included:
  - High-frequency noise
  - Steep transitions needing precise window sizing

---
