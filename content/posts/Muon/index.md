---
title: "Muon: Second Order Optimizer for Hidden Layers"
draft: false
date: 2025-07-18
summary: "Muon is a second-order optimizer for deep learning models, designed to accelerate training and reduce memory usage. It leverages information about the curvature of the loss landscape to achieve faster convergence and more efficient memory utilization. By overcoming historical computational barriers and standardizing its usage, Muon brings the theoretical advantages of second-order optimization to the scale required for LLMs, potentially reshaping both practice and expectations in deep learning."
tags: ["Optimizer", "Deep Learning", "Large Language Models", "Second Order Optimization", "Efficient Training", "Memory Efficiency", "Machine learning", "Muon", "Newton-Schulz", "Orthogonalization", "Momentum", "Gradient Descent", "Second Derivatives", "Hessian Computation", "Inversion", "Decoupled Weight Decay", "Weight Scaling", "Stochastic Gradient Descent", "AdamW", "ZeRO-1", "Parallelism", "Distributed Computing", "Tensor Parallelism", "Data Parallelism", "Model Parallelism", "Pipeline Optimization"]
---

# Muon is Scalable for LLM Training: Revolutionizing Optimizer Efficiency in Deep Learning

Large Language Models (LLMs) are at the heart of modern machine learning breakthroughs, but training them efficiently remains a monumental computational challenge. Traditionally, first-order optimizers like AdamW have dominated the field due to their relative simplicity and scalability—a necessity given the massive size of current models. However, second-order methods, which leverage information about the *curvature* of the loss landscape, promise faster and more data-efficient training. Their prohibitive computational cost has, until recently, kept them largely out of reach for practical LLM training.

The recent advances surrounding the **Muon optimizer** change this landscape. By overcoming historical computational barriers and standardizing its usage, Muon brings the theoretical advantages of second-order optimization to the scale required for LLMs, potentially reshaping both practice and expectations in deep learning.


## Definition: What is Muon?

Muon is an optimizer specially designed for the **2D parameters** of neural network hidden layers, such as weight matrices. It builds upon momentum-based updates, applying an orthogonalization step to better capture the geometry of parameter updates.

At a high level, Muon's iteration can be described as:

1. **Initialize Parameters:**  
   Start with the weight matrix parameters **W** and a momentum matrix **M** (initially zeros).

2. **Compute Gradient:**  
   For the current mini-batch, compute the gradient **G** of the loss with respect to **W**.

3. **Update Momentum:**  
   Update momentum as an exponential moving average:  
   M = β * M + (1 - β) * G  
   where β is the momentum coefficient.

4. **Orthogonalize the Momentum Update:**  
   Apply the Newton–Schulz iteration to orthogonalize **M**, producing an orthogonal matrix **O** close to **M**. This step replaces raw gradient updates with well-conditioned, approximately orthogonalized updates that better reflect the matrix geometry.

5. **Scale the Orthogonalized Update:**  
   Scale **O** by a factor proportional to the square root of the matrix dimensions to match the typical RMS magnitude of AdamW updates, ensuring stable update sizes.

6. **Apply Weight Decay:**  
   Add a weight decay term proportional to the current **W** (decoupled weight decay).

7. **Update Weights:**  
   Update weights as:  
   W = W - η * (scaled O + λ * W)  
   with η the learning rate and λ the weight decay coefficient.

8. **Repeat:**  
   Continue iterating for each training step.

To give a concrete illustration, the Newton–Schulz orthogonalization step often uses about 5 iterations and can be implemented efficiently in PyTorch using matrix multiplications. Here is an example snippet:

```python
def newtonschulz5(G, steps=5, eps=1e-7):
    assert G.ndim == 2
    a, b, c = (3.4445, -4.7750, 2.0315)
    X = G.bfloat16()
    X /= (X.norm() + eps)
    if G.size(0) > G.size(1):
        X = X.T
    for _ in range(steps):
        A = X @ X.T
        B = b * A + c * A @ A
        X = a * X + B @ X
    if G.size(0) > G.size(1):
        X = X.T
    return X
```

When training neural networks with Muon, **scalar and vector parameters** (like biases or LayerNorm weights), as well as input/output layers, should be optimized using standard optimizers such as AdamW. For convolutional parameters (4D tensors), Muon can be applied by flattening the last three dimensions to convert them into two-dimensional matrices.


## Muon's Design: Orthogonalizing Momentum for Efficient Optimization

Muon stands for **MomentUm Orthogonalized by Newton–Schulz**. The innovation is replacing the traditional SGD-momentum update matrix with the nearest semi-orthogonal matrix via Newton–Schulz iterations. This orthogonalization step intuitively "cleans up" the gradient update, tackling ill-conditioning by ensuring updates better respect the underlying parameter geometry. 

By efficiently approximating this orthogonalization, Muon implicitly incorporates second-order curvature information *without* heavy Hessian computations or inversions, enabling faster, more stable convergence than first-order baselines while avoiding their usual computational overhead.


## Solving the Computational Cost of Second-Order Optimization

Second-order optimizers have the promise of faster convergence by using curvature (second derivatives), but they are notorious for being computationally expensive. Muon sidesteps this by:

- Using **Newton–Schulz iteration** to compute approximate orthogonalization of the momentum matrix through efficient matrix multiplications.
- Maintaining only **first-moment momentum state**, avoiding large memory overhead.
- Implementing an **efficient distributed version**, compatible with ZeRO-1 style partitioning to optimize memory and communication costs.

These strategies allow Muon to retain the benefits of second-order methods—such as well-conditioned updates and improved stability—without the prohibitive costs that traditionally prevent their scaling to billions of parameters.


## Standardizing Muon for Modern ML Pipelines

A key to Muon’s practical impact is its ease of integration into existing training pipelines:

- **Weight Decay:** The authors added a decoupled weight decay term similar to AdamW, stabilizing weight magnitudes during long runs.
- **Per-Parameter Update Scaling:** The orthogonal updates are rescaled to match the RMS magnitude AdamW typically produces, allowing Muon to adopt existing learning rate schedules and unify hyperparameter tuning.
- **Compatibility:** Standard scalar and vector parameters continue to use AdamW, enabling Muon to blend seamlessly into heterogeneous models.
- **Open-Source Implementation:** The research team released a distributed Muon implementation and a suite of pretrained models, lowering the barrier to entry.

This approach ensures Muon "just works" at scale, without tedious new tuning—a crucial factor for adoption in large-scale industry and research settings.


## Empirical Evidence: Muon's Impact on Large-Scale Training

Muon has demonstrated impressive empirical performance:

- **Faster training on CIFAR-10:** Reduced time to reach 94% accuracy from 3.3 to 2.6 A100-seconds.
- **Speed improvements on FineWeb task:** A 1.35× speedup for reaching a competitive validation loss.
- **Scalability:** Effective training gains held across models scaling up to 774M and 1.5B parameters.
- **LLM Training:** Achieved GPT-2 XL level performance on HellaSwag with a 1.5B parameter model in 10 hours on 8 H100 GPUs, compared to 13.3 hours using AdamW.

These results reflect roughly **2× computational efficiency** and more stable convergence across scales and tasks, confirming Muon’s suitability for large-scale LLM training.


## Muon's Effect on the Machine Learning Landscape

The ability to bring second-order benefits to LLM-scale training fundamentally shifts optimization paradigms:

- Muon opens the door for **more computationally efficient training**, reducing the carbon footprint and infrastructure costs associated with model development.

- It shifts the **Pareto frontier** of training trade-offs—models trained with Muon achieve better performance with fewer floating-point operations and less training time.

- Researchers can now explore **larger models and datasets** without the prohibitive scaling constraints imposed by first-order optimizer inefficiencies.

- The optimizer’s standardized design encourages **broader adoption and experimentation**, accelerating innovation in model architectures and training strategies.


## Remaining Questions and Future Directions

While Muon addresses many historical barriers, some open questions remain:

- Will Muon scale gracefully to **even larger models (20B+ parameters)** trained on trillions of tokens?

- Is it possible to efficiently **distribute the Newton–Schulz iterations** across large-scale GPU clusters?

- Will Muon perform well not only for pretraining but also for **fine-tuning and reinforcement learning workloads**?

These are active research questions that will define the future trajectory of efficient large-scale optimization.


## Impact

Muon is a landmark step toward realizing scalable second-order optimization at unprecedented scales. By blending smart matrix orthogonalization with efficient distributed implementations and thoughtful standardization, Muon offers an optimizer that is both powerful and practical.

For the machine learning community, this means faster training, improved data efficiency, and a new set of possibilities in large-scale model development—all while lowering compute costs. Muon’s contributions are primed to reshape the optimization landscape of deep learning, enabling future breakthroughs in artificial intelligence.


## References

[1] https://arxiv.org/html/2502.16982v1
[2] https://arxiv.org/abs/2502.16982
[3] https://github.com/MoonshotAI/Moonlight
[4] https://x.com/Kimi_Moonshot/status/1893379158472044623
[5] https://www.reddit.com/r/MachineLearning/comments/1ixzj26/r_muon_is_scalable_for_llm_training/
[6] https://www.themoonlight.io/en/review/muon-is-scalable-for-llm-training
[7] https://arxiv.org/pdf/2502.16982.pdf
[8] https://ar5iv.labs.arxiv.org/html/2502.16982
[9] https://www.youtube.com/watch?v=L2xz6uubZgo
[10] https://kellerjordan.github.io/posts/muon/
