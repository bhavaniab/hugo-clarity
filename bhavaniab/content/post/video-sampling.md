+++
author = "Bhavani Anantapur Bache"
title = "How are videos stored in the digital world- Sampling of video?"
date = "2015-07-15"
description = "How are videos stored in the digital world- Sampling of video"
featured = true
tags = [
    "DSP",
    "Embedded Systems",
    "Technology",
]
categories = [
    "DSP",
    "Embedded Systems",
]
series = ["DSP"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/video.jpg"
+++

Sampling of an audio signal converts continuous time signal into a discrete time signal. Quantization converts continuous amplitude signal into a discrete amplitude signal. A discrete time and discrete amplitude signal is a digital signal. The present post describes the sampling of a video signal, different characteristics of the video which are considered for sampling, spatial sampling and temporal sampling. A real world video is composed of multiple objects each with their own characteristic shape, depth, texture and illumination. The color and brightness of a natural video changes with varying degrees of smoothness throughout the video (‘continuous tone’). Characteristics of a typical natural video frames that are relevant for video processing and compression include spatial characteristics (texture variation within scene, number and shape of objects, color, etc.) and temporal characteristics (object motion, changes in illumination, movement of the camera or viewpoint and so on). A natural video is spatially and temporally continuous. Representing a video in digital form involves sampling the real scene spatially and temporally.

<h3>Spatial sampling</h3>

Sampling the signal at a point in time produces a sampled image or frame that has defined values at a set of sampling points. The most common format for a sampled image is a rectangle with the sampling points positioned on a square or rectangular grid. Figure 1 shows a continuous-tone frame with two different sampling grids superimposed upon it. Sampling occurs at each of the intersection points on the grid and the sampled image may be reconstructed by representing each sample as a square picture element (pixel). The visual quality of the image is influenced by the number of sampling points. Choosing a ‘coarse’ sampling grid produces a low-resolution sampled image whilst increasing the number of sampling points slightly increases the resolution of the sampled image.

<h3>Temporal sampling</h3>

A moving video image is captured by taking a rectangular ‘snapshot’ of the signal at periodic time intervals. Playing back the series of frames produces the appearance of motion. A higher temporal sampling rate (frame rate) gives apparently smoother motion in the video scene but requires more samples to be captured and stored. Frame rates below 10 frames per second are sometimes used for very low bit-rate video communications (because the amount of data is relatively small) but motion is clearly jerky and unnatural at this rate. Between 10 and 20 frames per second is more typical for low bit-rate video communications; the image is smoother but jerky motion may be visible in fast-moving parts of the sequence. Sampling at 25 or 30 complete frames per second is standard for television pictures (with interlacing to improve the appearance of motion, see below); 50 or 60 frames per second produces smooth apparent motion (at the expense of a very high data rate).