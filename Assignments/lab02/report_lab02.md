# Lab Report — Lab 02: MRI Noise Removal via Fourier Notch Filtering

## Overview

_1 sentence: What was the goal of this lab?_

ANSWER:

 The aim of this lab was to clean up a corrupted MRI scan by finding its periodic stripe noise as a pair of spikes in k-space and cutting them out with a Fourier notch filter.

## Implementation

_1-2 sentences: Briefly describe the steps you performed to remove the noise, including the (row, col) positions of the noise spikes you found and the notch radius you used._

ANSWER:

After plotting the log-magnitude spectrum and reconstructing the image (ifftshift, ifft2, then a final fftshift so the scan didn't come out split into quarters), I hovered over the spectrum and found two small isolated spikes at (row, col) = (148, 147) and (108, 109), which I double-checked were mirror images through the centre since they sit at 128 ± 20 rows and 128 ± 19 columns. I then built a mask of ones with a zeroed disc of radius 2 around each spike (26 pixels in total), multiplied it with the k-space data and reconstructed the result, so only the noise frequencies were removed and the rest of the spectrum was left untouched.

## Results

_1-2 sentences: Describe how the filtered image compares to the corrupted one._

ANSWER:

Before filtering, diagonal stripes ran across the entire scan and hid most of the detail inside the head, but in the filtered image they are completely gone and the brain structure is easy to make out. The numbers back this up: the filtered output scored an SSIM of 0.9988 against the clean reference and just 0.0934 against the corrupted image, and all 11 pytests passed.