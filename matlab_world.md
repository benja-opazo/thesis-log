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
    * ```f0_candidates```: An array containing the best f0 candidates for each frame. The first row corresponds to ```f0_parameter.f0``` before it is fixed. The length of ```boundary_f0_list``` determines the number of rows of the array. 
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

### Spectrogram Calculation

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

## D4C (Band Aperiodicity Estimation)

WIP

## Get Seed Signal

> This script generates the parameters for the excitation signal generation

This script is based on the [Velvet Noise](http://users.spa.aalto.fi/mak/PUB/AES_Jarvelainen_velvet.pdf) and its [Frequency Domain Variant](https://arxiv.org/abs/1806.06812).

The general idea is to generate a sequence of the form

\begin{equation}
    k(m) = \|mT_d + r_1(m)(T_d-1)\|
\end{equation}

Where $r_1(m)$ and $r_2(m)$ are generated from a uniform distribution in $(0,1)$, $\| \bullet \|$ returns the nearest integer, and $T_d$ represents the average pulse interval in samples. Then, a signal is generated that follows this equation

\begin{equation}
    s(n) = 
    \begin{cases}
        2\|r_2(m)\| - 1 &n = k(m) \\
        0 &\text{otherwise}
    \end{cases}
\end{equation}

The way the scripts implements it is different, but it is based on the same principle

### Preprocessing

The first step of the script is to calculate the parameters to be used. Some of the parameters are
- ```fft_size```: Corresponds to the size of the ffts to be calculated. it depends on ```fs```.
- ```noise_length```: Corresponds to the length of the velvet noise. It is the nearest power of 2 of ```fs```.
- ```w```:
- ```frequency_interval``` Bandwidth by which the output noise is filtered with each bandpass filter

### Generate Modified Velvet Noise
> This function generates a velvet noise of length ```noise_length```

To generate the velvet noise, this script calls another script which generates a short Velvet Noise. The length of this short Velvet Noise is chosen randomly between 3 values:
- ```short_period =  8 * round( [8, 30, 60] * fs / 48000);```

In the case where ```fs = 48000```, the values are ```[64,240,480]```.

The short Velvet Noises are appended one after another until the length of the long Velvet Noise is at least ```noise_length```. Then, the long Velvet Noise is trimmed to be of length ```noise_length```.

The output noise is divided every ```frequency_interval``` Hertz, for example, if ```frequency_interval``` is 3000, then the noise is filtered with bandpass of bandwidth 3KHz, every 3KHz

### Short Velvet Noise

To generate the Short Velvet Noise, the script first generates a signal ```safety_rand``` where the first half is 2 and the second half is -2. then, it randomnly swaps samples from the signal until every sample has been swapped at least once. At this point, ```safety_rand``` is a seemingly random sequence of 2 and -2. Note that the definition of Velvet Noise is a sparse discrete signal, with many 0 values.

To randomly add the missing zeroes, the output signal is a vector filled with zeroes, that randomly and sparsely picks samples from the ```safety_rand``` signal. The sparseness is defined by the ```td``` variable, which defaults to 4, that means, that in average, the distance of the pulses (non zero samples) is 4.

### Pulse generation

> The script generates several pulses that are used as excitation signals for the voice synthesis. 

On the previous scripts, the frequency has been divided on frequency ranges, given a frequency interval. For example if the signal is sampled at 48 KHz, and the frequency interval is 3 KHz, then the frequency scale is divided by 8 intervals given by $\frac{48000}{2*3000}$. The script limits the maximum ```fs``` to $30000$, which gives in this case 5 frequency intervals. The script also adds 2 intervals, then the number of intervals in this case is $7$. This is stored on the ```number_of_aperiodicities``` variable.

Once the intervals are selected, different pulses are generated, by first synthesizing their spectra, given by

\begin{equation}
    S(\omega) = \frac{1}{2} \left( 1 + cos( \omega_0/\omega_r) \right)
\end{equation}

Where $\omega_0$ is the center frequency of each frequency interval, and $\omega_r$ is the frequency range. The pulses are generated by inverting the spectrum.

\begin{equation}
    s(t) = \mathcal{F}^{-1}(S(\omega))
\end{equation}

The **velvet noise** that was generated previously is filtered with each of the pulses spectrum $S(\omega)$, which acts as a passband filter.

### Output

The output is a single struct called ```seed_signals```:
- ```seed_signals```:
    - ```pulse```: An array that contains the generated pulses. It has ```number_of_aperiodicities``` rows.
    - ```noise```: An array that contains the filtered **velvet noises** with the spectrum of the pulses. It has ```number_of_aperiodicities``` rows.

The rows of the ```pulse``` and ```noise``` variables correspond to the same frequency range.

## Synthesis

> According to the paper, in this section the vocal cord vibration is calculated on the basis of the convolution of the minimum phase response and the extracted excitation signal [1]. On the script, after obtaining the excitation signal, the waveform is calculated based on the [overlap-add method](https://en.wikipedia.org/wiki/Overlap%E2%80%93add_method)

The input variables of this script are ```source_object```, ```filter_object``` and ```seeds_signals```.

Where ```source_object``` is a struct containing

* ```f0```: An array containing the predicted f0
* ```raw_f0_candidates```: An array containing the best f0 candidates for each frame. The first row corresponds to ```f0_parameter.f0``` before it is fixed. The number f0 candidates depends on the options given on the DIO script.
* ```temporal_positions```: An array containing the positions where the algorithm was applied
* ```vuv```: An array containing the positions of the voiced and unvoiced sections
* ```band_aperiodicity```: An array that contains the band aperiodicity components given by the D4C script. The first row corresponds to a (need checking) ```vuv``` array with different sensibility

```filter_object``` is a struct containing information about the spectral envelope. Its elements are
- ```temporal_positions```: An array containing the positions where the algorithm was applied
- ```spectrogram```: An array containing the smoothed spectrum in the frequency domain
- ```fs```: A variable containing the sampling frequency

The ```seed_signals``` are the parameters for excitation signal generation. Its elements are

- ```pulse```: An array that contains the generated pulses. It has ```band_aperiodicity``` rows.
- ```noise```: An array that contains the filtered **velvet noises** with the spectrum of the pulses. It has ```band_aperiodicity```

### Excitation Signal
> This script generates an excitation signal that is used as an input to generate the waveform

On the unvoiced parts of the signal, the excitation signal is a velvet noise, whereas on the voiced parts of the signal, the excitation signal corresponds to pulses given by the f0 (periodic component) and the band aperiodicity (aperiodic components)

First, the script calculates two values ```pulse_location_index``` and ```interpolated_vuv```, the first variable corresponds to the estimated glottal pulses (based on the f0 values) and the second variable corresponds to the ```vuv``` values interpolated to match the ```time_axis```. To calculate the ```pulse_location_index```, the script interpolates ```f0``` to match the  ```time_axis```, then calculates the ```total_phase``` of ```f0```, which is simply the sum of the ```f0``` on each sample, then wraps the ```total_phase``` from $0$ to $2\pi$. At this point, the wrapped phase is a waveform with many pulses each with ```f0``` frequency. Then it calculates the samples where the pulses are maximum and that corresponds to the ```pulse_location_index```. It is important to add that the scripts defaults the value of ```f0``` to that of ```default_f0``` (500 Hz) on the unvoiced parts of the signal.

The script then resamples the ```band_aperiodicity``` to match the sampling frequency ```fs```. It also transforms the ```band_aperiodicity```, which is on dB to a power scale (i.e. $10^{\text{band_aperiodicity}/10}$).

To generate the **aperiodic component**, the script adds the velvet noise to the ```aperiodic_component``` variable, weighted by the ```band_aperiodicity``` corresponding to those samples. It is important to note that when the velvet noise was generated, it was made on different frequency bands, in this step of the script, the different frequency bands are simply added with their corresponding weight.

To generate the **periodic component**, for each pulse, the script adds the pulses weighted by their respective band aperiodicity on that sample. The weight is given by $1 - \text{band_aperiodicity}$. The calculated pulses are then concatenated.

The ```excitation_signal``` corresponds to the sum of the aperiodic and periodic component

\begin{equation}
    \text{excitation_signal} = \text{periodic_component} + \text{aperiodic_component}
\end{equation}


> I think that the weight $1 - \text{band_aperiodicity}$ is added so that the energy of the sum of the periodic component and aperiodic component stays constant regardless of the band aperiodicity. I tried synthesizing [with](https://files.benjagueno.cl/assets/world_audio/with_weight.wav "weighted pulses") and [without](https://files.benjagueno.cl/assets/world_audio/no_weight.wav "unweighted pulses") that weight and I think it sounds better with the unweighted pulses.

### Waveform Generation

The waveform is generated by convoluting the ```excitation_signal``` with the ```filter_object.spectrogram```, the smoothed spectrogram of the voice, frame by frame.

Before convoluting, the spectrogram is filtered on the cepstrum domain. 

>I can't understand what happens on the liftering, but audio quality improves a lot

Note that the spectrogram was previosuly calculated on frames with a fixed length, given by the parameters of the whole script, for example, 5 [ms].

The output signal is the synthesized voice!

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