---
title : "Kuwahara"
date : 2024-04-17
draft : false
summary: "Kuwahara was the world's first edge preserving de-noising image processing algorithm."
tags: ["filter", "kuwahara", "image processing"]
---

The **Kuwahara filter** is a non-linear smoothing filter used in [image processing](https://en.wikipedia.org/wiki/Image_processing "Image processing") for adaptive noise reduction. Most filters that are used for image smoothing are linear [low-pass filters](https://en.wikipedia.org/wiki/Low-pass_filters "Low-pass filters") that effectively reduce noise but also blur out the edges. However the Kuwahara filter is able to apply smoothing on the image while preserving the edges.

It is named after Michiyoshi Kuwahara, Ph.D., who worked at [Kyoto](https://en.wikipedia.org/wiki/Kyoto_University "Kyoto University") and [Osaka Sangyo](https://en.wikipedia.org/wiki/Osaka_Sangyo_University "Osaka Sangyo University") Universities in [Japan](https://en.wikipedia.org/wiki/Japan "Japan"), developing early medical imaging of dynamic [heart](https://en.wikipedia.org/wiki/Heart "Heart") muscle in the 1970s and 80s.

# Kuwahara Filter description

The Kuwahara filter works on a window divided into 4 overlapping sub-windows.
In each sub-window, the mean and variance are computed.

The output value (located at the center of the window) is set to the mean of the sub-window with the smallest variance.

## Applications

Originally the Kuwahara filter was proposed for use in processing RI-angiocardiographic images of the cardiovascular system.

The fact that any edges are preserved when smoothing makes it especially useful for [feature extraction](https://en.wikipedia.org/wiki/Feature_extraction "Feature extraction") and segmentation and explains why it is used in [medical imaging](https://en.wikipedia.org/wiki/Medical_imaging "Medical imaging").

The Kuwahara filter however also finds many applications in artistic imaging and [fine-art photography](https://en.wikipedia.org/wiki/Fine-art_photography "Fine-art photography") due to its ability to remove textures and sharpen the edges of photographs. The level of abstraction helps create a desirable painting-like effect in artistic photographs especially in the case of the colored image version of the filter. These applications have known great success and have encouraged similar research in the field of image processing for the arts.

Although the vast majority of applications have been in the field of image processing there have been cases that use modifications of the Kuwahara filter for machine learning tasks such as clustering.

The Kuwahara filter has been implemented in [CVIPtools](https://en.wikipedia.org/wiki/CVIPtools "CVIPtools").

## Anisotropic Kuwahara Filtering with Polynomial Weighting Functions Paper

[The Anisotropic Kuwahara Paper link](https://hpi.de/doellner/publications/Document/import_cgs/jkyprian-tpcg2010.pdf/37b866dfd42ffbe9f4de4322211d9154.html?tx_extbibsonomycsl_publicationlist%5Baction%5D=download&cHash=0c0ccc92ae0942ab17477ef444e24d1a)

Kuwahara was the world's first edge preserving de-noising image processing algorithm.
It was upgraded by the "Anisotropic Kuwahara Filtering with Polynomial Weighting Functions" paper, by:

1. Upgraded by using a circular kernel instead of Box kernel.
2. Instead of using naive weights, we use gaussian weights.
3. This new formula:
1/(1+std_div), sector color = Ki, K(x)=(sum of Ki * Wi)/(sum of weights i)

This removes indeterminate behavior and removes all conditional logic of the old algorithm.
All these changes were made by **Guiseppe Papari**.

Thankfully we can just ditch the Gauss and instead approximate the weight using "Polynomials".

Then we'll calculate the Eigen-Values.
To calculate the Eigen-Values of the structure tensor and use them to calculate the eigenvectors that points in the direction of the minimum rate of change.
We're just essentially figuring out what direction a pixel points in using the eigenvector information.

The filter kernel can now angle itself and stretch itself to better fit image details and edges.

This new filter is called ***Anisotropic Kuwahara Filter***.

**Recommendation**:
In-order to achieve High Contrast Visuals, it is better to apply the anisotropic kuwahara then apply the dither effect.

### My Personal Optimized Implementation of Kuwahara filter

[Personal Implementation](https://github.com/AestheticVoyager/kuwahara-filter)
