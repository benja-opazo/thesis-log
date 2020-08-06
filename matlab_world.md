# Matlab WORLD

There are two WORLD vocoders, one using the Harvest method for f0 estimation, and one using the DIO method (that can be daisy chained with the StoneMask method).

 - The **Harvest Method** apparently does not work in real-time, but its output is almost perfect
 -  The **DIO + StoneMask Method** works in real-time but the quality degrades

Tinkering with the options, the DIO performance can improve if the ```frame_period``` option is set to a lower value. The ```frame_period``` option is the time in ms between consecutive filters.

**DIO Procedure (MatLab):**

### Preprocessing
> In this step  the different parameters are calculated, along with some useful calculations (downsampling, spectrum)

 The algorithm processes the options (```f0_floor, f0_ceil, channels_in_octave, target_fs, frame_period, allowed_range```)
 * **f0_floor** and **f0_ceil** indicates the boundaries of the filters
 * **channels_in_octave** indicates the number of filter per octave
 * **target_fs** is the frequency the input will be sampled (can be different, as a downsizing occurs)
 * **frame_period**: (WIP) indicates the length of each frame where the algorithm is applied
 * **allowed_range**: (WIP)
          
The algorithm calculates the ```temporal_positions``` and the ```boundary_f0_list```.
* **temporal_positions**: are the timestamps between each ```frame_period```
* **boundary_f0_list**: are the frequencies at which the signal will be filtered, depending on the ```f0_floor```, ```f0_ceil``` and ```channels_in_octave```
 
 The signal is then downsampled to the ```target_fs```, the mean is substracted (why?? $\rightarrow$ DC = 0). After downsampling, the spectrum is calculated, filtering low frequencies (```cutoff_in_sample```)
 
 ### Fundamentalness calculation
> **Fundamentalness** is defined as the variance of negative and positive going zero-crossing intervals and the intervals between successive peaks and dips
 
 The signal is first filtered using a Nuttall Window for each ```boundary_f0```.
 
 The algorithm calculates the negative (+ to -) and positive zero cross, along with peaks and dips (valleys). 
 
> To calculate the Zero cross, the signal is shifted, multiplied by itself and then the negative results are the zero crossing.
> e.g.: ([+ ==+ -== -] X [==+ -== - ?] = [+ ==-== + +])
 
The Algorithm linearly interpolates the position (which not necessarily is an integer) where the "real" zero crossing happens (```fine_edge_list```)

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

The candidates are sorted, 

### Appendix A: Filtering with the Nuttal Window