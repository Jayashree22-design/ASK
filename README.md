# ASK AND FSK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
Google Colab
# Program
# ASK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low')
    return lfilter(b, a, data)

# Parameters
fs = 1000              # Sampling frequency
fc = 50                # Carrier frequency
bit_rate = 10          # Number of bits
T = 1                  # Total duration

# Time axis
t = np.linspace(0, T, fs, endpoint=False)

# Binary message signal
bits = np.array([0,0,1,1,1,0,1,0,0,0])

bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

# Carrier signal
carrier = np.sin(2 * np.pi * fc * t)

# ASK Modulation
ask_signal = message_signal * carrier

# Demodulation
demodulated = ask_signal * carrier
filtered_signal = butter_lowpass_filter(demodulated, fc, fs)

# Decode bits
decoded_bits = (filtered_signal[::bit_duration] > 0.2).astype(int)

# Plotting
plt.figure(figsize=(12,8))

# Message signal
plt.subplot(4,1,1)
plt.plot(t, message_signal, color='blue')
plt.title("Message Signal")
plt.ylim(-0.1, 1.1)
plt.grid(True)

# Carrier signal
plt.subplot(4,1,2)
plt.plot(t, carrier, color='green')
plt.title("Carrier Signal")
plt.grid(True)

# ASK signal
plt.subplot(4,1,3)
plt.plot(t, ask_signal, color='red')
plt.title("ASK Modulated Signal")
plt.grid(True)

# Decoded bits
plt.subplot(4,1,4)
plt.step(range(len(decoded_bits)), decoded_bits,
         where='mid', color='red')

plt.plot(range(len(decoded_bits)),
         decoded_bits,
         'rx')

plt.title("Decoded Bits")
plt.ylim(-0.1,1.1)
plt.grid(True)

plt.tight_layout()
plt.show()
```
# FSK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low')
    return lfilter(b, a, data)

# Parameters
fs = 1000
f1 = 30
f2 = 70
bit_rate = 10
T = 1

# Time axis
t = np.linspace(0, T, int(fs*T), endpoint=False)

# Binary data
bits = np.array([1,1,1,1,1,0,1,1,0,0])

bit_duration = fs // bit_rate

# Message signal
message_signal = np.repeat(bits, bit_duration)

# Carrier signals
carrier_f1 = np.sin(2 * np.pi * f1 * t)
carrier_f2 = np.sin(2 * np.pi * f2 * t)

# FSK Modulation
fsk_signal = np.zeros_like(t)

for i, bit in enumerate(bits):

    start = i * bit_duration
    end = start + bit_duration

    if bit == 0:
        fsk_signal[start:end] = np.sin(
            2 * np.pi * f1 * t[start:end]
        )
    else:
        fsk_signal[start:end] = np.sin(
            2 * np.pi * f2 * t[start:end]
        )

# Demodulation
ref_f1 = np.sin(2 * np.pi * f1 * t)
ref_f2 = np.sin(2 * np.pi * f2 * t)

corr_f1 = butter_lowpass_filter(
    fsk_signal * ref_f1,
    f2,
    fs
)

corr_f2 = butter_lowpass_filter(
    fsk_signal * ref_f2,
    f2,
    fs
)

decoded_bits = []

for i in range(bit_rate):

    start = i * bit_duration
    end = start + bit_duration

    energy_f1 = np.sum(corr_f1[start:end] ** 2)
    energy_f2 = np.sum(corr_f2[start:end] ** 2)

    if energy_f2 > energy_f1:
        decoded_bits.append(1)
    else:
        decoded_bits.append(0)

decoded_bits = np.array(decoded_bits)

# Final demodulated signal
demodulated_signal = np.repeat(decoded_bits, bit_duration)

# Plotting
plt.figure(figsize=(10,10))

plt.subplot(5,1,1)
plt.plot(t, message_signal, color='blue')
plt.title("Message Signal")
plt.grid(True)

plt.subplot(5,1,2)
plt.plot(t, carrier_f1, color='green')
plt.title("Carrier Signal for bit = 0 (f1)")
plt.grid(True)

plt.subplot(5,1,3)
plt.plot(t, carrier_f2, color='red')
plt.title("Carrier Signal for bit = 1 (f2)")
plt.grid(True)

plt.subplot(5,1,4)
plt.plot(t, fsk_signal, color='magenta')
plt.title("FSK Modulated Signal")
plt.grid(True)

plt.subplot(5,1,5)
plt.plot(t, demodulated_signal, color='black')
plt.title("Final Demodulated Signal")
plt.grid(True)

plt.tight_layout()
plt.show()
```
# Output Waveform

# ASK
<img width="1190" height="790" alt="image" src="https://github.com/user-attachments/assets/12cf58f0-141a-4ab8-a7e9-5ec223eabd21" />

# FSK
<img width="989" height="989" alt="image" src="https://github.com/user-attachments/assets/a8053c76-fc04-4c08-9f82-d9abac297a36" />

# Results
```
Thus the reqiuired output is obtained .
```
