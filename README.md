## Using GAN to Generate Chest X‑Ray Images

### 1. Overview
- **Objective:** Augment our chest X‑ray dataset by synthesizing images of healthy subjects and pneumonia patients using a conditional Generative Adversarial Network (cGAN).
- **Key Improvements:**
  - **Stable Training:** Introduced modifications to preserve gradient flow and prevent mode collapse, ensuring the generator and discriminator continue to learn throughout training.  
  - **Feature Focus:** Enhanced discriminator feature extraction so that the generator is guided to reproduce clinically relevant patterns (e.g., lung opacities) rather than trivial textures.  
  - **Dual Task Discrimination:** Forced the discriminator to simultaneously decide (a) real vs. synthetic, and (b) healthy vs. pneumonia, driving the generator to produce semantically meaningful variations.

### 2. Dataset  
![Dataset Sample](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/1218e620-6328-48b1-95d8-20bbbeff794e)  
- **Source:** [Kaggle Chest X‑Ray Pneumonia dataset](https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia)  
- **Image Dimensions:** 64×64×3 (resized due to limited GPU memory)  
- **Classes:**  
  - **Normal (no lung disease)**  
  - **Pneumonia**

### 3. GAN Architecture

| Generator                                                        | Discriminator                                                     |
|:-----------------------------------------------------------------:|:-----------------------------------------------------------------:|
| ![Generator](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/b67effdb-ba3b-470d-9074-91403a25da31) | ![Discriminator](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/eb1fda76-9a03-4ff2-a975-1c4354c9f8d3) |

- **Conditional Inputs:** Class labels are embedded and concatenated with noise (for G) or with image feature maps (for D).  
- **Normalization & Activation:** Batch normalization after each convolution; LeakyReLU in D and ReLU in G, with Tanh output.

### 4. Generated Samples  
- **Healthy Subjects**  
  ![Healthy Sample](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/e6c8bc72-83c0-4ab3-aa22-3fc29e646efb)  
- **Pneumonia Patients**  
  ![Pneumonia Sample](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/f49438f9-4811-4cd2-a869-313defb18fad)

### 5. Quantitative Evaluation

#### 5.1. Classifier Setup  
- **Architecture:** VGG‑16 pretrained on ImageNet, fine‑tuned on GAN‑generated images.  
- **Purpose:** Assess whether features learned from synthetic images transfer to real X‑rays.

| Classification Network                                           | Training Curves                                                   |
|:-----------------------------------------------------------------:|:-----------------------------------------------------------------:|
| ![VGG16 Architecture](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/6e556685-5760-4bc0-b3ec-515484313a7e) | ![Training Curves](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/8c6a7e2f-155b-4e5b-ae26-cb7277488754) |

#### 5.2. Performance on Real Test Set  
- **Accuracy:** 93.90%  
- **Precision:** 92.62%  
- **Recall:** 99.12%  
- **F₁ Score:** 95.76%

<details>
<summary>Classification Report</summary>

![Classification Report](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/bc9904b8-6cdb-4c4f-8b8c-f3cd158486b9)
</details>

<details>
<summary>Confusion Matrix</summary>

![Confusion Matrix](https://github.com/kaledhoshme123/Using-GAN-to-Generate-Chest-X-Ray-Images/assets/108609519/cab6ec07-b84b-43cf-b74a-1d9abcbbda53)
</details>
