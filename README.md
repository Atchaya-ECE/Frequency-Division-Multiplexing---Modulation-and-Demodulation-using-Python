# Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

# Aim:
To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.

# Apparatus Required:
* Google Colab (or any Python environment)
* Python libraries: numpy, matplotlib, scipy (scipy.signal)

# Theory:
FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

# Procedure:
1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband
# Program:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt

# -----------------------------------------
# PARAMETERS
# -----------------------------------------
fs = 50000               # Sampling frequency
t = np.arange(0, 0.05, 1/fs)

# Carrier frequencies
fc = [2000, 4000, 6000, 8000, 10000, 12000]

# -----------------------------------------
# 1. MESSAGE SIGNALS
# -----------------------------------------
messages = [
    0.8*np.sin(2*np.pi*50*t),
    0.8*np.sin(2*np.pi*100*t),
    0.8*np.sin(2*np.pi*150*t),
    0.8*np.sin(2*np.pi*200*t),
    0.8*np.sin(2*np.pi*250*t),
    0.8*np.sin(2*np.pi*300*t)
]

# -----------------------------------------
# 2. MODULATION (DSB-SC)
# -----------------------------------------
modulated = []
for i in range(6):
    modulated.append(messages[i] * np.cos(2*np.pi*fc[i]*t))

# -----------------------------------------
# 3. FDM COMPOSITE SIGNAL
# -----------------------------------------
fdm_signal = sum(modulated)

# -----------------------------------------
# 4. DEMODULATION
# -----------------------------------------
demod = []
for i in range(6):
    demod.append(fdm_signal * np.cos(2*np.pi*fc[i]*t) * 2)

# -----------------------------------------
# 5. LOW-PASS FILTER
# -----------------------------------------
def lowpass(signal, cutoff=500, fs=fs):
    b, a = butter(5, cutoff/(fs/2))
    return filtfilt(b, a, signal)

recovered = []
for i in range(6):
    recovered.append(lowpass(demod[i]))

# -----------------------------------------
# PLOTTING
# -----------------------------------------

# 1. Message signals (6 graphs in one figure)
plt.figure(figsize=(12, 12))
for i in range(6):
    plt.subplot(6, 1, i+1)
    plt.plot(t, messages[i])
    plt.title(f"Message Signal {i+1}")
    plt.xlabel("Time (s)")
    plt.ylabel("Amplitude")
plt.tight_layout()
plt.show()

# 2. Demodulated signals (6 graphs in one figure)
plt.figure(figsize=(12, 12))
for i in range(6):
    plt.subplot(6, 1, i+1)
    plt.plot(t, recovered[i])
    plt.title(f"Recovered (Demodulated) Signal {i+1}")
    plt.xlabel("Time (s)")
    plt.ylabel("Amplitude")
plt.tight_layout()
plt.show()

# 3. FDM Composite Signal
plt.figure(figsize=(12, 4))
plt.plot(t, fdm_signal)
plt.title("FDM Composite Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.tight_layout()
plt.show()
```
# Tabulation:
![WhatsApp Image 2025-11-26 at 15 17 35_21a4b057](https://github.com/user-attachments/assets/c750c45e-bdab-4da9-ba8e-994ec1277cc6)

![WhatsApp Image 2025-11-26 at 15 17 35_01a91934](https://github.com/user-attachments/assets/fbeea271-a1e8-4a0e-bb19-49db8aa2bc32)

# Output:
![WhatsApp Image 2025-11-26 at 14 37 49_7888c1c0](https://github.com/user-attachments/assets/2bdfcfc4-4123-4e23-ba7c-78b2e8858c6f)

![WhatsApp Image 2025-11-26 at 13 49 11_8d7fe287](https://github.com/user-attachments/assets/dc9c5011-91f8-4e0e-815f-9a632d8192f5)

![WhatsApp Image 2025-11-26 at 14 37 49_71b26c3f](https://github.com/user-attachments/assets/73a78193-a843-42be-a193-aa00acedff48)

# Result:
![WhatsApp Image 2025-11-26 at 15 18 59_b50604bb](https://github.com/user-attachments/assets/5027eca9-b381-4b31-a4e8-29c762ed6425)
