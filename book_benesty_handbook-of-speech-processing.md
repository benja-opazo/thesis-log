# Book: **J Benesty, M. Mohan, Y. Huang - Handbook of Speech Processing**

## Notes

## Links

# Part A: Production, Perception and Modelling of Speech

## 2 Physiological Processes of Speech Production

>* **[Prosody](https://en.wikipedia.org/wiki/Prosody_(linguistics))**: is concerned with those elements of speech that are not individual phonetic segments (vowels and consonants) but are properties of syllables and larger units of speech, including linguistic functions such as intonation, tone, stress, and rhythm. Such elements are known as suprasegmentals. 

>* **Glottis**: The gap between the free edges of the vocal folds is
called the glottis.

In sharp voice, the open phase of the glottal cycle becomes shorter, while in soft voice, the open phase becomes longer

>* **Open Quotient** (OQ): Ratio of the open phase within a glottal cycle.

>* **Speed Quotient** (SQ): Ratio of the closing slope to the the glottal cycle.

The two former parameters determine the slope of the spectral envelope, and thus (I think?), the harmonic components.

|          | Open Quotient | Speed Quotient  |
| -------- | ------------- | --------------- |
| High     | -             | Pulsating Waves |
| Low      | Sinusoidal    | -               |

Obs: The values on the table reffer to the glottal airflow

Oscillation of vocal folds during natural speech is quasipreiodic, and these variations are observed in speech waveforms as two types of measures:

>* **Jitter**: Frequency perturbation (normally 1%)

>* **Shimmer**: Amplitude perturbation (normally 6%)

Another aspect of voice is fundamental frequency ($F_0$)

>* **Fundamental Frequency**: Lowest harmonic component in voiced sounds. It is the natural frequency of vocal fold vibration.

| Sex     | Lower bound [Hz] | Upper bound [Hz]  |
| ------- | ---------------- | ----------------- |
| Males   |  80              | 400               |
| Females | 120              | 800               |

>* **Vocal Tract**: Includes all the air spaces where acoustic pressure vairation takes place in speech production.

>* **Subglottal track**: Is the lower respiratory track below the glottis down to the lungs. Vocal source sounds propagate from the glottis to the trachea, causing >the subglottal resonances in speech spectra. The second subglottal resonances is often observed below the second formant of high vowels. The following table shows >the resonant frequencies of the subglottal airway

| Harmonic | Frequency [Hz] |
| -------- | -------------- |
| First    | 640            |
| Second   | 1400           |
| Third    | 2100           |

## 4 Perception of Speech and Sound

The absolute treshold in quiet conditions for a continuous sinusoid is highly dependent on the frequency of the pure tone.

* The highest sensitivity is at 1 KHz (This frequency carries most speech information)
* Perceived loudness doubles with an increase by 10 dB
* A broadband sound is perceived as being louder than a narrowband sound at the same energy level

>* **Sound Pressure Level (SPL)**: Normalized treshold in quiet at 1 KHz averaged over a large numberof normal hearing subjects is defined as 0 dB SPL

>* **Isophones**: All combination of sound pressure levels and frequencies of sinusoid that produce the same loudness as a reference 1 KHz sinsusoid of a given level (in dB SPL)

As discussed earlier, human perception of loudness depends on the bandwidth of the sound and the energy levels of the sound, as a consequence, in order to express speech sound pressure approximating human loudness perception, there are the following options available

* **RMS**: Using the formula for RMS, that is $$ RMS = \frac{1}{T} \int_{0}^{T} f(t) dt $$ The RMS value is usually expressed as dB SPL, i.e.: as $10\log(I/I_0)$ where $I$ is the signal intensity, and $I_0$ is the reference signal intensity at auditory treshold.
* **A-Weighted signal level**: This method takes into account the higher sensitivity of the ear to mid-frequencies at low levels. A spectral wighting function that approximates the isophones between 20 and 30 phon (A-wightinh) is applied to the spectral components of the sound before they are summed up.
    * There are also B- and C-weighting curves for higher signal levels
* **Loudness in sone**: For a narrow-band sound (sinusoid), the loudness in sone is expressed as $$N[sone] = (I/I_0)$$ where $I_0$ is the reference intensity (40 dB SPL for a sinusoid at 1 kHz). $\alpha \approx 0.3$

>* **JND**: Just Noticeable difference
>   * Frequency: 3 Hz below 500 Hz and about 0.6% frequencies over 1000 Hz

The within channel analysis has the following relevant phenomena:

*  **Below 1 KHz**: The temporal fine structure of the bandpass channel is coded in the auditory nerve. **The Signal phase can be exploited during central processing stages**
*  **Over 1 KHz**: Primarily the envelope of the signal is extracted and analyzed. **The ear is comparatively phase deaf above 1 KHz**.

 <img src="http://files.benjagueno.cl/images/book_benesty_handbook-of-speech-processing_chap4_signal-processing-auditory-system.png" alt="Nature" class="responsive" style="max-width:70%"> 
 


### 4.2 Acoustical Information Required for Speech Perception

>* **Speech Intelligibility** (SI): Is the proportion of speech items (e.g., syllables, words, or sentences) correctly repeated by (a) listener(s) for a given speech intelligibility test.

SI can be represented as a function of speech level L (measured in dB) of the speech signal or to the speech-to-noise ratio (SNR, measured in dB).

$$ SI(L) = \frac{1}{A} \left( 1 + SI_{max} \frac{A-1}{1+ \exp \left( - \frac{L-L_{mid}}{S} \right)} \right)$$

Where $L_{mid}$ is the speech level of the midpoint of the intelligibility function, $s$ is a slope parameter, $SI_{max}$ corresponds to the maximum intelligibility (can be smaller than 1), $A$ is the number of response alternatives (finite in a closed response format and $\infty$ in an open response format). Note that the slope at $L_{mid}$ is given by

$$ \frac{SI_{max}(A-1)}{4As} $$

>* **Speech Reception Treshold** (SRT): denotes the speech level (L), which belongs to a given intelligibility (e.g.: SI = 0.7).

Since the standard deviation of SRT estimates is inversely proportional to the slope of the intelligibility function, these sentence tests are better suited for efficient and reliable SRT measurements than single-word tests.

#### Internal Representation Approach

The **internal representation approach** of modeling speech reception assumes that the speech signal is transformed by our auditory system with some non-linear parallel processing operations into an internal representation.

>* **Ideal Observer**: An ideal observer performs a pattern match between the incoming internal representation and stored internal representations.

>* **External Variability**: It is the deviation from any of the stored internal templates due to neural noise physiological and psychological factors

The internal representation is used as an input for a central cognitive recognition (an ideal observer), whose accuracy depends on the external variability of the speech

## 5 Speech Quality Assessment

:::info
This Chapter talks about quality assessment in communication systems, nevertheless, it provides some objective measures that may be useful in the future
:::

> * **Auditory Event**: The result of a speech perception process

Frequency-domain measures are known to be highly correlated with human perception, but still relatively simple to implement. One of their critical advantages is that they are less sensitive to signal misalignment (not that relevant for this thesis). Some of the most popular frequency domain techniques are the Itakura–Saito (IS), the cepstral distance (CD), the log-likelihood (LL), and the log-area-ratio (LAR) measures. For this thesis, these methods may prove useful when assessing the quality of the resinthesized voice

## 7 Linear Prediction

A speech signal can be written in the following form

$$ x(k) = \sum_{l=1}^{L}a_l x(k-l) + G u(k)$$

Where k is the time index, L represents the order of the predictor, $a_l$ are the linear prediction coefficients, $G$ is the gain of the system and $u(k)$ is the excitation signal (e.g.: train impulses or random noise). Note that this function can be written in the frequency domain using the z-transform as an all-pole transfer function

$$H(Z) = \frac{G}{1-\sum_{l=1}^{L} a_lz{-l}} = \frac{G}{A(z)}$$

### Forward Linear Prediction

The objective of forward LPC is to predict the value of the sample $x(k)$ from it past values i.e.: $x(k-1), x(k-2), \dots$. The forward prediction error is defined as

\begin{align}
    e_{f,L} &= x(k) - \hat{x}(k) \\
            &= x(k) - \sum_{l=1}^{L}a_{L,l} x(k-l) \\
            &= x(k) - a_{L}^{T}\mathbf{x_L}(k-1)
\end{align}

Where $\hat{x}(k)$ is the predicted sample, the forward predictor $\mathbf{a}_L$ is a vector of length L, and is given by

$$\mathbf{a}_L =
\begin{matrix}
    [a_{L,1} & a_{L,2} & \cdots & a_{L,L}]^T
\end{matrix}
$$

and $\mathbf{x_L}(k-1)$ is a vector containing the $L$ most recent samples and is given by

$$\mathbf{x_L}(k-1) = 
\begin{matrix}
    [x(k-1) & x(k-2) & \cdots & x(k-L)]^T
\end{matrix}
$$

Note that the vector $\mathbf{x_L}(k-1)$ includes $x(k-1)$.

The objective is to **minimize the mean-square error** (MSE) that is given by

$$J_f(\mathbf{a}_L) = \mathbf{E} [ e^{2}_{f,L}(k) ]$$

That is, the expectation of the square of the error as defined previously. To minimize said error, we take the gradient of $J_f(\mathbf{a}_L)$ with respect to $\mathbf{a}_L$ and we equate to a vector of length L containing only zeroes, that is, $\mathbf{0}_{Lx1}$. We find the Wiener-Hopf equations

$$\mathbf{R}_L\mathbf{a}_{o,L} = \mathbf{r}_{f,L}$$

where $\mathbf{a}_{o,L}$ is the _optimal_ forward predictor. Where $\mathbf{R}_L$ is the correlation matrix and is given by

\begin{align}
    \mathbf{R}_L 
    &= \mathbf{E} \{ \mathbf{x}_L(k-1) \mathbf{x}^T_L(k-1) \} = \mathbf{E} \{ \mathbf{x}_L(k)\mathbf{x}^T_L(k) \} \\
    &= \left[ \begin{matrix}
            r(0) & r(1) & \cdots & r(L-1) \\
            r(1) & (r=) & \cdots & r(L-2) \\
            \vdots & \vdots & \ddots & \vdots \\
            r(L-1) & r(L-2) & \cdots & r(0)    
        \end{matrix} \right]
\end{align}

And $\mathbf{r}_{f,L}$ is the correlation vector and is given by

$$ \mathbf{r}_{f,L}
= \mathbf{E} [ \mathbf{x}_L(k-1) x(k)]
= \left[ \begin{matrix}
    r(1) & r(2) & \cdots & r(L)
  \end{matrix} \right]
$$

Assuming a nonsingular correlation matrix (so we can invert it), the forward predictor is given by

$$ \mathbf{a}_{o,L} = \mathbf{R}^{-1}_{L} \mathbf{r}_{f,L} $$

The minimum mean-square error (MMSE) is given by

\begin{align}
    \mathbf{J}_{f,min}
    &= \mathbf{J}_f(\mathbf{a}_{o,L}) \\
    &= r(0) - \mathbf{r}^{T}_{f,L} \mathbf{a}_{o,L} = E_{f,L}
\end{align}

### Backward Linear Prediction

The objective of Backward LPC is to predict past values of a signal, that is $x(k-L)$ given its future values ($x(k), x(k-1), \cdots, x(k-L+1)$).

The backward prediction error is defined as

\begin{align}
    e_{b,L}(k)
    &= x(k-L) - \hat{x}(k-L) \\
    &= x(k-L) - \sum_{l=1}^{L} b_{L,l} x(k-l+1) \\
    &= x(k-L) - \mathbf{b}^{T}_{L} \mathbf{x}_L(k)
\end{align}

Where $\hat{x}(k)$ is the predicted sample, the backward predictor $\mathbf{b}_L$ is a vector of length L, and is given by

$$\mathbf{a}_L =
\begin{matrix}
    [b_{L,1} & b_{L,2} & \cdots & b_{L,L}]^T
\end{matrix}
$$

and $\mathbf{x_L}(k-1)$ is a vector containing the $L$ most recent samples and is given by

$$\mathbf{x_L}(k-1) = 
\begin{matrix}
    [x(k-1) & x(k-2) & \cdots & x(k-L)]^T
\end{matrix}
$$

Following the same procedure as the forward linear predictor, we optain the optimal backward predictor coefficients as

$$\mathbf{b}_{o,L} = \mathbf{R}_{L}^{-1} \mathbf{r}_{b,L}$$

The MMSE of the backward prediction is given by

\begin{align}
    \mathbf{J}_{b,min}
    &= \mathbf{J_{b}(\mathbf{b}_{o,L})} \\
    &= r(0) - \mathbf{r}^{T}_{b,L}\mathbf{b}_{0,L}
    = E_{b,l}
\end{align}

## 9 Homomorphic Systems and Cepstrum Analysis of Speech

### Cepstrum

>We define **Cepstrum** as
>
> $$ c[n] = \frac{1}{2\pi} \int_{-pi}^{pi} \log |X(e^{j \omega})| e^{j\omega n} d\omega$$

Note that this is the **Inverse Discrete Fourier Transform** of the **logarithm** of the **magnitude** of the **Discrete Time Fourier Transform** of the signal.

Note that the power of the exponential in the integral is positive, that happens because we are calculating an **inverse** DTFT with the integral.

> We define **quefrency** as the independent variable of the cepstrum, that is, the _n_ in $c[n]$. Note that this variable could be associated as a time variable, after all, is the independent variable of an IDTFT.

### Homomorphic Systems

[Homomorphic systems](https://en.wikipedia.org/wiki/Homomorphic_filtering) are **non-linear** systems that can be mapped non-linearly to a different domain in order to attain some linear systems characteristics, most notably the **superposition**. In simpler terms, homomorphic systems are nonlinear systems that _transforms a combination by convolution into a corresponding combination by addition_.

> The operator $D_*\{\,\}$ is called the **characteristic system** for convolution and $D^{-1}_* \{\,\}$ is its inverse.

The characteristic system **transforms a combination by convolution into a corresponding combination by addition**

$$x[n] = x_1[n]*x_2[n] \iff \hat{x}[n] = \hat{x}_1[n] + \hat{x}_2[n]$$

Where

$$\hat{x}[n] = D_*\{ x[n] \}$$

Conversely, the inverse characteristic system must transform a sum into a convolution, by satisfying

$$H\{ x_1[n]*x_2[n] \} = H\{ x_1[n]\} * H\{ x_2[n] \} = y_1[n] * y_2[n]$$

Where $H$ corresponds to the following system

![Homomorphic system](https://files.benjagueno.cl/images/book_benesty_handbook-of-speech-processing_chap9_homomorphic_system.png "Homomorphic System")

Note that $L\{\,\}$ can be any linear system

The characteristic system can be represented by the following set of equations

\begin{align}
    X(e^{j\omega}) &= \sum_{n=-\infty}^{\infty} x[n] e^{-j \omega n} \\
    \hat{X}(e^{j\omega}) &= \log[X(e^{j\omega})] \\
    \hat{x}[n] &= \frac{1}{2\pi} \int_{-\pi}^{\pi} \hat{X}(e^{j\omega})e^{j\omega n}d\omega
\end{align}

Where the $\log$ operation is the complex logarithm defined as

$$\log[a + jb] = \log|a| + j \arg \{a + jb\} $$

And the $\arg\{ \, \}$ operator is the phase of the complex number.

The cascading of the previously defined operations has the desired property of turning convolution into addition ($D_*\{\,\}$), and conversely, by using complex exponential instead of complex $\log$, the inverse characteristic system is attained ($D^{-1}_*\{\,\}$).

![Characteristic](https://files.benjagueno.cl/images/book_benesty_handbook-of-speech-processing_chap9_characterisic_system.png "Characteristic System" =x390)

> Note that these definitions are closely related to the Cepstrum definition, and for that reason, the output of the characteristic system is called the **complex spectrum**.

If $x[n]$ is real, then $\hat{x}[n]$ is also real, hence, the complex spectrum is not necessarily complex, but it is named after the _complex logarithm_ that plays an important role in this definition.

The relationship between the Cepstrum and the Complex Cepstrum is given by

$$ c[n] = \text{Ev}\{\hat{x}[n]\} = \frac{\hat{x}[n] + \hat{x}[n]}{2}$$

Where $\text{Ev} \{ \, \}$ represents the even part of the function.

That means that **the Cepstrum is the even part of the Complex Cepstrum**

### Numerical Computation
:::info
I Should read section 9.1.3 when (if) needed
:::

### Discrete-Time Model for Speech Production


