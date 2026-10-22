# Lab 5 — Motor Parameter Identification

This lab investigates how a motor responds to different power settings. A TRIK Studio program collects rotation data, and a Python notebook fits a mathematical model to describe the motor’s acceleration and steady speed.

## Data Collection

The graphical program controls **one motor (M4)** and reads its **encoder (E4)** using two nested loops:

- The outer loop changes PWM from −100% to +100% in steps of 5%.
- The inner loop records time and encoder angle for two seconds at each setting.

After each test, the motor stops and the buffered measurements are appended to a TXT file. New runs preserve previous recordings and begin with a `# NEW RUN` marker.

Each data row contains:

```text
PWM, battery_V, applied_voltage_V, time_ms, angle_deg
```

Applied voltage is estimated as:

```text
applied_voltage = PWM / 100 × battery_voltage
```

## Analysis

The notebook:

- Checks data completeness and timestamps.
- Converts measurements into seconds and radians.
- Estimates steady angular speed and motor time constant at each power.
- Fits the voltage–speed relationship to estimate the effective motor constant.
- Examines low-power motion and differences between forward and reverse.
- Compares recorded angle with a numerical simulation of the fitted model.

## Files

| File | Purpose |
|---|---|
| `Motor_identification_voltage.qrs` | TRIK Studio graphical collection program |
| `motor_single_voltage.txt` | Recorded data |
| `Lab5_motor_analysis.ipynb` | Analysis, parameter tables, graphs, and discussion |

## Run Locally

Place the notebook and TXT file in the same folder.

Install the dependencies:

```bash
python -m pip install jupyter numpy pandas scipy matplotlib
```

Open the notebook in Jupyter or VS Code and select **Run All**. If the TXT contains several runs, the notebook analyzes the last run by default.

Before collecting new data, check the motor ports, battery-voltage setting, and output path in the QRS project. Its current Windows output folder is `C:/Robotics/`, which must exist.

## Scope and Limitations

The included dataset was collected in the **TRIK Studio 2D simulator**, using a configured supply voltage of **11.2 V**. The voltage column is calculated rather than independently measured.

The results describe this single-motor simulation. They do not establish physical motor parameters or differences between two wheels. The model comparison uses the same data used for fitting; a separate experiment would be needed to evaluate predictive accuracy.
