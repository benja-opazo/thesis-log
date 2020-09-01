# Matlab WORLD

There are two WORLD[1] vocoders, one using the Harvest method for f0 estimation, and one using the DIO method (that can be daisy chained with the StoneMask method).

 - The **Harvest Method** apparently does not work in real-time, but its output is almost perfect
 -  The **DIO + StoneMask Method** works in real-time but the quality degrades

Tinkering with the options, the DIO performance can improve if the ```frame_period``` option is set to a lower value. The ```frame_period``` option is the time in ms between consecutive filters.

## **DIO [2] (MatLab):**

### Preprocessing
> In this step  the different parameters are calculated, along with some useful calculations (downsampling, spectrum)

 The script processes the options (```f0_floor, f0_ceil, channels_in_octave, target_fs, frame_period, allowed_range```)
 * **f0_floor** and **f0_ceil** indicates the boundaries of the filters
 * **channels_in_octave** indicates the number of filter per octave
 * **target_fs** is the frequency the input will be sampled (can be different, as a downsizing occurs)
 * **frame_period**: (WIP) indicates the length of each frame where the algorithm is applied
 * **allowed_range**: (WIP)
          
The script calculates the ```temporal_positions``` and the ```boundary_f0_list```.
* **temporal_positions**: are the timestamps between each ```frame_period```
* **boundary_f0_list**: are the frequencies at which the signal will be filtered, depending on the ```f0_floor```, ```f0_ceil``` and ```channels_in_octave```
 
 The signal is then downsampled to the ```target_fs```, the mean is substracted (why?? $\rightarrow$ DC = 0). After downsampling, the spectrum is calculated, filtering low frequencies (```cutoff_in_sample```)
 
 ### Fundamentalness calculation
> **Fundamentalness** is defined as the variance of negative and positive going zero-crossing intervals and the intervals between successive peaks and dips
 
 The signal is first filtered using a Nuttall Window for each ```boundary_f0```.
 
 The algorithm calculates the negative (+ to -) and positive zero cross, along with peaks and dips (valleys). 
 
> To calculate the Zero cross, the signal is shifted, multiplied by itself and then the negative results are the zero crossing.
> e.g.: ([+ ==+ -== -] X [==+ -== - ?] = [+ ==-== + +])
 
The Algorithm linearly interpolates the position (which is not necessarily an integer) where the "real" zero crossing happens (```fine_edge_list```)

Calculates the midpoint (in seconds) between each zero crossing (```interval_locations```), using the ```fine_edge_list```

Calculates the sample length between each zero crossing, which not necessarily is an integer number, and then gets the frequency associated to that length (```interval_based_f0```), using the ```fine_edge_list```

> For peaks and valleys, the same procedure is applied to the derivative of the signal

Now we have a list of two important values, the frequency that the zero crossing have, and the midpoint of each zero crossing interval. This is a function $interval(t)$, where $t$ is in seconds (not in samples). We can interpolate the values of $interval(t)$ for each ```temporal_position```, and we get the estimated value of the frequency between each zero crossing for each frame, **assuming that the change of zero crossing frequency is continuous** (which obviously is not).

That is exactly what the algorithm do, for the negative and positive zero crossings, along with the peaks and dips of the signal.

For each interpolated value the mean and the standard deviation is calculated. (```f0_candidate``` and ```f0_score``` respectively)

Some scores do not make sense, for example, if the ```f0_candidate``` is higher than the allowed ```f0_ceil```, lower than the allowed ```f0_floor``` or out of the boundary that the score is being calculated to.

The ```raw_score``` is then calculated as

\begin{equation}
    \text{raw_f0_score} = e^{-(f0\_score/f0\_candidate)}
\end{equation}

The lower the std deviation is (```f0_score```) the better.

> Maybe this score can be improved

The candidates are then sorted, and the best candidate is fixed using a 4 step procedure:

* Step 1: Fixes rapid changes of f0 contour, replacing them by 0
* Step 2: short-time voiced period (under ```voice_range_minimum```) is replaced by 0
* Step 3: ??
* Step 4: ??

### Output

The outputs of the script are

* ```f0_parameter```: A struct containing
    * ```f0```: An array containing the predicted f0
    * ```f0_candidates```: A matrix containing the 7 best f0 candidates for each frame. The first row corresponds to ```f0_parameter.f0``` before it is fixed
    * ```temporal_positions```: An array containing the positions where the algorithm was applied
    * ```vuv```: An array containing the positions of the voiced and unvoiced sections

## StoneMask (MatLab)

WIP

## **CheapTrick [3] (MatLab):**

### Preprocessing

> In this step  the different parameters are calculated (either the default values or the values given by the options), the value of ```q1``` is fixed. There are extra values that are taken into account from the ```source_object``` struct.

 The algorithm processes the options (```q1``` and ```fft_size```)
 * **q1**: A parameter that is used for the spectral recovery.
 * **fft_size**: The size of the fft, that limits the lowest F0 that WORLD can work with, given by $3f_s/\text{fft_size}$.

The ```source_object``` struct corresponds to the output of the DIO algorithm, that is ```f0```, ```f0_candidates```, ```temporal_positions``` and```vuv```.

The algorithm defaults the ```f0``` value to ```default_f0``` if no voice is detected (when ```vuv == 0```). 

## Spectrogram Calculation

> This step corresponds to the whole CheapTrick algorithm

The algorithm can be divided into four steps, in the first step, the waveform is windowed with a Hanning window, the second step obtains the Power Spectrum of the windowed waveform, on the third step the power spectrum is smoothed by filtering with a rectangular window and the last step obtains the spectral envelope by liftering in the quefrency domain.

### Windowed Waveform

> The signal is windowed with a Hanning window

To window the waveform, the algorithm first calculates some parameters
* **half_window_length**: Due to the length of the window being $3T_0$, this parameter is equal to $1.5f_s/f_0$ rounded to the nearest integer.
* **base_index**: This parameter corresponds to the samples index of the window. It is the array ```[-half_window_length:half_window_length]```
* **index**: This value positions the hanning window in the actual sample where the window is being applied on the original signal.
* **safe_index**: the same as ```index``` but the algorithm makes sure that the parameter is not out of bounds.
* **time_axis**: A scaled version of ```base_index```, divided by $1.5fs$

The script then calculates a [Hanning Window](https://en.wikipedia.org/wiki/Hann_function) (or Hann), which is of the form

\begin{equation}
 W[n] = \frac{1}{2} + \frac{1}{2}cos(\pi*n)
\end{equation}

Where $n$ is a discrete time parameter,  which corresponds to the multiplication of ```time_axis``` with ```current_f0```, then, it ranges from $-1$ to $1$ with $2*\text{half_window_length}+1$ elements.

The window is then normalized by its [Euclidean Norm](https://en.wikipedia.org/wiki/Norm_(mathematics)#Euclidean_norm)

\begin{equation}
    W_{norm}(t) = \frac{W[n]}{\sqrt{\sum_{n} W[n]^2 }}
\end{equation}

The last step is to window the waveform

\begin{equation}
    y[n] = x[n]w[n] - w[n]\frac{ \overline{x[n]w[n]} }{\overline{w[n]}}
\end{equation}

Where $\overline{f[n]}$ corresponds to the mean of the function.

The second term in the windowing is used to filter the DC component of the waveform ($DC = 0$)

### Power Spectrum

> The power spectrum is calculated and a DC correction is applied, where the DC value goes from being $0$ to being the same value as in ```f0 + fs/fft_size```.

To get the [power spectrum](https://en.wikipedia.org/wiki/Spectral_density), the spectrum is squared and then calculated the magnitude

\begin{equation}
    S_{w}(f) = |\hat{y}[n]|^2
\end{equation}

Where $\hat{y}[n]$ corresponds to the Fourier Transform of $y[n]$.

Afterwards, the script applies a so called "DC correction", where the power spectrum on frequencies lower than ```f0 + fs/fft_size``` is added to the same spectrum but with the frequency axis flipped, that way on DC, the value of the power spectrum is the same value as in ```frequency = f0 + fs/fft_size``` (because before this correction $DC = 0$), and the power spectrum is continuous. 

> Note that the "overshoot" given by fs/fft_size is equivalent to one frequency "bin"

This correction is applied to the firsts values of the ```power_spectrum``` array, in order to maintain symmetry, the scripts copies the first half of the power spectrum, flips it on the frequency axis and replaces the second half of the power spectrum with this corrected power spectrum. The same results can be obtained by applying the previous steps to the last values of the array (instead of flipping the array and replacing the values).

### Power Spectrum Smoothing

> In this step the Spectrum is smoothed simply by filtering with a rectangular window. The script is optimized to work in real time


To smooth the power spectrum, the following filtering is applied

\begin{equation}
    P_s(\omega) = \frac{3}{2\omega_0} \int_{-\omega_0/3} ^{\omega_0/3} P(\omega + \lambda) d\lambda
\end{equation}

Note that doing this the naive way is inefficient. To smooth the spectrum, the script does the following:

1. Concatenates the spectrum with itself. With the spectrum being periodic, it is giving leeway to use the neighboring parts of the spectrum (it can also wrap around the array)
2. Calculates the cumulative sum of the spectrum (```cumsum()```) and multiplies this array with $\Delta \omega$ which in this case is ```fs/fft_size```. Note that the result is the approximate cumulative area of the power spectrum, its integral! (to be more precise, its [Left Riemann Sum](https://en.wikipedia.org/wiki/Riemann_sum#Left_Riemann_sum))
3. Interpolates the values of the cumulative sum starting from $\omega = - f_0/3$, and in ```fs/fft_size``` intervals for a total of ```fft_size``` values. This variable is called ```low_levels```
4. Interpolates the values of the cumulative sum starting from $\omega = f0/3$, same as before. This variable is called ```high_levels```
5. The smoothed spectrum is then ```(high_levels - low_levels) * 1.5 / f0```
6. To avoid values on the spectrum that are ```0```, a noise of amplitude [eps](https://www.mathworks.com/help/matlab/ref/eps.html) is added.

In the second step an approximation of the cumulative area is calculated, but we want the integral between $\omega -\omega_0/3$ and $\omega + \omega_0/3$. To fix this, we substract the integral at those boundaries. We then face two problems:
1. The frequency at $\omega \pm \omega_0/3$  is not necessarily a data point we have, hence the interpolation
2. We have to do this for **every** frequency

The solution is to interpolate once for each shifting ($\pm\omega_0$) and substract one from the other, this way, the interpolated arrays are shifted accordingly, and it is done for every frequency once.

Note that as a result of the interpolation and this particular implementation, the output of this function has a power spectrum in the interval $[0, fs]$

> The matlab scripts implements a custom ```interp1()``` function called ```interp1H()```, which is an implementation of a linear interpolation that returns 0 when the values to be interpolated are outside of the values of the known function (i.e.: extrapolation) instead of the ```NaN``` value that MatLab returns in this case


### Spectral Envelope

> This step is the heart of the algorithm, it calculates the spectral envelope by liftering it in the quefrency domain.

To obtain the spectral envelope, a liftering in the quefrency domain is done as follows

\begin{equation}
     P_l(\omega) = \exp(\tilde{q}_0 \log(P(\omega)) + \tilde{q}_1 \log(P(\omega + \omega_0)P(\omega - \omega_0)))
\end{equation}

Where $P_l(\omega)$ is the output that we are looking for. and $P(\omega)$ represents the power spectrum calculated in the previous step. The script calculates the following set of equations

\begin{align}
    P_l(\omega) &= \exp(\mathcal{F} [l_s(\tau)l_q(\tau)p_s(\tau)]) \\
    l_s(\tau) &= \frac{\sin(\pi f_0 \tau)}{\pi f_0 \tau} \\
    l_q(\tau) &= \tilde{q}_0 + 2 \tilde{q}_1\cos(2\pi f_0 \tau) \\
    p_s(\tau) &= \mathcal{F}^{-1}[\log(P_s(\omega))]
\end{align}

Where $\tilde{q}_0$ and $\tilde{q}_1$ are adjustable parameters of the algorithm, and in this implementation $\tilde{q}_0 = 1 - 2\tilde{q}_1$

In the particular case of the script, the output of the previous step has a power spectrum in the interval $[0, fs]$, whereas the input assumes that the spectrum is in the interval $[0, 2fs]$ , but the output is trimmed to be defined in the interval $[0, fs]$.

> Note that the output of this sections is in the frequency domain, not the quefrency domain

### Output

The output of the script is a single struct called ```spectrum_parameter```

* ```spectrum_parameter```: A struct containing
    * ```temporal_positions```: An array containing the positions where the algorithm was applied
    * ```spectrogram```: An array containing the smoothed spectrum in the frequency domain
    * ```fs```: A variable containing the sampling frequency

## References

[1] M. Morise, F. Yokomori, and K. Ozawa, “WORLD: A Vocoder-Based High-Quality Speech Synthesis System for Real-Time Applications,” IEICE Trans. Inf. & Syst., vol. E99.D, no. 7, pp. 1877–1884, 2016.

[2] M. Morise, H. Kawahara and H. Katayose: Fast and reliable F0 estimation method based on the period extraction of vocal fold vibration of singing voice and speech, AES 35th International Conference, CD-ROM Proceeding, Feb. 2009.

[3] M. Morise, “CheapTrick, a spectral envelope estimator for high-quality speech synthesis,” Speech Communication, vol. 67, pp. 1–7, Mar. 2015, doi: 10.1016/j.specom.2014.09.003. 


## Appendix

WIP

### Appendix A: Filtering with the Nuttal Window

WIP

### Appendix B: CheapTrick Pitch Synchronous Analysis

CheapTrick uses a Hanning window with a length of $3T_0$, where $T_0$ represents the fundamental period of the signal (related to $F_0$). The overall power of a periodic signal $y(t)$ windowed by the Hanning window $w(t)$ is

\begin{equation}
    \int_{0}^{3T_0}(y(t)w(t))^2dt = 1.125 \int_{0}^{T_0}y^2(t)dt
\end{equation}

This equation shows that the overall power of the periodic signal windowed by the window is temporally stable.