---
title: "Dither"
date: 2024-04-17
draft: false
summary: "In the realm of digital image processing, dithering algorithms play a crucial role in reducing the color palette of an image while maintaining visual quality."
tags: ["fitler", "dither", "image processing"]
---

# Introduction

In the realm of digital image processing, dithering algorithms play a crucial role in reducing the color palette of an image while maintaining visual quality. These algorithms distribute quantization errors across neighboring pixels, resulting in visually pleasing images with fewer colors. In this blog post, we'll delve into the implementation of two popular dithering algorithms, **Floyd-Steinberg** and **Atkinson**, using the power of **Numba** for performance optimization.

## Understanding Dithering Algorithms

Before we delve into the code, let's briefly understand the two dithering algorithms we'll be exploring:

1. Floyd-Steinberg Dithering:
    - Developed by Robert W. Floyd and Louis Steinberg in 1976.
    - Distributes quantization errors to neighboring pixels in a specific pattern.
    - Produces sharp images with noticeable noise.
2. Atkinson Dithering:
    - Developed by Bill Atkinson in 1982.
    - Similar to Floyd-Steinberg but distributes errors differently.
    - Produces smoother images with less visible noise.

### Implementation with Numba

Now, let's see how we can implement these dithering algorithms efficiently using **Numba**, a Just-In-Time compiler for Python code.

```Python
@numba.jit("f4[:,:,:](f4[:,:,:])", nopython=True, nogil=True)
def floyd_steinberg(image):
    Lx, Ly, Lc = image.shape
    for j in range(Ly):
        for i in range(Lx):
            for c in range(Lc):
                rounded = round(image[i,j,c])
                err = image[i,j,c] - rounded
                image[i,j,c] = rounded
                if i<Lx-1:
                    image[i+1,j,c] += (7/16)*err
                if j<Ly-1:
                    image[i,j+1,c] += (5/16)*err
                    if i>0:
                        image[i-1,j+1,c] += (1/16)*err
                    if i<Lx-1:
                         image[i+1,j+1,c] += (3/16)*err
    return image

@numba.jit("f4[:,:,:](f4[:,:,:])", nopython=True, nogil=True)
def atkinson(image):
    frac = 8
    Lx, Ly, Lc = image.shape
    for j in range(Ly):
        for i in range(Lx):
            for c in range(Lc):
                rounded = round(image[i,j,c])
                err = image[i,j,c] - rounded
                image[i,j,c] = rounded
                if i<Lx-1:
                    image[i+1,j,c] += err / frac
                if i<Lx-2:
                    image[i+2,j,c] += err /frac
                if j<Ly-1:
                    image[i,j+1,c] += err / frac
                    if i>0:
                        image[i-1,j+1,c] += err / frac
                    if i<Lx-1:
                        image[i+1,j+1,c] += err / frac
                if j<Ly-2:
                    image[i,j+2,c] += err / frac
    return image
```

### Explanation of the Code

- We utilize NumPy for numerical operations, PIL (Python Imaging Library) for image loading and saving, and Numba for JIT compilation to enhance performance.
- Both Floyd-Steinberg and Atkinson algorithms are implemented as Numba-jitted functions.
- The algorithms iterate through each pixel of the image, applying error diffusion to distribute quantization errors.
- Finally, the processed images are saved to disk.

## Results & Conclusion

By applying Floyd-Steinberg and Atkinson dithering algorithms to an input image, we've successfully reduced its color palette while preserving visual quality.
The utilization of Numba for performance optimization ensures efficient processing, making these algorithms suitable for large-scale image manipulation tasks.

Experimentation with different images and tweaking parameters can yield varying results, allowing for customization based on specific requirements. Dithering algorithms continue to be relevant in various applications, including digital art, printing, and image compression.

In conclusion, by exploring dithering algorithms such as Floyd-Steinberg and Atkinson and leveraging the power of Numba for implementation, we've gained insights into enhancing image processing tasks with efficient and optimized code.

[Link to Complete Implementation in GitHub](https://github.com/AestheticVoyager/dither-filter/tree/main)
