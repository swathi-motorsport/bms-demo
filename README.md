# bms-demo
import numpy as np
import matplotlib.pyplot as plt

# ----------------------------------------
# USER INPUTS
# ----------------------------------------

battery_capacity_ah = 5.0          # Battery capacity in Ampere-hours
initial_soc = 100                  # Starting SOC in %
dt = 1                             # Time step in seconds

# Example current profile (positive = discharge, negative = charge)
# You can replace this list with actual sensor data later
current_data = [1.5, 1.7, 2.0, 1.8, 0.5, -0.2, -0.5, 0, 0, 1.0, 1.2]

# ----------------------------------------
# SOC CALCULATION
# ----------------------------------------

soc_list = [initial_soc]
soc = initial_soc

for current in current_data:
    # Convert battery capacity from Ah → As (Ampere-seconds)
    capacity_as = battery_capacity_ah * 3600

    # ΔSOC = (current * dt / capacity) * 100
    delta_soc = (current * dt / capacity_as) * 100

    # Discharging reduces SOC, charging increases SOC
    soc = soc - delta_soc

    # Bound SOC between 0 and 100
    soc = max(0, min(100, soc))

    soc_list.append(soc)

# ----------------------------------------
# PLOT RESULTS
# ----------------------------------------

plt.figure(figsize=(8, 4))
plt.plot(soc_list, marker='o')
plt.title("SOC Estimation Using Coulomb Counting")
plt.xlabel("Time Step")
plt.ylabel("State of Charge (%)")
plt.grid(True)
plt.show()

# ----------------------------------------
# PRINT RESULTS
# ----------------------------------------

print("Final Estimated SOC:", round(soc_list[-1], 2), "%")
