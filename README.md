# filter
Brainlife App to filter (a subset of channels in) an MEEG file.  

# Documentation

#### Input files are:
    * `meg/fif` meg data file

#### Input parameters are:
* ` l_freq `:  float | None ,  For FIR filters, the lower pass-band edge; for IIR filters, the lower cutoff frequency. If None the data are only low-passed. 
* ` h_freq `:  float | None ,  For FIR filters, the upper pass-band edge; for IIR filters, the upper cutoff frequency. If None the data are only high-passed. 
* ` picks `:  str | array_like | slice | None ,  Channels to include. Slices and lists of integers will be interpreted as channel indices. In lists, channel type strings (e.g., ['meg', 'eeg']) will pick channels of those types, channel name strings (e.g., ['MEG0111', 'MEG2623'] will pick the given channels. Can also be the string values “all” to pick all channels, or “data” to pick data channels. None (default) will pick all data channels. Note that channels in info['bads'] will be included if their names or indices are explicitly provided. 
* ` notch ` : float, Comma separated list of frequencies to filter out. Nota: A separate filter is applied for this purpose.
* ` filter_length `:  str | int ,  Length of the FIR filter to use (if applicable):
    ‘auto’ (default): The filter length is chosen based on the size of the transition regions (6.6 times the reciprocal of the shortest transition band for fir_window=’hamming’ and fir_design=”firwin2”, and half that for “firwin”).
    str: A human-readable time in units of “s” or “ms” (e.g., “10s” or “5500ms”) will be converted to that number of samples if phase="zero", or the shortest power-of-two length at least that duration for phase="zero-double".
    int: Specified length in samples. For fir_design=”firwin”, this should not be used. 
* ` l_trans_bandwidth `:  float | str ,  Width of the transition band at the low cut-off frequency in Hz (high pass or cutoff 1 in bandpass). Can be “auto” (default) to use a multiple of l_freq:
min(max(l_freq * 0.25, 2), l_freq) Only used for method='fir'. 
* ` n_jobs `:  int | str ,  Number of jobs to run in parallel. Can be ‘cuda’ if cupy is installed properly and method=’fir’. 
* ` method `:  str ,  ‘fir’ will use overlap-add FIR filtering, ‘iir’ will use IIR forward-backward filtering (via filtfilt). 
* ` iir_params `:  dict | None ,  Dictionary of parameters to use for IIR filtering. If iir_params is None and method=”iir”, 4th order Butterworth will be used. For more information, see mne.filter.construct_iir_filter(). 
* ` phase `:  str ,  Phase of the filter, only used if method='fir'. Symmetric linear-phase FIR filters are constructed, and if phase='zero' (default), the delay of this filter is compensated for, making it non-causal. If phase='zero-double', then this filter is applied twice, once forward, and once backward (also making it non-causal). If 'minimum', then a minimum-phase filter will be constricted and applied, which is causal but has weaker stop-band suppression. 
* ` fir_window `:  str ,  The window to use in FIR design, can be “hamming” (default), “hann” (default in 0.13), or “blackman”. 
* ` fir_design `:  str ,  Can be “firwin” (default) to use scipy.signal.firwin(), or “firwin2” to use scipy.signal.firwin2(). “firwin” uses a time-domain design technique that generally gives improved attenuation using fewer samples than “firwin2”. 
* ` skip_by_annotation `:  str | list of str ,  If a string (or list of str), any annotation segment that begins with the given string will not be included in filtering, and segments on either side of the given excluded annotated segment will be filtered separately (i.e., as independent signals). The default (('edge', 'bad_acq_skip') will separately filter any segments that were concatenated by mne.concatenate_raws() or mne.io.Raw.append(), or separated during acquisition. To disable, provide an empty list. Only used if inst is raw. 
* ` pad `:  str ,  The type of padding to use. Supports all numpy.pad() mode options. Can also be "reflect_limited", which pads with a reflected version of each vector mirrored on the first and last values of the vector, followed by zeros.
Only used for method='fir'. 

#### Output files/figures are:
* `meg/fif` meg data file.

## Authors:
- [Maximilien Chaumon](maximilien.chaumon@icm-institute.org)
### Funding Acknowledgement
brainlife.io is publicly funded and for the sustainability of the project it is helpful to Acknowledge the use of the platform. We kindly ask that you acknowledge the funding below in your code and publications. Copy and past the following lines into your repository when using this code.

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)

### Citations
1. Avesani, P., McPherson, B., Hayashi, S. et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6, 69 (2019). [https://doi.org/10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Citation

Hayashi, S., Caron, B.A., Heinsfeld, A.S. et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. Nat Methods 21, 809–813 (2024). https://doi.org/10.1038/s41592-024-02237-2
