# A Review of 3D Gaussian Splatting Representation, Capabilities, and Practical Advantages

Qingjun Bao

2025/05/16

## Overview

3D Gaussian Splatting (3DGS) is an emerging technique in the fields of 3D reconstruction and rendering. It has attracted significant attention due to its efficient computation, powerful representational capacity, and broad application potential. By representing scenes with 3D Gaussian functions and leveraging rasterization-based rendering, it enables real-time, high-quality 3D reconstruction and visualization. This report provides a comprehensive description of 3DGS, analyzes its key characteristics, and explores its advantages in both academic and commercial contexts.

The rise of 3DGS is not incidental. A 3D Gaussian can simultaneously encode spatial location, color, opacity, shape, and lighting information, providing a rich representational format. Gaussian functions possess several mathematically desirable properties, including closure under standard transformations, infinite differentiability, computational efficiency, and the ability to support unstructured representations. The "splatting" process can be intuitively likened to colored snowballs striking a wall—the cumulative impact forms the final image. Key advantages of 3DGS include high rendering speed, expressive power, editability, and compatibility with real-time SLAM systems, making it valuable in both academic research and commercial deployment.

Due to its expressive capacity, computational efficiency, and flexibility, 3DGS has become a leading technology in 3D scene representation and rendering:

- It can simultaneously represent spatial, visual, and lighting attributes.
- It achieves image formation by successively "splatting" Gaussians, making it suitable for function-based 3D modeling.
- Its compatibility with SLAM systems and strong real-time performance position it for academic and industry-wide adoption.

## Description: Representing Scenes as Collections of 3D Gaussians

3D Gaussian Splatting (3DGS) is a rapidly emerging rendering technique in computer graphics and vision. Offering both high rendering performance and photorealistic visual quality, 3DGS is positioned as a successor to Neural Radiance Fields (NeRF) and similar neural-based methods. Unlike traditional 3D modeling approaches based on meshes or point clouds, 3DGS abandons structural geometric components and instead adopts a set of continuously differentiable 3D Gaussian functions as its primitive rendering elements. This enables a flexible and unstructured way to represent complex scenes.

The core idea of 3DGS is to decompose a complete 3D scene into thousands of independent 3D Gaussian distributions, each acting as an atomic rendering unit. A single Gaussian carries spatial position and scale, as well as color, transparency (alpha), orientation, and anisotropic covariance attributes. These parameters collectively determine the visual contribution of the Gaussian to the final rendered image. When viewed from a specific perspective, the 3D Gaussians are projected onto a 2D image plane and composited through weighted blending, yielding a smooth and realistic final image. This process can be metaphorically compared to throwing countless colored, semi-transparent snowballs at a canvas—where their accumulation forms the 2D projection of a 3D world.

In contrast to implicit representations like NeRF, 3DGS is an explicit representation method. It avoids the need for dense volumetric integration through neural networks in every frame, significantly improving runtime performance. This makes 3DGS especially suitable for real-time rendering scenarios such as VR, AR, digital twins, and interactive content generation. Moreover, each Gaussian function, being differentiable, integrates seamlessly into deep learning frameworks, enabling gradient-based scene optimization and editing.

3DGS is also highly scalable. During modeling, the number, density, and dimensionality of Gaussians can be flexibly adjusted to achieve multiresolution representations—from coarse approximations to finely detailed reconstructions. This adaptability has enabled early applications in VFX production, cultural heritage preservation, intelligent manufacturing, and urban digitalization.

Unlike traditional mesh or point cloud methods, 3DGS leverages the continuity and differentiability of Gaussians to capture scene geometry and appearance in an unstructured way. Each Gaussian contains the following core attributes:

- **Position (Mean, μ)**: Specifies the center of the Gaussian in 3D space.
- **Covariance (Σ)**: A 3x3 matrix describing the shape and orientation of the Gaussian, allowing ellipsoidal forms via eigen-decomposition—key for capturing thin structures and geometric detail.
- **Color**: Encoded typically as RGB values, possibly extended with spherical harmonics (SH) to model view-dependent lighting effects.
- **Opacity (α)**: Controls the transparency of the Gaussian. During rendering, Gaussians with varying opacities are blended along the view ray to synthesize depth and translucency.

### Model Construction

To construct a 3DGS model, a multi-view image dataset is used. Classic Structure from Motion (SfM) techniques recover camera poses and a sparse 3D point cloud. Each sparse point is then initialized as a Gaussian, typically with isotropic covariance, color, and opacity. These serve as the atomic units for further optimization.

Next, a differentiable rendering pipeline is employed: from each training view, all Gaussians are projected onto the 2D image plane to form 2D Gaussians. These are sorted by depth and blended using alpha compositing to generate the rendered image.

The rendered image is compared to the reference photo using loss functions such as L1 loss and Structural Similarity Index (SSIM). Gradients of the loss are backpropagated to update each Gaussian's position, covariance, color, and opacity. Iteratively, the Gaussians converge to a configuration that accurately represents local geometry and appearance.

To improve quality and compactness, adaptive density control is applied. Gaussians in high-error regions are densified (duplicated or split), while low-contribution Gaussians are pruned to reduce redundancy and enhance performance.

At inference time, no neural networks are needed. Optimized Gaussians are directly projected and blended to render new views in real time—making 3DGS significantly faster and easier to integrate into traditional graphics pipelines.

## Characteristics and Analysis

3DGS has quickly become a research hotspot due to its compelling characteristics in rendering speed, expressive power, and flexibility. These features not only enhance the efficiency and quality of 3D reconstruction but also open new possibilities for AR/VR, digital twins, and robotics.

1. **High Rendering Speed**: 3DGS is revolutionary in rendering performance. Its rasterization-based pipeline projects explicit Gaussian primitives directly onto the image plane without volumetric integration. This process is highly parallelizable and achieves real-time framerates (60FPS+) on modern GPUs, balancing speed with visual fidelity.
2. **Strong Expressiveness**: 3DGS excels in capturing geometry, texture, color, transparency, and view-dependent effects like specular highlights. Anisotropic covariance enables accurate modeling of fine structures such as hair and foliage, while spherical harmonics support lighting effects. It surpasses point cloud and voxel methods in representing transparent and thin materials.
3. **Editability**: With each Gaussian represented explicitly and independently, scene components can be directly modified. Though editing thousands of Gaussians remains nontrivial, it is far more accessible than manipulating dense neural fields. Tools have emerged for local editing—color adjustment, structure refinement, or object removal.
4. **SLAM Compatibility**: The speed and adaptability of 3DGS make it ideal for real-time SLAM systems. It can incrementally build maps while simultaneously rendering them. Techniques like Gaussian Splatting SLAM and SplaTAM show strong performance in monocular and RGB-D settings, supporting navigation and AR interactions.
5. **High Visual Fidelity**: Trained 3DGS models generate detailed, photorealistic images, handling reflections, lighting, and transparency well. Compared to NeRF, outputs have sharper edges, smoother gradients, and more consistent lighting, making 3DGS ideal for VFX, product visualization, and digital asset creation.
6. **Fast Optimization**: 3DGS relies on gradient-based parameter updates rather than training deep networks. Initialized from sparse point clouds, its optimization converges faster—often within minutes—significantly reducing modeling time versus NeRFs, which can take hours or days.

Overall, 3DGS represents a paradigm shift from offline modeling to interactive reconstruction. Its hybrid nature—combining graphics principles with learnable parameters—makes it well-suited for research and deployment across a wide range of domains.

## Academic and Commercial Value

3DGS offers substantial academic and commercial value as a bridge between computer graphics, computer vision, and machine learning. It redefines how 3D content is modeled, optimized, and visualized.

### Academic Value

As a novel explicit representation, 3DGS opens new research directions beyond neural implicit fields:

- **Dynamic Scene Reconstruction**: Efficiently captures time-varying elements such as motion, fluids, and deformable objects—beneficial for film, animation, and immersive media.
- **Geometric Editing**: Gaussian parameters support local and semantic editing, fostering research in controllable and interactive 3D content generation.
- **Physics-Based Simulation**: Integrating with physics engines, 3DGS can visualize heat, lighting, or impact simulations. Its differentiability supports inverse simulation and real-time visualization.

Since winning the **Best Paper Award at SIGGRAPH 2023**, 3DGS has seen rising interest in top conferences (CVPR, ECCV, NeurIPS), with papers on optimization, representation design, and multimodal extensions.

Open-source implementations on GitHub, alongside resources on Hugging Face, Medium, and Papers with Code, further accelerate its adoption. The growing ecosystem marks a transition from research novelty to standard practice.

### Commercial Value

3DGS is also proving valuable across industries:

- **Entertainment**: Used in film VFX, game development, and virtual set creation. Teams can quickly build assets with high realism, reducing rendering costs.
- **Industry and Manufacturing**: Powers digital twins in design, architecture, and simulation. Compatible with BIM/CAD systems, it enables interactive, high-fidelity visualization.
- **Consumer Platforms**: Its efficiency enables deployment on mobile platforms (e.g., Android). Unity and Unreal plugins extend its reach into commercial development workflows.

## Applications and Use Cases

3DGS has demonstrated value in various application domains:

- **Film and VFX**: Models dynamic elements like motion, cloth, fire, and water for post-production.
- **Game Engines**: Integrated into Unity and Unreal for immersive scene generation and rendering.
- **AR/VR**: Enhances realism and performance in interactive environments with SLAM integration.
- **Architecture and Simulation**: Enables intuitive modeling and visualization for planning and analysis.
- **Robotics and Perception**: Supports real-time tracking and mapping for navigation and environment understanding.

| **Domain**       | **Use Case**                                                 |
| ---------------- | ------------------------------------------------------------ |
| Film & VFX       | Dynamic asset modeling and visual effects (e.g., fluids, cloth) |
| Game Development | Immersive scene rendering via engine integration             |
| AR/VR            | Real-time reconstruction and SLAM-enhanced visualization     |
| Digital Twins    | Urban planning and architectural visualization               |
| Robotics & SLAM  | High-quality real-time mapping and localization              |

## Conclusion

3D Gaussian Splatting offers a powerful new approach to 3D modeling and rendering. Its explicit representation, high performance, and compatibility with real-time systems make it a compelling alternative to neural implicit fields like NeRF. With continued research and tool development, 3DGS is poised to transform 3D graphics and vision workflows—bridging the gap between academic exploration and industrial deployment. It stands as a methodological innovation and a practical technology foundation for future interactive and intelligent 3D systems.
