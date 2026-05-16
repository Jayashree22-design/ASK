# ASK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
Google Colab
# Program
```
# ASK
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

Low-pass filter
def lp(x, c, fs):
    b, a = butter(5, c/(0.5*fs), btype='low')
    return lfilter(b, a, x)

 Parameters
fs, f1, f2, br, T = 1000, 30, 70, 10, 1
t = np.linspace(0, T, fs*T, endpoint=False)

Message
bits = np.random.randint(0, 2, br)
spb = fs // br
msg = np.repeat(bits, spb)

 FSK Modulation
fsk = np.zeros_like(t)
for i, b in enumerate(bits):
    f = f2 if b else f1
    fsk[i*spb:(i+1)*spb] = np.sin(2*np.pi*f*t[i*spb:(i+1)*spb])

Demodulation (Correlation)
ref1 = np.sin(2*np.pi*f1*t)
ref2 = np.sin(2*np.pi*f2*t)
c1 = lp(fsk*ref1, f1, fs)
c2 = lp(fsk*ref2, f2, fs)

decoded = []
for i in range(br):
    s, e = i*spb, (i+1)*spb
    decoded.append(1 if np.sum(c2[s:e]**2) > np.sum(c1[s:e]**2) else 0)

demod = np.repeat(decoded, spb)

Plot
plt.figure(figsize=(10,10))
plt.subplot(511);
 plt.plot(t, msg);
plt.title("Message"); 
plt.grid()
plt.subplot(512); 
plt.plot(t, np.sin(2*np.pi*f1*t)); 
plt.title("Carrier f1"); 
plt.grid()
plt.subplot(513); 
plt.plot(t, np.sin(2*np.pi*f2*t)); 
plt.title("Carrier f2"); plt.grid()
plt.subplot(514); plt.plot(t, fsk);
 plt.title("FSK Signal"); 
plt.grid()
plt.subplot(515); 
plt.plot(t, demod); 
plt.title("Demodulated"); 
plt.grid()
plt.tight_layout(); 
plt.show()

# FSK

import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs,f1,f2,br,T = 1000,30,70,10,1
t = np.linspace(0,T,fs*T,endpoint=False)
bits = np.random.randint(0,2,br)
spb = fs//br
msg = np.repeat(bits,spb)

FSK
fsk = np.zeros_like(t)
for i,b in enumerate(bits):
    fsk[i*spb:(i+1)*spb] = np.sin(2*np.pi*(f2 if b else f1)*t[i*spb:(i+1)*spb])
Demod
b,a = butter(5,f1/(0.5*fs),'low')
c1 = lfilter(b,a,fsk*np.sin(2*np.pi*f1*t))
b,a = butter(5,f2/(0.5*fs),'low')
c2 = lfilter(b,a,fsk*np.sin(2*np.pi*f2*t))

dec = [(np.sum(c2[i*spb:(i+1)*spb]**2) >
        np.sum(c1[i*spb:(i+1)*spb]**2))*1 for i in range(br)]
demod = np.repeat(dec,spb)

Plot
plt.figure(figsize=(9,9))
plt.subplot(511);
plt.plot(t,msg);
plt.title("Message");
plt.grid()
plt.subplot(512);
plt.plot(t,np.sin(2*np.pi*f1*t));
plt.title("Carrier f1"); plt.grid()
plt.subplot(513);
plt.plot(t,np.sin(2*np.pi*f2*t));
plt.title("Carrier f2");
plt.grid()
plt.subplot(514);
plt.plot(t,fsk);
plt.title("FSK");
plt.grid()
plt.subplot(515);
plt.plot(t,demod);
plt.title("Demodulated");
plt.grid()
plt.tight_layout();
plt.show()
```
# Output Waveform

# ASK
<img width="679" height="679" alt="image" src="https://github.com/user-attachments/assets/5f5528a8-7fe9-43f6-972b-aaf53b878804" />

# FSK
<img width="692" height="393" alt="image" src="https://github.com/user-attachments/assets/88f21e8d-e23a-4057-ad56-5ef6f51a29fc" />
<img width="691" height="276" alt="image" src="https://github.com/user-attachments/assets/9cd50c6a-7ddd-4ab9-a7dd-74c15416feec" />

# Results
```
Thus the reqiuired output is obtained .
```
