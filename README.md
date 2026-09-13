# Classification of EEG-Based Brain Connectivity Networks in Schizophrenia Using a Multi-Domain Connectome Convolutional Neural Network

This is the implementation of the paper: “Classification of EEG-Based Brain Connectivity Networks in Schizophrenia Using a Multi-Domain Connectome Convolutional Neural Network”

## Main Goal

Can we detect schizophrenia from EEG signals by looking at how different brain regions are connected?

### Dataset Used: [Laboratory for Neurophysiology and Neuro-Computer Interfaces](http://brain.bio.msu.ru/eeg_schizophrenia.htm) 39 healthy participant and 45 schizophrenic patients
I have also uploaded the combined version of the [dataset] (https://github.com/adi18-ui/Brain-Schizophrenia-Detection/tree/main/Dataset)

## Connectivity Features

This implementation extracts 3 types of connectivity features:

### i) Vector Autoregressive feature (in time domain)

Explains how the activity of 1 EEG channel can predict future activity in another EEG channel. We construct a matrix-like structure with rows and columns both as individual channels. The value between each EEG channel (eg: Fp1 and Fp2) indicates the connection strength between the respective channels.

### ii) Partial Directed Coherence (frequency domain)

It checks directed connections between EEG channels in different frequency bands.

It considers 5 frequency bands (delta, theta, alpha, beta, gamma). It is computed from VAR coefficients only.

### iii) Complex Network features (graph-level brain topology)

It considers 4 measures to calculate efficiency.

1. **Degree:** How strongly 1 channel is connected to all other channels.
2. **Global efficiency:** It calculates how easily information travels across the whole brain network. If brain regions are connected through short paths, then global efficiency increases.
3. **Clustering coefficient:** How strongly a node’s neighbours are connected (accounts for local information).
4. **Transitivity:** Global version of clustering; it calculates how strongly the whole brain forms the network. It outputs a single value

## Fusion Methods

After calculating these individual features, the results were combined in 3 categories.

- Feature level
- Decision level
- Score level fusion

In this notebook, only the Decision Level Fusion is implemented.

### Preprocessing Steps

#### i) Linear detrending

removes slow changes or drift in the signal over time.

#### ii) DC offset removal

shifts each EEG channel so that it is centred around zero.

#### iii) 50 Hz notch filtering

removes power-line noise around 50 Hz.

#### iv) Band-pass filtering

keeps EEG activity between 1–45 Hz and removes frequencies outside this range.

#### v) Artifact handling

Very large signal values are detected using a robust threshold. Small artifact regions are replaced using interpolation.

#### vi) Common Average Reference (CAR)

the average activity across all EEG channels is subtracted from each channel.

#### vii) Per-channel standardisation

each channel is scaled to have approximately zero mean and unit standard deviation.
