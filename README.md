# **Image Inpainting of Grayscale Images using ASD Algorithm**  

## **Overview**  
This project focuses on image inpainting using the **Alternating Steepest Descent (ASD)** algorithm to reconstruct undersampled grayscale images. The key idea is to treat the problem as a low-rank matrix completion task and recover missing pixel values by minimizing a constrained optimization function.  

We demonstrate this by undersampling a **256×256 grayscale image of a house**, where the rank is **50**—significantly lower than its size. This is crucial because matrix completion is more feasible when dealing with low-rank matrices. The missing pixel locations are determined using a **masking operator** that follows a random normal distribution with a specified **undersampling ratio**. The Hadamard product of the masking operator and the original image generates the undersampled image.  

To reconstruct the image, we solve a constrained optimization problem where missing values are chosen to minimize the matrix rank. However, since directly minimizing rank is computationally expensive, we reformulate the problem using a **projector matrix** and minimize the **Frobenius norm** between the projections of the undersampled image and a low-rank factorized approximation (product of two matrices, A and B).  

We then implemented the Alternating Steepest Descent (ASD) algorithm to minimize the optimization function. 

---
## **ASD Algorithn**


#### **Why ASD?**  
ASD is widely used in optimization problems due to its **simplicity** and **low memory requirements**. The algorithm applies the **steepest descent method** alternately between the two matrices (A and B), updating each while keeping the other fixed. The gradient is computed iteratively, and the **step size** is determined using the ratio of the Frobenius norm of the gradient to the norm of its projection.  


#### **How does ASD Works?**  
  
- Instead of solving for everything at once, ASD **updates one part of the image representation at a time**:  
  - First, it adjusts one matrix while keeping the other fixed.  
  - Then, it switches and refines the second matrix while keeping the first one fixed.  
- By repeating this process, it gradually improves the estimated image.

#### **Adjusting the Update Step**  

Each update needs to be carefully scaled to ensure that the algorithm moves in the right direction without making overly large or small changes. This is done by measuring how much the current estimate deviates from the known pixels and adjusting accordingly.


#### **Stopping When the Image is Reconstructed**  
The algorithm continues refining the image until:  
✔️ The difference between the estimated and known pixels is very small.  
✔️ A set number of iterations have been completed.  


#### **Why ASD Works Well?**  
- **Efficient**: Instead of working with a massive image matrix, ASD works with smaller building blocks, reducing complexity.  
- **Pattern-Based**: It doesn’t just guess missing pixels but **learns the structure** from known ones.  
- **Accurate**: It performs well even when a significant portion of the image is missing, provided enough structure remains.  

---

## **Results**

![Image](https://github.com/user-attachments/assets/b0e4bf45-b7e1-4495-a11f-850bdc88562f)

✅ The algorithm **successfully reconstructs** the image with low computational cost (converging within **60-80 iterations**).

✅ The optimal value of **r = 50** provides the best reconstruction quality. 

✅ Higher values of **r > 50** allow the model to capture more intricate details, but improvements are negligible beyond the true rank.  

✅ The algorithm achieves **~95% accuracy** when at least **60% of the image is retained**.  

✅ The algorithm's accuracy remains above **85%** for perturbed undersampled images indicating it's resilience to low levels of pertubation.

### **Testing on Different Scenarios**  

- **Noise-Induced Images**: The algorithm performs well, handling moderate gaaussian noise effectively. However, excessive noise leads to noticeable blurring.  
- **Pattern Undersampling**: The ASD algorithm does not work well even when using structured undersampling patterns (e.g., checkerboard masking).  
- **Transformed Images**: The method successfully inpaints versions of the house image that were **twisted or zoomed in**.  

---

## **Applications**  

This method is particularly useful for **image inpainting** where missing pixel information needs to be reconstructed in **low-rank matrices**.  

A **real-world example** of this is: 

✅ **Recommender Systems** – Where sparse user preference data is collected, and the missing values are inferred to make better recommendations.  

---

## **Features**  
✔️ Inpainting of **undersampled, pattern-undersampled, and perturbed images**.  
✔️ **High accuracy** (up to 95%) for images with **at least 60% pixel retention**.  
✔️ Can handle **structured and random undersampling**.  

---

## **Technology Stack**  
- **Python**  
- Libraries: `numpy`, `matplotlib`, `random`, `time`  

---

## **Usage**  
This algorithm can be used for:  
- **Image Processing & Restoration** – Recovering lost pixel data.  
- **Reconstruction of Incomplete Data** – Filling in missing values in datasets, including **survey data completion**.  

---

## **Credits & Acknowledgments**  
This coursework was completed under the guidance of **Mr. Waleed Ali** (Mathematics Professor).  
