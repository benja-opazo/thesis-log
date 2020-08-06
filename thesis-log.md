# Thesis Log

## Notes

## Links

Audio samples from "Transfer Learning from Speaker Verification to Multi speaker Text-To-Speech Synthesis" ([Link](https://google.github.io/tacotron/publications/speaker_adaptation/))

The Visi-Pitch: A Clinical Tool ([pdf](http://w140.com/tekwiki/images/0/05/Kay_visi-pitch_6087_ad.pdf))

Analysis of variance (ANOVA) ([Wiki](https://en.wikipedia.org/wiki/Analysis_of_variance))

SPTK is a publicly available toolkit that provides linear predictive and MLSA analysis/synthesis ([Link](http://sp-tk.sourceforge.net/))

AHOCODER is a publicly available toolkit that provides high-quality HNM synthesis ([Link](https://aholab.ehu.eus/ahocoder/info.html))

COVAREP A Cooperative Voice Analysis Repository for Speech Technologies ([Github](https://github.com/covarep/covarep))

Mean Opinion Score  (MOS) are a standard measure for subjective sound quality tests⁽[ʳᵉᶠ](https://deepmind.com/blog/article/wavenet-generative-model-raw-audio)⁾
 ([Wiki](https://en.wikipedia.org/wiki/Mean_opinion_score))
 
 ==WORLD Vocoder ([Github](https://github.com/mmorise/World), [Link](http://www.kki.yamanashi.ac.jp/~mmorise/world/english/download.html))==
 
 Mozilla Common Voice Dataset ([Link](https://discourse.mozilla.org/t/common-voice-dataset-release-mid-year-2020/62938))
 
 Audio Latency on Linux ([Link](https://www.mindmusiclabs.com/audio-latency-demystified-part-2-4-real-time-linux-approaches/))



## Week 1 (09/03/2020)

1. Thesis proposal
2. J. Elman - **Effects of frequency-shifted feedback on the pitch of vocal production** ([Link](https://files.benjagueno.cl/papers/Elman1981.pdf))
    * They hypothesize that there is some tendency for subjects to compensate for the frequency shifts. That is, if they listen to a higher pitch, the subjects will lower their voices
    * Frequency shifts as great as 15% were not noticeable to many people
    * The distortion (frequency-shift feedback) is sufficient to affect F0 (compensation), but not so great as to cause them to reject the feedback as valid
    * Awareness of the distortion seemed not to be necessary for nor to prevent compensation (note that the experiments weren't under natural speaking situation)

3. J. Kreiman - **Perceptual Evaluation of Voice Quality** ([Link](https://files.benjagueno.cl/papers/Kreiman1993.pdf))
    * As stated in the title, this review discuses perceptual evaluation of voice quality
    * Voice quality is fundamentally perceptual in nature
    * They discus about types of rating scales:
      * Categorical rating: Discrete unordered categories
      * Equal-appearing interval (EAI): scale between 1 and n to a voice sample
      * Visual Analog (VA): undifferentiated lines
      * Direct Magnitude Estimation (DME): Listeners assign a number to a voice sample to indicate the extent to which it possesses the quality being rated
      * Paired Comparison: Listeners compare two stimuli
    * Listeners frequently agree about what constitutes normal phonation or sever pathology but disagree on more on the mild section of the scale
    * Listeners varied widely in their levels of reliability and agreement
    * EAI ratings (but not VA ratings) drifted significantly during a single listening session
    * Existing protocols (1993) do not consistently result in reliable ratings
  
## Week 2 (16/03/2020)


1. J. Kreiman - **Defining and Measuring Voice Quality** ([Link](https://files.benjagueno.cl/papers/Kreiman2003.pdf))
    * In this paper, the researchers try to define what is voice quality (using several definitions, pros and con of each definition), and different methods to measure it, taking into account the aforementioned definitions.
    * The overall quality (or timbre) of a sound is traditionally defined as "that attribute of auditory sensation in terms of which a listener can judge that two sounds similarly presented and having the same loudness and pitch are dissimilar"
    * Quality is multidimensional, that includes the spectral envelope and its changes in time, fluctuations of amplitude and fundamental frequency, and periodicity of the signal.
    * Voice Quality is a perceptual phenomenon -> listener's contribution to quality is fundamental
    * Voice quality may best be thought of as an interaction between listeners and a signal
    * There is considerable confusion surrounding the problem of measuring voice quality
    * The GRBAS protocol was developed from the result of factor analyses of restricted populations of speakers, small sets of voices and **short stimuli**
    * **A proposed solution to a more reliable voice quality assessment is to use a speech synthesized voice to calibrate the listeners internal scale** (*Gerratt, B.R., and Kreiman, J., “Measuring Vocal Quality With Speech Synthesis”, J.Acoust.Soc.Am., 110:2560-2566, 2001.*)
2. R. Taitelbaum-Swead - **The effect of DAF and FAF (frequency) on speech production: cochlear vs normal hearing** ([Link](https://files.benjagueno.cl/papers/Taitelbaum_Swead2019.pdf)) 
    * The objective is to evaluate the effect of DAF and FAF on speech production among prelingual cochlear implant users and normal hearing individuals
    * There is compelling evidence that absence of auditory feedback during language acquisition affects the development of speech production abilities
    * Adults who have already acquired speech, the absence of auditory feedback is associated with deterioration in speech production over time
    * Auditory feedback affects speech not only when it is absent but also when it is altered
    * During FAF, talkers show a compensatory patterns, however some talkers change their pitch in the same direction as the feedback
    * According to this study, temporal alterations of speech production as a result of DAF was more pronounced than the effect of spectral alterations (FAF).
    * Other literature reported that FAF has a noteworthy effect on speech production, particularly in fundamental frequency
3. Z. Wang - **Automatic Assessment of Pathological Voice Quality using Multidimensional Acoustic Analysis Based on the GRBAS Scale** ([Link](https://files.benjagueno.cl/papers/Wang2015.pdf)) 
    * Perceptual evaluation of VQ is considered a gold standard for assessing pathological voice quality, inter and intra-listeners variability associated with different perceptual ratings cannot be ignored
    * **Some of the more common assessment protocols include the Buffalo Voice Profile (BVP) Analysis, Vocal Profile Analysis (VPA) scheme and GRBAS scale**
    * Jitter, shimmer and harmonics-to-noise-ratio (HNR) have been used to evaluate VQ
    * Glottis Quotient (GQ) is also used to indicate the stability of vocal folds during phonation
    * MFCC can be adopted for voice quality assessment (with machine learning)
    * Glottal Closure Instants (GCIs) and Glottal Open Instants (GQIs) can be automatically estimated by using DYPSA algorithm.
    * The paper focuses on the automatic assessment of VQ by estimating or calculating different features and then reducing the feature set by using different algorithms. The assessment of dysphonic voices is difficult and unreliable, because pathological voices are heterogeneous
4. J. Kreiman - **Toward a unified theory of voice production and perception**  ([Link](https://files.benjagueno.cl/papers/Kreiman2014.pdf))
    * Acoustic perturbation measures are not individually informative about voice quality, because listeners cannot hear even large differences in jitter or shimmer
    * **In order to quantify the entire voice pattern, they apply analysis-by-synthesis to copy each voice sample with a speech synthesizer (Kreiman,Antoñanzas-Barroso,&Gerratt,2010).**
    * This is important for copy-synthesize a voice:
      * They assumed that the listeners are more likely to pay attention to acoustic parameters that vary across voices
      * Four factors accounted for most of the variance in source spectral shape:
        * Source spectral slope above 4ì KHz
        * The slope below 450 Hz
        * The slope from 1.5 KHz to 4 KHz (two factors(?))
      * Significant variabilty across voices in the relative amplitudes of:
        * The first and second harmonics (H1-H2)
        * The second and fourth harmonics (H2-H4)
      * overall spectral slope
      * high frequency noise excitation
      * After running a same/different test on subjects in order to fine tune the synthesizer they found that they had to add two new parameters
        * The spectral slope from the fourth harmonic to the harmonic nearest 2 kHz in frequency (H4-2 kHz)
        * Spectral slope from that harmonic to the harmonic nearest 5 kHz (2 kHz - 5 kHz)
    * They used the UCLA voice synthesizer to copy-synthesize the voices
    * The voice source was estimated via inverse filtering, and its spectrum was then calculated via FFT
    * **The percept of breathiness is influenced by a steep drop in harmonic energy in the lower frequencies**

:::info
Notes: Que tipo de sintetizador ocupan??
:::

5. I. Itze - [Principles of Voice Production](./_zGm-Kl4SyWXceT5vQ88CA)
    * Introduction
    * Chapter 2

6. J Benesty, M. Mohan, Y. Huang - [Handbook of Speech Processing](./64WRtla6ROOu8VlVHRvDXQ)
    * Introduction
    * Chapter 2

## Week 3 (20/03/2020)
1. J Benesty, M. Mohan, Y. Huang - [Handbook of Speech Processing](./64WRtla6ROOu8VlVHRvDXQ)
    * Chapter 4

## Week 4 (06/04/2020)
1. J Benesty, M. Mohan, Y. Huang - [Handbook of Speech Processing](./64WRtla6ROOu8VlVHRvDXQ)
    * Chapter 4 (cont.)
    * Chapter 5
    * Chapter 7

2. L. Atlas, S. Shamma - Joint Acoustic and Modulation Frequency ([Link](https://files.benjagueno.cl/papers/Atlas2003.pdf)) 
    * This papers aims to improve perceptual understanding and modeling
    * It proposes a new representation and coding of acoustic signals
    * Cells in the auditory cortex are best driven by sounds that combine both spectral and temporal modulations
    * The following image shows the power of Modulation Frequency visualization, by recognizing two distinct speakers.

    ![Modulation Frequency visualization](https://files.benjagueno.cl/images/paper_atlas2003_fig2.png)
    
    * The objective is to represent a frequency modulated signal as a two-dimensional distribution $P(\eta, \omega)$, where $\eta$ is the modulation frequency and $\omega$ is the acoustic frequency
    * An analysis/synthesis approach ideally requires invertibility and perfect reconstruction
    * A Modulated Signal can be expressed as $$s_1(t) = m(t) \cos(\omega_c t)$$ Where $m(t)$ is the modulating signal and it is nonnegative and bandlimited. The Fourier transform of $s_1(t)$ may be expressed as $$S_1(e^{j\omega}) = \textbf{F} \{ (t) \} = \int s_1(t) e^{-j\omega t}dt$$
    * Using a bilinear formulation such as seen in [time-frequency analysis](https://en.wikipedia.org/wiki/Bilinear_time%E2%80%93frequency_distribution), $S_1(e^{j \omega})$ can be expressed in terms of $\omega$ and $\eta$, as follows $$S \left( \omega - \frac{\eta}{2} \right) S^* \left( \omega + \frac{\eta}{2} \right)$$ Where $\omega$ corresponds to the acoustic frequency of the signal and $\eta$ can express symmetric translation of that frequency (as a consequence of modulation).
    * A sufficient condition for the ideal modulation frequency versus acoustic frequency distribution is $$ P_{ideal}(\eta, \omega) = \left\{S\left( \omega - \frac{\eta}{2} \right) S^* \left( \omega + \frac{\eta}{2} \right) \phi_m(\eta) \right\} * \phi_c(\omega) $$ Where $\phi_m(\eta)$ is a multiplicative kernel in $\eta$ and $\phi_c(\omega)$ is a convolutional kernel in $\omega$

## Week 5 (13/04/2020)

1. J Benesty, M. Mohan, Y. Huang - [Handbook of Speech Processing](./64WRtla6ROOu8VlVHRvDXQ)
    * Chapter 7 (cont.)
    * Chapter 9

## Week 6 (20/04/2020)
1. J Benesty, M. Mohan, Y. Huang - [Handbook of Speech Processing](./64WRtla6ROOu8VlVHRvDXQ)
    * Chapter 9 (cont.)
2. Matlab -  LPC and Cepstrum [matlab_lpc_cepstrum.m](https://files.benjagueno.cl/matlab/lpc_cepstrum_test.m)
3. Meeting 23/04
    * CELP: Code Excited Linear Prediction
    * Text: [El Abuelo](https://fonoaudiologiaexpress.files.wordpress.com/2012/05/prot-habla.pdf) (phonetically balanced)
    * **Todo**:
        - [ ] Resynthesize "El Abuelo" with LPC and Cepstrum (windows of 10-20 ms).
        - [ ] Read contemporary papers about speech synthesis. They have to be doable in real-time

## Week 7 (27/04/2020)
 1. Matlab -  LPC and Cepstrum [matlab_lpc_cepstrum.m](https://files.benjagueno.cl/matlab/lpc_cepstrum_test.m)
 2. Github: [LPCsynthesis](https://github.com/krylenko/LPCsynthesis)
     * It is not that good
 3. Y. Bennane - **Synthesis of Pathological Voices and Experiments on the Effect of Jitter and Shimmer in Voice Quality Perception**  ([Link](https://files.benjagueno.cl/papers/Bennane2017.pdf))
     * Voice synthesizer based on Direct Digital Synthesis (DDS). Useful to generate only jitter, shimmer or both.
     * The treshold of normal/pathological voice is 1.04% for jitter, and 3.81% for shimmer.
     * Excess additive noise at the glottis may cause voices to be perceives as breathy.
     * Hoarseness is not well understood int he xontext of properties of the modulation
     * The DDS is a method usually used to generate periodic signals in variant fields
 5. Meeting 30/04
     * LPC Troubleshoot
         - [ ] Check Overlap
         - [ ] Check inverse filtering
         - [ ] Check Prof. Zañartu's Code
    * **Todo**:
        - [ ] Continue Resynthesizing
        - [ ] Continue Reading contemporary papers about speech synthesis

## Week 8 (04/05/2020)
 1. Conference: [IEEE ICASSP 2020](https://cmsworkshops.com/ICASSP2020/TechnicalProgram.asp)
 2. S. Mohammadi - **An Overview of Voice Conversion Systems** ([link](https://files.benjagueno.cl/papers/Mohammadi2017.pdf))
     * Linear predictive coding (LPC) is an implementation of all-pole models, and mel-log spectrum approximation (MLSA)
     * [SPTK](http://sp-tk.sourceforge.net/) is a publicly available toolkit that provides linear predictive and MLSA analysis/synthesis
     * Signal-based analysis/synthesis approaches model the speech signal by not making any restrictive assumptions (such as the independence of source signal and filter); hence they usually have higher quality. The downside is that they are less flexible for modification.
     * [AHOCODER](https://aholab.ehu.eus/ahocoder/info.html) is a publicly available toolkit that provides high-quality HNM synthesis
     * **3.2 Contextual Features**: Most of the mapping functions assume frame-by-frame processing. Human speech is highly dynamic over longer segments and the frame-by-frame assumption restricts the modeling power of the mapping function
     * **7.1 Objective Evaluation**: The most prominent measure used in the VC literature is the mel-cepstrum distance (mel-CD), also measured in dB. **The mel-CD is suitable for evaluating preliminary experiments, defining training criterions, and validation purposes, but not for evaluating the final system regarding quality due to the low correlation with human perception**
    \begin{equation}
         \text{mel-CD}(y, \hat{y}) = \left( \frac{10}{\ln 10} 
    \right) \sqrt{2(y-\hat{y})^{T}(y-\hat{y})}
    \end{equation}
    * An automatic voice conversion evaluation strategy was proposed, wherein both speech quality and speaker similarity were automatically computed ([Huang et al., 2016](https://files.benjagueno.cl/papers/Huang2016.pdf))
 2. D. Huang - **An Automatic Voice Conversion Evaluation Strategy Based on Perceptual Background Noise Distortion and Speaker Similarity** ([link](https://files.benjagueno.cl/papers/Huang2016.pdf))
     * Objectvie VQ Measurements: They were acquired by linear combination of basic objective measures such as segmental SNR (segSNR) [31], weighted-slope spectral distance [32], and PESQ measure [12].
 3. Meeting 07/05
    * Voice Quality Measurements:
        * CPP: Cepstral peak prominence
        * HNR: Harmonic Noise Ratio
    * Glottal Source Voice Quality (objective?)

## Week 9 (11/05/2020

1. D. Childers - **Glottal source modeling for voice conversion** ([link](https://files.benjagueno.cl/papers/Childers1995.pdf))
    * They used two methods to synthesize voice: Using a polynomial model (an LP synthesizer) and using a LF model (LF??).
    * The LP parameters are not easily related to the glottal excitation waveform and the vocal tract parameters. and manipulating the parameters by rule is a difficult and largely an unsolved problem.
    * For example, recent research has shown that characteristics of the glottal source waveform, such as the glottal pulse width, glottal pulse skewness, the abruptness of glottal closure, and a turbulence noise component (Childers and Lee, 1991), are important both for speech synthesis and for modeling voice types and vocal disorders.
    * A simple glottal waveform model can characterize attributes of the glottal volume-velocity waveform for three voice types, namely, modal (a vocal register), vocal fry (a vocal register) and breathy voice.
    * The size of the CELP codebook is typically determined by three factors: the transmission rate, computational complexity and the frame update rate.
    * It appears that the mean value of the normalized parameter $t_c$ which is equivalent to the open quotient ($OQ_{LF}$) **is a potential feature for distinguishing the three voice types** (modal voice, vocal fry and breathy voice)
    * In general, the incorporation of turbulence noise in the excitation enhanced the naturalness of the synthesized speech.
2. T. Drugman - **Glottal Source Processing: from Analysis to Applications** ([link](https://files.benjagueno.cl/papers/Drugman2014.pdf))
    * ==Glottal source processing is necessary for the study of glottal-based vocal effects, which can be segmental (as for vocal fry), or be controlled by speakers on a separate supra-segmental layer (as in the case for the voice quality modifications)==
    * The voice source or glottal source is the volume velocity waveform that serves as the excitation of speech produced by the vocal folds
    >* **Open Phase**: Time span during which the vocal folds (VF) are opening
    >* **Return Phase**: Period during which the VF are closing
    >* **Closed Phase**: The period during which the VF remain closed

![Waveforms](https://files.benjagueno.cl/images/paper_drugman2014_fig3.png "Typical Waveform" =x400)
 
    Typical Waveform of the glottal flow (top) and the glottal flow derivative (bottom)
     


## Week 10 (25/05/2020)

#### ICASSP

1. **GCI Detection from Raw Speech using a Fully-Convolutional Network** - Axel Roebel
    * Covers Full pitch range
        * Doesn't actually work with rough voices
    * They compared their results with other methods, **most notably the SEDREAMS (Drugman 2012)** which is present in **COVAREP**
        * In general, SEDREAM outperforms DPI algorithm (at least in detection rate)
2. **Deep Representation Learning** - Yoshua Bengio
3. **F0-Consistent Many-to-Many Non-Parallel Voice Conversion Via Conditional Autoencoder** - Kaizhi Qian

4. Meeting 28/05
    * Finish Voice Quality on Drugman's Review, voice pitch
    * Study Covarep
    * Look for references in the toolboxes

## Week 11 (01/06/2020)

1. T. Drugman - **Glottal Source Processing: from Analysis to Applications** ([link](https://files.benjagueno.cl/papers/Drugman2014.pdf))
    * Besides the application of statistical parametric synthesis, several systems have targeted voice transformation by processing the excitation signal. Cabral et al. (2008), Degottex et al. (2011b) and Agiomyrgiannakis and Rosec (2009) proposed the use of the LF model to perform voice modifications (e.g. in terms of breathiness or tenseness of the generated speech). Several approaches have also focused on the manipulation of the excitation signal to carry out high-quality pitch modification (Cabral and Oliveira, 2005; Degottex et al., 2011b; Drugman and Dutoit, 2010a). Finally, the glottal source has also been employed in the context of voice conversion (i.e. with a specific target speaker in view) where, in addition to improving the segmental quality, it also offers the possibility to apply voice quality modifications (Childers, 1995; Pozo and Young, 2008).
    * The weakest point of current glottal source processing algorithms is related to their lack of robustness.
2. L. Rachman - **DAVID: An open-source platform for real-time transformation of infra-segmental emotional cues in running speech** ([link](https://files.benjagueno.cl/papers/Rachman2017.pdf))
    * More recently, speech synthesis techniques, typically pitch-synchronous overlap-and-add methods (PSOLA) **and shape-invariant phase vocoder (Roebel, 2010)**, support the active testing of hypotheses by directly manipulating the acoustic parameters of the vocal stimuli (Bulut & Narayanan, 2008).
    * A second particularity of this work is that the transformation can be done in real time, modifying speech as it is uttered, without imparting any delay capable of breaking a natural conversation flow (in practice, less than 20ms)
    * The approach described here manages to operate in real-time by careful design rather than by technical prowess. First, we favor effects that can be implemented efficiently, such as simple time-domain filtering, and in cascade (such as vibrato and pitch shifting both using the same pitch shifting module). Second, because the manipulation is designed to be ‘‘natural”, our effects operate over very subtle parameter ranges (e.g., +/− 40 cents pitch shifting, instead of e.g., +/− 1 octave as targeted in Cabral and Oliveira 2005), for which even simplistic (and fast) approaches are sufficient.
    * ==Aucouturier et al. (2016) found that vocal feedback with a latency of 20 ms did not disrupt continuous speech==
    * Because the manipulations affect voice spectrum, headphones should present a relatively flat frequency response. In this study, we used Beyerdynamic’s DT770 Pro headphones, which we found satisfy these requirements.
    * [Video](https://player.vimeo.com/video/146702945)

## Week 12 (08/06/2020)
    
##### ==A. Roebel - **Shape-invariant speech transformation with the phase vocoder**== ([Link](https://files.benjagueno.cl/papers/Roebel2010.pdf))

The paper proposes a new phase vocoder based method for **shape invariant real-time modification of speech signals**. The algorithm, compared to PSOLA works with a very satisfying performance.

A standard phase vocoder performs signal transformation modifying the spectral frames of a STFT given by

\begin{equation}
    X_l(k) = \sum_{n} x(n) w(n-C_l) e^{\frac{-2 \pi k n}{N}}
\end{equation}

Where:
* $x(n)$ is the input signal
* $w(n)$ is the analysis window that is centered around the origin
* $M$ is the windows length
* $N > M$ is the DFT size
* $C_l$ is the window center for frame $l$

The idea is to modify the content and position of the frames $X_l(k)$, obtaining the **modified sequence $(\widetilde{X_l})$** which is then synthesized using overlap-add.

**$C_{l}^{'}$ are the synthesis frame positions**. If the STFT frames are time-shifted, the phases of the STFT have to be adapted to achieve coherent overlap-add **of the sinusoidal components**.

Phases at position $C'(l)$ are obtained from phases at position $C'(l-1)$ as follows

\begin{align}
    I &= C_l - C_{l-1} \\
    \Theta_l(k) &= \frac{\left[ \arg(X_l(k)) - \arg(X_{l-1}(k)) - I \frac{2\pi k}{N} \right]_{2 \pi }}{I} \\
    \widetilde{\Phi_{l}(k)} &= \widetilde{\Phi_{l-1}(k)} + (\Theta_l(k) + \frac{2\pi k}{N})(C_{l}^{'} - C_{l-1}^{'})
\end{align}

Where:
* $\Theta_l$ is the frequency difference between the center frequency at bin $k$ that is obtained using **the principal value $[]_{2\pi}$** of the observed and nominal expected phase in frame $l$
* $\Phi_l(k)$ is the phase in $X_l(k)$

Note that the phase update in the phase vocoder does not take into account the phase relations between **the different sinusoids** (vertical phase synchronization).

The vertical phase synchronization of the sinusoidal components is perceptually uncritical for most musical signals, whereas for **speech signals it affects the perception of the underlying excitation pulses**, and leads to an artifact that is generally describes as missing clarity of the transformed voice.

**A Shape Invariant Processing (SHIP) is a transformation algorirthm that preserves these inter-partial phase relations**.

For the cases of harmonic sounds (where the frequencies are whole multiples of the resonant frecuency), the phase of the modified frame can be obtained as

\begin{equation}
    \widetilde{\Phi_l(k)} = \Phi_l(k) + \Theta_l(k) \Delta_n
\end{equation}

where:
* $\Delta_n$ is a time shift that will be determined below.

The inter-partial phase synchronization is always maintained because:
* $\Delta_n$ is generally small
* The recursive structure of eq (4) is avoided.

**Phase estimation of the optimal time shift**

The optimal time shift can be obtained from the cross-correlation between the last synthesis frame $\widetilde{X_{l-1}(k)}$ and the current unmodified synthesis frame $X_l(k)$ that has been placed in position $C_{l}^{'}$

They constrain the cross-correlation to use only sinusoidal signal components by using a spectral mask $S_l(k)$ This way, the impact of the signal background noise during the estimation of the optimal overlap position is attenuated.

It is needed that $N \geq 2M$, if it is not fulfilled, the complex signal spectrum can be interpolated prior to masking. And the **interpolation can be limited to the frequency range containing sinusoidal components**.

The cross-correlation sequence for the sinusoidal components denotes as $Z(n)$ is given by

\begin{equation}
    Z(n) = \sum_{k=0}^{N}((X_l(k)^* S_l(k) \widetilde{X_{l-1}}(k)S_{l-1}(k))e^{j\frac{kn}{N} 2\pi}
\end{equation}

Where:
* $S_l(k)$ is the spectral mask for each frame $l$

The objective is to find the maximum of the underlying periodic structure of the cross-correlation sequence by removing as much as possible the effect of the analysis window.

For a given frame offset, between the synthesis frames, the preferred time delay is 

\begin{equation}
    O_l = C_{l}^{'} - C_{l-1}^{'}
\end{equation}

In this case we do not have to modify the phases of the synthesis.

For modified signals, the idea is that the delay is as close as possible to $O_l$, how close?, $O_l \pm P/2$

Where:
* $P$ is the length of the signal period at the center of the current synthesis frame $X_l$

The optimal time shift following the constraints discussed above can be calculated with the following equations

\begin{align}
    N(n) &= \max(Z_q(n),Z_w(D)) \\
    Z'(n) &= Z(n)/N(n) \\
    T_l &= \underset{n}{\arg \max} (Z'(n) N(n-O_l))
\end{align}

Where:
* $Z_w(n)$ is the auto-correlation sequence of the analysis window
* $N(n)$ is a normalization sequence that compensates the effect of $Z_w(n) on the cross-correlation sequence
* $Z'(n)$ represents the cross-correlation sequence after compensation of the systematic impact of the analysis window by means of $N(n)$.
* $T_l$ is the optimal time delay, which is given when $Z'(n)N(n-O_l)$ is maximum

Then, we get

\begin{equation}
    \Delta_n = T_l - O_l
\end{equation}

---
 1. Presentation:
     * Is JND different between voice perception and voice generation?
     * **When doing the thesis, it is important to note that latency must be constant (no jitter)**

## Week 13 (15/06/2020)

1. G. Degottex - **Pitch Transposition and Breathiness Modification using a Glottal Source Model and its Adapted Vocal-Track Filter** ([Link](https://files.benjagueno.cl/papers/Degottex2011.pdf))
    * **Not in real-time**
    * This paper addresses the pitch transposition and the modification of breathiness by means of an analytic description of the deterministic component of the voice source, a glottal model.
    * The model to synthesize they use is the following
\begin{equation}
    S(\omega) = \left[ H^{f_0}(\omega) \cdot G^{Rd}(\omega) + N^{\sigma_g}(\omega)\right] \cdot C^{\bar{c}}(\omega) \cdot L(\omega)
\end{equation}
    * The voice signal can be parametrized by $\{f_0,Rd,E_e,\sigma_g, \bar{c} \}$
    * For small transpositions, the quality of the SVLN method seems between those of SHIP and STRAIGHT
    * However, for a significant transposition (one octave below or above), the glottal model used in SVLN constraints the deterministic component of the glottal source in some way that a minimal naturalness is ensured

2. **WaveNet** ([Blog post](https://deepmind.com/blog/article/wavenet-generative-model-raw-audio), [Paper 2016](https://arxiv.org/pdf/1609.03499.pdf), [Paper 2017](https://arxiv.org/pdf/1711.10433.pdf)) 
    * It uses **machine learning** to synthesize voice
    * The best algorithm to synthesize voice (2017)
    * It can even synthesize music!
    * **Extremely slow**: 1 second of synthesis takes 1 minute of a powerful GPU
    * Very hard to train
    * **Parallel Wavenet** is very fast to synthesize (1 second of voice takes less than a second), but almost impossible to train if you are not Google or Apple

3. M. Gentilucci - **Composing Vocal Distortion: A Tool for ==Real-Time Generation of Roughness==** ([Link](https://files.benjagueno.cl/papers/Gentilucci2019.pdf))
    * In the **spectral domain**, a rough voice is characterized by a low harmonic-to-noise ratio, with the presence of noise and subharmonics 
    * In the **time domain**, rough voices are mainly characterized by the presence of important degrees of jitter and shimmer
    * From the **perceptual point of view**, roughness is related to the ability of the auditory system to differentiate close individual sinusoids presented together
    * Vocal roughness results from nonlinear phenomena in the vocal production system and, depending on the effect, may have various physiological causes that can hardly be determined from the signal itself.
    * A first possible cause of roughness can be laryngeal mucous lesions (nodules, polyps) or laryngeal mobility lesions, such as paralysis (Mu ˜noz et al. 2003)
    * ==Jordi Bonada (2008): Basically, this is a pitch-synchronous analysis/resynthesis framework allowing the manipulation of sinusoidal components of a voice’s spectrum before resynthesizing it in the time domain==
    * The approach presented here is mostly suitable for generating sounds with stable subharmonics
    * The most computationally intensive step in Angus is estimating f0, which is necessary to set appropriate frequencies for the modulating signals. This is done with an implementation of the YIN algorithm (originally developed by de Cheveign´e and Kawahara 2002) that runs in real time (see http://forumnet.ircam.fr/product/max-sound -box-en), guaranteeing that the effect is free of audible latency

![Roughness](https://files.benjagueno.cl/images/paper_gentilucci2019_fig6.png "Real-Time Roughness Method" =x400)

3. M. Morise - **WORLD: A Vocoder-Based High Quality Speech Synthesis System for Real-Time Applications** ([Link](https://files.benjagueno.cl/papers/Morise2016.pdf))


    * The speech synthesized by most of the conventional vocoder systems is inferior to that of waveform-based systems [4]. **An exception is a vocoder based system called STRAIGHT** [5], which is capable of high-quality speech synthesis. STRAIGHT also **makes it easy to manipulate speech**, and its technology has been used in various studies.
    * Real-time STRAIGHT [10] has been proposed as a way to meet the demand for real-time processing, **but the simplified algorithm it uses degrades the quality of the synthesized speech**
    * TANDEM-STRAIGHT [12], [13] is supposed to be a simplified version that outputs almost all the same parameters as STRAIGHT. Although the system works well, it is hard to use it for real-time speech analysis and synthesis.

![WORLD Overview](https://files.benjagueno.cl/images/paper_morise2016_fig1.png "World Overview" =x300)

## Week 14 (22/06/2020)

1. G. Degottex - **Mixed source model and its adapted vocal tract filter estimate for voice transformation and synthesis** ([Link](https://files.benjagueno.cl/papers/Degottex2013.pdf))


2. Meeting 25/06
    * WORLD citations
    * WORLD citations in voice conversion context
    * Check on WORLD black boxes

## Week 15 (06/07/2020)

1. M. Morise - **Fast and Reliable F0 Estimation Method Based on the Period Extraction of Vocal Fold Vibration of Singing Voice and Speech** ([Link](https://files.benjagueno.cl/papers/Morise2009.pdf))
    * A fast and reliable fundamental frequency (F0) extraction method is proposed for real-time interactive applications using a singing voice
    * In this article, a Nuttall window [8] is used as a low-pass filter with a sidelobe of about -90 dB for evaluations
    * Fundamentalness is defined as the variance of negative and positive going zero-crossing intervals and the intervals between successive peaks and dips
    * The best candidate is selected from the calculated fundamentalness of all the filtered signals. The lowest fundamentalness at each observation point is selected in this step
    * There are several important parameters of the proposed method for accurate F0 extraction. Mainly, the type of low-pass filters and the number of candidate filters relate to the accuracy ofthe proposed method

![DIO](https://files.benjagueno.cl/images/paper_morise2009_fig1.png "DIO method outline" =x300)

2. H. Kawahara - **Using instantaneous frequency and aperiodicity detection to estimate F0 for high-quality speech synthesis** ([Link](https://files.benjagueno.cl/papers/Kawahara2016.pdf))
    * Not in real time

## Week 15 (21/07/2020)

1. M. Morise - **Harvest: A high-performance fundamental frequency estimator from speech signals** ([Link](https://files.benjagueno.cl/papers/Morise2017.pdf))
    * Not in real time
    * Useful for Statistical Parametric Speech Synthesis (SPSS)

2. M. Morise - **CheapTrick, a spectral envelope estimator for high-quality speech synthesis** ([Link](https://files.benjagueno.cl/papers/Morise2015.pdf))

3. M. Morise - **Error Evaluation of an F0-Adaptive Spectral Envelope Estimator in Robustness against the Additive Noise and F0 Error** ([Link](https://files.benjagueno.cl/papers/Morise2015-2.pdf))

## Week 16 (27/07/2020)

> The WORLD code calculates a FFT once every N samples, [this](https://stackoverflow.com/questions/6663222/doing-fft-in-realtime) answer from stackOverflow implements a rolling FFT, although it uses a rectangular window instead of a a Hanning (Hann) window.

> This [document](https://www.comm.utoronto.ca/~dimitris/ece431/slidingdft.pdf) is a tutorial of the SDFT (Sliding DFT)
>
 
1. K. Kim - **It’s About Time: Minimizing Hardware and Software Latencies in Speech Research With Real-Time Auditory Feedback** ([Link](https://files.benjagueno.cl/papers/Kim2019.pdf))
    * It has been shown, for example, that auditory feedback delays disrupt aspects of ongoing speech production when the delay reaches 50 ms or more (Stuart et al., 2002).
    * Additionally, studies on speech auditory–motor learning across trials have demonstrated that adaptation to frequencyshifted auditory feedback is reduced or eliminated when the feedback signal is delayed by 100 ms (Max & Maffett, 2015; Mitsuya et al., 2017; work currently in progress in our lab is testing the effect on adaptation of delays shorter than 100 ms).
    * It has been reported that a delay greater than 25 ms can already slow down the speech rate and a delay greater than 50 ms may induce disfluencies (Stuart et al., 2002)