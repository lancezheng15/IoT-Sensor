#  Summary Report: IoT Sensor Temperature Analysis

##  Overview
This report summarizes the findings from analyzing temperature sensor data from two IoT devices over the period **April 13–20, 2025**. A deep learning model (LSTM) was trained to model temperature patterns and detect deviations.

---

##  Total Records Processed
- **Device 1**: ~10,000 one-minute intervals
- **Device 2**: ~10,000 one-minute intervals
- **Combined**: ~20,000 time steps over 8 days

---

##  Key Patterns Identified
- **Cycle Period**:  
  - Average time between temperature peaks: **~41.21 minutes**
- **Amplitude (Peak-to-Trough)**:  
  - Average fluctuation range: **~14.2°C**
- **Temperature Range**:  
  - Observed temperatures varied between **6°C and 22°C**

---

##  Deviation Detection Results
- **Deviation Threshold**: Absolute error > 2°C
- **Total 77 Deviations Detected**:  
 
- **Total Alerts Generated**: 77 across both devices

---

##  Model Performance (LSTM)
- **Input Sequence Length**: 90 minutes
- **Model Type**: LSTM with time-of-day phase features
- **Evaluation on April 20 Test Set**:
  - **Test MSE**: 0.7946
  - **Test R²**: 0.9748

---

##  Comparison Between Devices
- Both **Device 1 and Device 2** display nearly **identical periodic behavior**, suggesting they are similar machines or systems.
- **Cycle period and amplitude are consistent** across both devices.
- Minor timing offsets between devices, but no major deviations in shape or pattern.

---

##  Output Artifacts
- `temperature_output_device_device_1.csv`
- `temperature_output_device_device_2.csv`
- `timeseries.png`: Actual vs. predicted temperatures with deviation markers
- `cycle_pattern.png`: Predicted temperature over multiple cycles
- `error_histogram.png`: Histogram of prediction errors with threshold line
- `deviation_alerts.csv`: Tabulated list of deviation alerts

---
#  Comparison: Temperature vs. Motor Current Modeling with LSTM

##  Performance Summary

- **Temperature Data (April 13–20):**  
  - **LSTM Test MSE:** ~0.95  
  - **R² Score:** ~0.97  
  - The model successfully captured smooth, sinusoidal-like temperature cycles with high accuracy and minimal deviation.

- **Motor Current Data (April 13):**  
  - **LSTM Test MSE:** ~1.25  
  - **R² Score:** ~0.98  
  - Despite the sharp on/off transitions in current, the model performed well after careful resampling and sequence preparation.

---

##  Challenges Encountered

- **Sampling Frequency:**  
  - Motor current data was sampled at sub-second intervals, leading to extremely high volume.  
  - Resampling to 10-second intervals was necessary to reduce sequence size and prevent memory issues.

- **Data Characteristics:**  
  - **Temperature** shows **smooth** and continuous oscillations with moderate noise.  
  - **Motor Current** exhibits **binary-like pulse behavior** (flat + spikes) with abrupt changes, requiring the model to learn sharp transitions.

- **Model Generalization:**  
  - Temperature cycles were more forgiving to small prediction offsets.  
  - Current predictions were more sensitive to timing and peak alignment; even small delays can cause noticeable errors in amplitude.

---

##  Key Pattern Differences

| Feature             | Temperature         | Motor Current         |
|---------------------|---------------------|------------------------|
| **Cycle Shape**     | Smooth sine-like    | Sharp rectangular pulses |
| **Cycle Period**    | ~1440 min (daily)   | ~10 min intervals      |
| **Amplitude Range** | ~15–22°C            | 0–25 A                 |
| **Noise Level**     | Moderate            | Very low, except spikes |
| **LSTM Suitability**| Excellent           | Good, needs tuning     |

---

## Conclusion

While both datasets were successfully modeled using LSTM networks, the **temperature data** was more naturally suited due to its smooth temporal evolution. The **motor current**, though more discrete in nature, could still be effectively predicted with proper preprocessing. Key lessons included the importance of **resampling**, **sequence length tuning**, and understanding the **physical meaning behind the data patterns**.

