# Easy Introduction to Wavelets
https://www.youtube.com/watch?v=ZnmvUCtUAEE

## Basics
- Fourier Transform vs Wavelet Transform(compact support, area integral = 0)
- analyzing function
- FT output coefficients correspond to frequency (1-D matrix with identification of frequency, results in a 2-D plot)
- WT output 2x2 matrix of coefficients correspond to **scale** and **translation** (2-D matrix with identification of scale and translation, results in a 3-D plot)
- Translation and Scale
  - Translation:
    - shifting wavelet forward in time (lead and lag) 
  - Scale: (like scales in music class, notes with increasing or decreasing pitch)
    - stretch wavelet out for lower frequency, compressive for higher frequency
      - low scale = wavelet more compressed = high-frequency scale
      - high scale = wavelet more stretched out = low-frequency scale
    - VIP
      - stretch out wavelet for lower frequency, **length** of wavelet will increase > at lower frequencies, less accurate identifying the time at which a particular low frequency occurs
      - higher frequency wavelets(much shorter) will have better localization in time

## Criteria for choosing Wavelet function, trade-offs
- frequency and time domain **resolution**
  - Heisenbery uncertainty principle
    - cannot be both extremely accurate about location in time and location in frequency
    - Heisenbery boxes
      - all boxes MUST maintain the same area
        - higher frequency (lower scale): (shorter period, narrower time scales, boarder frequency scales) => higher time resolution(better), but lower frequency resolution(worse)
        - lower frequency (higher scale): (longer period, boarder time scales, narrower frequency scales) => lower frequency resolution(worse), but higher frequency resolution(better)
      - high frequency wavelet(low scale): more compact in time, support for achieving high resolution in time, trade-off low resolution in frequency
      - low frequency wavelet(high scale): more compact in frequency, support for achieving high resolution in frequency, trade-off low resolution in time
    - music interpretation
      - ascending pitch, frequency doesn't increase linearly, but increase in octave, doubling the frequency
      - with bass music instrument: often playing low frequency notes for longer duration (16-th notes)
    - Fourier Transform: no time resolution, but high frequency resolution => Windowed Fourier Transform
- Vanishing Moments (k)
  - wavelet with higher vanishing moments = more complex wavelet
    - more accurate representation of complex signal
    - Disadvantage: longer support
    - p vanishing moments: polynomials upto the p-th order will not be identified by the wavelet
- Regularity
  - lower Regularity: wavelet with more jags (eg square shape, sawtooth shape)
  - higher Regularity: wavelet with smoother function (more Vanishing Moments = higher Regularity)
- Selectivity in Frequency
  - Heisenbery uncertainty:
    - more selective wavelet = less compact support (less selective in time)
    - Fourier Transform: very selective in frequency: each analyzing function can identifying only one frequency
    - Wavelet Transform: encompass a range of frequencies in little beat
    - for lower range of frequencies, need to be more selective
      - trade-off: Heisenbery uncertainty principle: the more selective, less compact support
