# Thesis Log

## Notes

## Links

Audio samples from "Transfer Learning from Speaker Verification to Multi speaker Text-To-Speech Synthesis" ([Link](https://google.github.io/tacotron/publications/speaker_adaptation/))

The Visi-Pitch: A Clinical Tool ([Link](http://w140.com/tekwiki/images/0/05/Kay_visi-pitch_6087_ad.pdf))

Analysis of variance (ANOVA) ([Link](https://en.wikipedia.org/wiki/Analysis_of_variance))

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
 2. S. Mohammadi - **An Overview of Voice Conversion Systems** [link](https://files.benjagueno.cl/papers/Mohammadi2017.pdf)
     * Linear predictive coding (LPC) is an implementation of all-pole models, and mel-log spectrum approximation (MLSA)
     * [SPTK](http://sp-tk.sourceforge.net/) is a publicly available toolkit that provides linear predictive and MLSA analysis/synthesis
     * Signal-based analysis/synthesis approaches model the speech signal by not making any restrictive assumptions (such as the independence of source signal and filter); hence they usually have higher quality. The downside is that they are less flexible for modification.
     * [AHOCODER](https://aholab.ehu.eus/ahocoder/info.html) is a publicly available toolkit that provides high-quality HNM synthesis
     * **3.2 Contextual Features**: Most of the mapping functions assume frame-by-frame processing. Human speech is highly dynamic over longer segments and the frame-by-frame assumption restricts the modeling power of the mapping function
     * **7.1 Objective Evaluation**: The most prominent measure used in the VC literature is the mel-cepstrum distance (mel-CD), also measured in dB. **The mel-CD is suitable for evaluating preliminary experiments, defining training criterions, and validation purposes, but not for evaluating the final system regarding quality due to the low correlation with human perception**
$$ \text{mel-CD}(y, \hat{y}) = \left( \frac{10}{\ln 10} \right) \sqrt{2(y-\hat{y})^{T}(y-\hat{y})}$$
    * An automatic voice conversion evaluation strategy was proposed, wherein both speech quality and speaker similarity were automatically computed ([Huang et al., 2016](https://files.benjagueno.cl/papers/Huang2016.pdf))

 2. D. Huang - **An Automatic Voice Conversion Evaluation Strategy Based on Perceptual Background Noise Distortion and Speaker Similarity** ([link](https://files.benjagueno.cl/papers/Huang2016.pdf))
     * 