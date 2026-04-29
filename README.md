# Noisy Chord Sequence Denoising

This project compares **structured probabilistic inference** and **learned message passing** for denoising noisy music chord sequences.

The main goal is to understand when a classical graphical model, such as a Hidden Markov Model, performs best and when a Graph Neural Network becomes more competitive.

## Project Overview

Music chord labels form a sequence. Neighboring chords are usually related, so a denoising method should use temporal structure instead of treating each chord independently.

In this project, the input is a noisy chord label sequence, and the output is a cleaned or denoised chord sequence.

The central question is:

> When is explicit structured inference strongest, and when does learned message passing become more useful?

## Methods Compared

The project compares several denoising methods:

- No-denoise baseline
- Spike removal baseline
- Majority filter baseline
- HMM with Viterbi decoding
- HMM with marginal decoding
- GNN trained on uniform noise
- GNN trained on burst noise
- GNN trained on mixed noise

## Technical Approach

The HMM treats the clean chord labels as hidden states and the noisy chord labels as observations.

The model uses:

- Transition probabilities between clean chord labels
- Emission probabilities from clean labels to noisy labels
- Viterbi decoding for the most likely full sequence
- Forward-backward inference for marginal predictions

The GNN represents the chord sequence as a chain graph:

- Each time step is a node
- Neighboring time steps are connected by edges
- Node features come from noisy chord labels and local context
- Graph message passing is used to predict clean chord labels

## Dataset

The project uses chord sequences from the **McGill Billboard dataset**, simplified into a 25-class chord vocabulary.

The sequences are represented at the beat level to make the denoising problem structured but manageable.

## Noise Models

Two synthetic noise settings are used:

### Uniform Noise

Each chord label has some probability of being randomly changed to another chord label.

This setting matches the HMM emission model well.

### Burst Noise

Errors occur in correlated spans instead of isolated individual labels.

This setting is harder for a simple HMM because the HMM assumes observation errors are independent over time.

## Evaluation Metrics

The main metrics are:

### Accuracy

Measures how many predicted chord labels match the clean chord labels.

### Change Rate

Measures how often adjacent predicted chords change.

A high change rate means the sequence is jumpy.  
A very low change rate may indicate over-smoothing.

## Main Findings

The main results show:

- HMM-based inference performs best under uniform noise.
- HMM-Marginal often gives the strongest per-time-step accuracy.
- GNN models become more competitive under burst noise.
- GNN-B performs best among learned models when trained and tested on burst noise.
- GNN-MIX gives more balanced performance across both noise settings.
- Structured inference is strongest when the model assumptions match the data.
- Learned message passing becomes more useful when the noise pattern is harder to model directly.

## Key Takeaway

The main conclusion is:

> Structure wins in matched settings, while learning becomes more valuable under mismatch.

In other words, the HMM performs very well when the noise process matches its assumptions. The GNN becomes more useful when the corruption pattern is more complex, such as burst noise.

## Repository Contents

```text
.
├── noisychordsequencedenoising.py
├── Final Presentation_ Noisy Chord Sequence Denoising.pdf
├── README.md
├── LICENSE
├── requirements.txt
└── Final Results\
