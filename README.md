# PYTHON WORLD VOCODER: 
*************************************

This is a line-by-line implementation of WORLD vocoder (Matlab, C++) in python. It supports *python 3.0* and later.

# INSTALLATION
*********************

```
pip install worldvocoder
```

# EXAMPLE
**************

```python
import worldvocoder as wv
import soundfile as sf
import librosa

# read audio
audio, sample_rate = sf.read("some_file.wav")
audio = librosa.to_mono(audio)

# initialize vocoder
vocoder = wv.World()

# encode audio
dat = vocoder.encode(sample_rate, audio, f0_method='harvest')

```

in which, ```sample_rate``` is sampling frequency and ```audio``` is the speech/singing signal.

The ```dat``` is a dictionary object that contains pitch, magnitude spectrum, and aperiodicity. 

We can scale the pitch:

```python
dat = vocoder.scale_pitch(dat, 1.5)
```

Be careful when you scale the pich because there is upper limit and lower limit.

We can make speech faster or slower:

```python
dat = vocoder.scale_duration(dat, 2)
```

To resynthesize the audio:

```python
dat = vocoder.decode(dat)
output = dat["out"]
```

To use d4c_requiem analysis and requiem_synthesis in WORLD version 0.2.2, set the variable ```is_requiem=True```:

```python
# requiem analysis
dat = vocoder.encode(fs, x, f0_method='harvest', is_requiem=True)
```

To extract log-filterbanks, MCEP-40, VAE-12 as described in the paper `Using a Manifold Vocoder for Spectral Voice and Style Conversion`, check ```test/spectralFeatures.py```. You need Keras 2.2.4 and TensorFlow 1.14.0 to extract VAE-12.
Check out [speech samples](https://tuanad121.github.io/samples/2019-09-15-Manifold/)

# NOTE:
**********

* The vocoder use pitch-synchronous analysis, the size of each window is determined by fundamental frequency ```F0```. The centers of the windows are equally spaced with the distance of ```frame_period``` ms.

* The Fourier transform size (```fft_size```) is determined automatically using sampling frequency and the lowest value of F0 ```f0_floor```. 
When you want to specify your own ```fft_size```, you have to use ```f0_floor = 3.0 * fs / fft_size```. 
If you decrease ```fft_size```, the ```f0_floor``` increases. But, a high ```f0_floor``` might be not good for the analysis of male voices.


# CITATION:

Dinh, T., Kain, A., & Tjaden, K. (2019). Using a manifold vocoder for spectral voice and style conversion. Proceedings of the Annual Conference of the International Speech Communication Association, INTERSPEECH, 2019-September, 1388-1392.
