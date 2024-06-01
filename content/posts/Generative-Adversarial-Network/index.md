---
title: "Generative Adversarial Network"
date: 2024-05-29
draft: false
summary: "In practice, a GAN's generator creates fake images, which are then mixed with real images. The discriminator randomly selects an image from this mix to determine whether it is real or generated. Based on the discriminator's accuracy, both the generator and discriminator are adjusted. After numerous iterations, the generator becomes proficient at producing realistic images."
tags: ["GAN", "ML", "AI", "Encoder", "Decoder", "Vector", "DCGAN", "CoGAN", "PGGAN", "SGAN", "Machine Learning", "Learning"]
---

# Typical Neural Network Architecture vs. GAN Architecture

A neural network is like a highly sophisticated, multi-layered calculator that learns from data. It consists of numerous "neurons" (tiny calculators) connected in layers, with each layer performing a unique function to help the network make predictions or decisions. These neurons communicate with each other using numerical data and mathematical operations, akin to a game of telephone where each neuron modifies the information before passing it along.
In contrast, a Generative Adversarial Network (GAN) operates like two secretive collaborators working together. One collaborator, the "counterfeiter" (or generator), attempts to create new data that appears real and authentic, much like a master forger trying to produce convincing fake art. The other collaborator, the "cop" (or discriminator), evaluates the counterfeiter's creations to determine if they are genuine or fake. This process continues in a loop, with the counterfeiter improving its ability to generate realistic data and the cop getting better at detecting fakes. Unlike a single neural network that functions independently, a GAN consists of two parts working in tandem, often in a competitive manner.
In practice, a GAN's generator creates fake images, which are then mixed with real images. The discriminator randomly selects an image from this mix to determine whether it is real or generated. Based on the discriminator's accuracy, both the generator and discriminator are adjusted. After numerous iterations, the generator becomes proficient at producing realistic images.
Both networks in this scenario are multi-layer perceptrons (MLPs), typically used for simpler problems. However, MLPs can be combined to tackle more complex tasks, though this approach is not highly efficient.

## DCGAN

In 2015, researchers [Alec Radford](https://scholar.google.com/citations?user=dOad5HoAAAAJ&hl=en) and [Luke Metz](https://scholar.google.com/citations?user=k_u5ULgAAAAJ&hl=en) proposed using more complex networks instead of simple ones to construct an even more sophisticated network. This led to the creation of [Deep Convolutional GANs (DCGANs)](https://arxiv.org/abs/1511.06434), which utilize convolutional neural networks instead of MLPs. This approach demonstrated improvements in generating realistic data.

## CoGAN

Around the same time, [Couple GANs (CoGANs)](https://arxiv.org/abs/1606.07536) were introduced, employing two pairs of generators and discriminators. In this setup, two simultaneous games occur during each training round. The generators share information but tweak their outputs to fool their respective discriminators. This results in generators capable of producing images with slight variations, such as a person with different hair colors or with and without glasses. Despite these advancements, GANs still struggled with generating high-quality images, often producing blurry and low-resolution results due to the discriminator's tendency to detect fakes more easily at higher resolutions.

## Progressively Growing GAN

In 2017, NVIDIA researchers introduced [Progressive Growing of GANs (PGGAN)](https://arxiv.org/abs/1710.10196), a technique that significantly improved GAN capabilities and image quality. Traditional GANs have fixed architectures, leading to limitations in capacity and training stability. PGGANs address these issues by gradually increasing the size of both the generator and discriminator networks during training, enhancing their ability to learn complex patterns and maintaining stable training.

### How PGGAN Works

1. **Initial setup**: Start with a small generator and discriminator.
2. **Progressive growth**: Incrementally add layers to both networks.
3. **Training**: Continue training with the same loss functions as traditional GANs.

### Benefits of PGGAN

1. **Better image quality**: Generates more realistic and diverse images.
2. **Increased resolution**: Produces high-resolution images (e.g., 1024x1024 pixels).
3. **Improved stability**: Ensures stable training throughout the process.

## Style-Based GANs

In 2018, NVIDIA researchers introduced [Style-based GANs (SGANs)](https://arxiv.org/abs/1812.04948), designed to generate high-quality images with the ability to manipulate their style while maintaining content consistency. Traditional GANs often produce images with a fixed style, which may not match the desired outcome. SGANs overcome this by allowing for more control over the generated image's style.

### How SGANs Work

SGANs consist of two main components:

1. **Generator**: Takes a random noise vector and generates an image with a specific style.
2. **Style encoder**: Extracts style information from a reference image to manipulate the generated output.

By separating content from style, SGANs provide more flexibility and control in image generation.

### Benefits of SGANs

1. **More control**: Enables precise manipulation of images while preserving their style.
2. **Improved quality**: Generates high-quality, diverse images.
3. **Flexibility**: Creates new images that are variations or combinations of existing ones.

### Real-World Applications

SGANs have been used in various domains, including:

1. **Computer vision**: Tasks like image-to-image translation, data augmentation, and style transfer.
2. **Artistic creation**: Generating realistic images with specific styles or creatively manipulating existing images.

### Recommendation

If you've not yet tried or know of [This person does not exist](https://thispersondoesnotexist.com/), I highly recommend checking it out. Not only for the fun of it, but also for seeing a GAN in action.
