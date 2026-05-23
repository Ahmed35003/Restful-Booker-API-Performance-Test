# 🚀 Restful-Booker API Performance Testing Project

A comprehensive performance evaluation and scripting optimization project conducted using **Apache JMeter** on the **Restful-Booker** hotel booking API. 

This project explores system behavior across multiple execution profiles (Baseline, Load Optimized, Stress, and Spike testing) to analyze backend latency, scalability limitations, and concurrency behavior.

---

## 📂 Project Directory Structure
* **`final_script.jmx`**: The fully optimized, dynamic JMeter execution script.
* **`*.jtl`**: Raw execution logs holding timestamped tabular statistics for auditing (`load_baseline_results.jtl`, `load_optimized_results.jtl`, `stress_results.jtl`, `spike_results.jtl`).

---

## 🛠️ Performance Test Scenarios & Concurrency Profiles

### 1. Load Test (Baseline vs. Optimized)
* **Objective:** Establish baseline metrics and observe structural test improvements.
* **Setup & Concurrency:** Conducted with 20 concurrent virtual users.
* **Crucial Enhancement:** In the **Baseline** test, the authentication request (`Auth_01`) hammered the server **1,209 times** (once per transaction loop). By implementing a **Once Only Controller**, the **Optimized** run restricted authentication to exactly **20 times** (once per active user session). This reduced needless authorization traffic by **98.3%** and isolated actual booking transactional performance.

### 2. Stress Test
* **Objective:** Push the server limits to identify resource choking points and latency breakdown thresholds.
* **Setup & Concurrency:** Scaled up to 100 concurrent users.

### 3. Spike Test
* **Objective:** Evaluate how the backend handles sudden, massive traffic bursts.
* **Setup & Concurrency:** Immediate injection of a 60-user wave executing rapidly.

---

## 📊 Comprehensive Performance Metrics Summary

Here is the empirical consolidation of all test scenarios extracted directly from the JMeter HTML Dashboards:

| Metric / Scenario | Load Test (Baseline) | Load Test (Optimized) | Stress Test (100 Users) | Spike Test (60 Users Burst) |
| :--- | :---: | :---: | :---: | :---: |
| **Total Samples** | 4,793 | 4,677 | 10,537 | 4,773 |
| **Error Rate (%)** | 0.00% | 0.00% | 0.00% | 0.00% |
| **Global Throughput** | 14.21 / sec | 13.89 / sec | 52.81 / sec | 68.08 / sec |
| **Global Average RT** | 331.51 ms | 366.51 ms | 1174.88 ms | 491.85 ms |
| **Global 95th Percentile** | 842.30 ms | 836.00 ms | 2929.30 ms | 1264.00 ms |
| **Auth_01 Avg RT** | 758.45 ms | 784.75 ms | 1141.00 ms | 1904.20 ms |
| **CreateBooking_02 Avg RT** | 188.55 ms | 728.69 ms | 2208.81 ms | 934.04 ms |
| **CreateBooking_02 Max RT** | 515 ms | 1,149 ms | 31,682 ms | 21,715 ms |
| **GetBooking_03 Avg RT** | 186.88 ms | 180.25 ms | 654.91 ms | 238.47 ms |
| **UpdateBooking_04 Avg RT** | 187.02 ms | 182.14 ms | 643.92 ms | 237.12 ms |
| **Overall APDEX Score** | 0.873 | 0.833 | 0.456 | 0.808 |

---

## 🏃‍♂️ How to Run the Script
1. Clone this repository to your local workspace.
2. Open **Apache JMeter**.
3. Import the `final_script.jmx` file.
4. To execute the tests in non-GUI (CLI) mode for accurate results, navigate to the directory and run:
   ```bash
   jmeter -n -t final_script.jmx -l results.jtl -e -o Reports_Output/
