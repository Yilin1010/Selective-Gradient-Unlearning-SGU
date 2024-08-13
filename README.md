### **Selective Gradient Unlearning (SGU)**

- Main Steps:
	1. Calculate gradients for both retain set and forget set
	2. Select gradients based on specific criteria, defined by selective function.
	3. Update model parameters using the selected gradients

#### **Gradient Selection Function**

- **Core Idea**: Distinguish and select gradients based on their correlation and magnitude.
- Define a selective function $S(g_r, g_f)$ that selects gradients based on the following criteria:

$$S(g_r, g_f) = \begin{cases}
    g_r & \text{if } g_r \cdot g_f < 0 \\
    g_r & \text{if } g_r \cdot g_f \geq 0 \text{ and } |g_r| - |g_f| > \tau_d \\
    0 & \text{otherwise}
\end{cases}$$

where $\tau_d$ is an **adaptive threshold** determined by quantiles of the gradient differences and magnitudes, respectively, in each batch.

### Explanation:
1. **Negative correlation (gr · gf < 0)**:
   - opposite directions, the gradients conflict on this feature. Choose to keep the retain gradient to improve performance on the retain data while moving away from the influence of forget data.

2. **Positive correlation and retain gradient is important (gr · gf ≥ 0 and |gr| - |gf| > τd)**:
   - Gradient directions are consistent, but the retain gradient is significantly larger. Choose to keep the retain gradient, because we want to preserve important features.

3. **Default case**:
   - When importance is similar or unclear, set the gradient to zero.

4. **Optional**:
   $- g_f \space\space \text{if } g_f \cdot g_f \geq 0 \text{ and } |g_f| - |g_r| > \tau_m$
   - It's also possible to perform gradient ascent for larger forget gradients. This can improve the efficiency of Unlearning but might sacrifice model performance.

**Three Types of Unlearning Action**
- maintain, pruning, reverse


### Achievement  

- Provide Flexibility in adjust selection criterian and corresponding unlearning operation, to balance model utility, unlearning quality and efficiency.

### Limitation

- This selective function is quite simple, just separately compare by individual feature, didn't consider feature interdependencies.
- Lack of massive experiments and reliable evaluation metric

### Possible Improvement

- Design more effective selective functions
- use interpretability metric to understand unlearning process
	- Sparse Auto-Encoders (language model)  
- Do experiments with LLMs to understand whether this approach can generalize across image tasks and language tasks.